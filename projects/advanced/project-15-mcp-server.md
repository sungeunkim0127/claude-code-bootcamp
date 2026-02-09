# Project 15: Build Custom MCP Server

**Level**: Advanced
**Time**: 6-8 hours
**Prerequisites**: Completed Lessons 51-62 (MCP section)

---

## Overview

Build a Model Context Protocol (MCP) server that connects Claude Code to the OpenWeatherMap API. You will create a production-quality server with typed tool definitions, in-memory caching, rate limiting, retry logic, and comprehensive error handling. By the end, Claude can fetch weather data, search forecasts, and check API health through natural conversation.

OpenWeatherMap has a generous free tier (1,000 calls/day), predictable JSON, and simple API key auth -- ideal for learning MCP server development.

---

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                       Claude Code                            │
│  "What is the weather in Tokyo?" --> calls get_weather tool  │
└────────────────────────────┬─────────────────────────────────┘
                             │ MCP Protocol (stdio)
                             ▼
┌──────────────────────────────────────────────────────────────┐
│                   MCP Weather Server                         │
│  ┌──────────┐  ┌──────────────┐  ┌────────────────────────┐ │
│  │  Tools    │  │  Middleware   │  │  API Client            │ │
│  │get_weather│  │ Cache Layer  │  │ OpenWeatherMap REST    │ │
│  │search_   │──│ Rate Limiter │──│ Authentication         │ │
│  │ forecast │  │ Retry Logic  │  │ Response Parsing       │ │
│  │get_api_  │  │              │  │                        │ │
│  │ status   │  │              │  │                        │ │
│  └──────────┘  └──────────────┘  └────────────────────────┘ │
└──────────────────────────────────────────────────────────────┘
                             │
                             ▼
                ┌──────────────────────┐
                │  OpenWeatherMap API   │
                └──────────────────────┘
```

### Project Structure

```
weather-mcp-server/
├── src/
│   ├── index.ts           # Entry point
│   ├── server.ts          # MCP server + tool registration
│   ├── services/
│   │   └── weatherApi.ts  # OpenWeatherMap API client
│   ├── middleware/
│   │   ├── cache.ts       # In-memory cache with TTL
│   │   └── rateLimiter.ts # Sliding-window rate limiter
│   ├── utils/
│   │   └── retry.ts       # Exponential backoff retry
│   └── types.ts           # Shared TypeScript interfaces
├── tests/
│   ├── middleware/         # Cache + rate limiter tests
│   ├── tools/             # API client tests
│   └── integration/       # End-to-end server tests
├── package.json
├── tsconfig.json
└── .env.example
```

---

## Phase 1: Project Setup & Design (1 hour)

### Task 1.1: Initialize the Project

```bash
mkdir weather-mcp-server && cd weather-mcp-server
npm init -y
npm install @modelcontextprotocol/sdk zod
npm install -D typescript @types/node vitest tsx
```

### Task 1.2: Configure TypeScript and package.json

Create `tsconfig.json` with strict mode, ES2022 target, and Node16 module resolution. Set `"type": "module"` in `package.json` and add scripts for `build` (tsc), `dev` (tsx), `test` (vitest run), and `start` (node dist/index.js).

### Task 1.3: Define Shared Types

Create `src/types.ts`:

```typescript
export interface WeatherData {
  city: string; country: string;
  temperature: { current: number; feelsLike: number; min: number; max: number };
  conditions: { main: string; description: string; icon: string };
  wind: { speed: number; direction: number };
  humidity: number; visibility: number; timestamp: string;
}

export interface ForecastEntry {
  datetime: string;
  temperature: { current: number; min: number; max: number };
  conditions: { main: string; description: string };
  precipitation: number;
  wind: { speed: number; direction: number };
}

export interface ForecastData { city: string; country: string; entries: ForecastEntry[] }

export interface ApiStatusData {
  operational: boolean; responseTimeMs: number;
  rateLimitRemaining: number;
  cacheStats: { entries: number; hitRate: number };
}

export interface CacheEntry<T> { data: T; expiresAt: number }

export interface ServerConfig {
  apiKey: string; cacheTtlSeconds: number;
  rateLimitPerMinute: number; baseUrl: string;
}
```

---

## Phase 2: Core Server Implementation (2 hours)

### Task 2.1: Build the API Client

Create `src/services/weatherApi.ts`. This class handles all OpenWeatherMap communication:

```typescript
import { WeatherData, ForecastData, ServerConfig } from "../types.js";

export class WeatherApiClient {
  constructor(private config: ServerConfig) {}

  async getCurrentWeather(city: string, units = "metric"): Promise<WeatherData> {
    const url = new URL(`${this.config.baseUrl}/weather`);
    url.searchParams.set("q", city);
    url.searchParams.set("units", units);
    url.searchParams.set("appid", this.config.apiKey);

    const response = await fetch(url.toString());
    if (!response.ok) {
      if (response.status === 401) throw new Error("Invalid API key.");
      if (response.status === 404) throw new Error(`City not found: "${city}".`);
      if (response.status === 429) throw new Error("Rate limit exceeded.");
      throw new Error(`API error ${response.status}: ${await response.text()}`);
    }

    const raw = await response.json() as Record<string, any>;
    return {
      city: raw.name, country: raw.sys.country,
      temperature: { current: raw.main.temp, feelsLike: raw.main.feels_like,
                     min: raw.main.temp_min, max: raw.main.temp_max },
      conditions: { main: raw.weather[0].main,
                    description: raw.weather[0].description, icon: raw.weather[0].icon },
      wind: { speed: raw.wind.speed, direction: raw.wind.deg },
      humidity: raw.main.humidity, visibility: raw.visibility,
      timestamp: new Date(raw.dt * 1000).toISOString(),
    };
  }

  async getForecast(city: string, days = 5, units = "metric"): Promise<ForecastData> {
    // Similar pattern: build URL, fetch, handle errors, parse response
    // Free tier returns 5-day/3-hour forecasts; slice to days * 8 entries
    const url = new URL(`${this.config.baseUrl}/forecast`);
    url.searchParams.set("q", city);
    url.searchParams.set("units", units);
    url.searchParams.set("appid", this.config.apiKey);

    const response = await fetch(url.toString());
    if (!response.ok) {
      if (response.status === 404) throw new Error(`City not found: "${city}".`);
      throw new Error(`API error ${response.status}: ${await response.text()}`);
    }
    const raw = await response.json() as Record<string, any>;
    const entries = raw.list.slice(0, days * 8).map((item: any) => ({
      datetime: item.dt_txt,
      temperature: { current: item.main.temp, min: item.main.temp_min, max: item.main.temp_max },
      conditions: { main: item.weather[0].main, description: item.weather[0].description },
      precipitation: item.pop ?? 0,
      wind: { speed: item.wind.speed, direction: item.wind.deg },
    }));
    return { city: raw.city.name, country: raw.city.country, entries };
  }

  async healthCheck(): Promise<{ ok: boolean; responseTimeMs: number }> {
    const start = Date.now();
    try {
      const url = new URL(`${this.config.baseUrl}/weather`);
      url.searchParams.set("q", "London");
      url.searchParams.set("appid", this.config.apiKey);
      const r = await fetch(url.toString());
      return { ok: r.ok, responseTimeMs: Date.now() - start };
    } catch { return { ok: false, responseTimeMs: Date.now() - start }; }
  }
}
```

### Task 2.2: Create the MCP Server with Tool Registration

Create `src/server.ts` -- the heart of the project:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { z } from "zod";
import { WeatherApiClient } from "./services/weatherApi.js";
import { Cache } from "./middleware/cache.js";
import { RateLimiter } from "./middleware/rateLimiter.js";
import { withRetry } from "./utils/retry.js";
import { ServerConfig, WeatherData, ForecastData } from "./types.js";

export function createWeatherServer(config: ServerConfig): McpServer {
  const server = new McpServer({ name: "weather-server", version: "1.0.0" });
  const api = new WeatherApiClient(config);
  const cache = new Cache(config.cacheTtlSeconds);
  const limiter = new RateLimiter(config.rateLimitPerMinute);

  server.tool("get_weather", "Get current weather for a city.", {
    city: z.string().describe("City name, e.g. 'Tokyo'"),
    units: z.enum(["metric", "imperial"]).default("metric"),
  }, async ({ city, units }) => {
    const key = `weather:${city.toLowerCase()}:${units}`;
    const cached = cache.get<WeatherData>(key);
    if (cached) return { content: [{ type: "text" as const, text: fmtWeather(cached, true) }] };
    if (!limiter.allowRequest()) return { content: [{ type: "text" as const, text: "Rate limit reached." }] };
    const data = await withRetry(() => api.getCurrentWeather(city, units));
    cache.set(key, data);
    return { content: [{ type: "text" as const, text: fmtWeather(data, false) }] };
  });

  server.tool("search_forecast", "Get multi-day forecast (up to 5 days).", {
    city: z.string(), days: z.number().int().min(1).max(5).default(3),
    units: z.enum(["metric", "imperial"]).default("metric"),
  }, async ({ city, days, units }) => {
    const key = `forecast:${city.toLowerCase()}:${days}:${units}`;
    const cached = cache.get<ForecastData>(key);
    if (cached) return { content: [{ type: "text" as const, text: fmtForecast(cached, true) }] };
    if (!limiter.allowRequest()) return { content: [{ type: "text" as const, text: "Rate limit reached." }] };
    const data = await withRetry(() => api.getForecast(city, days, units));
    cache.set(key, data);
    return { content: [{ type: "text" as const, text: fmtForecast(data, false) }] };
  });

  server.tool("get_api_status", "Check API health and cache stats.", {}, async () => {
    const h = await api.healthCheck();
    const s = cache.getStats();
    const text = [`API Status`, `Operational: ${h.ok ? "Yes" : "No"}`,
      `Response time: ${h.responseTimeMs}ms`, `Rate limit remaining: ${limiter.remaining()}`,
      `Cache: ${s.entries} entries, ${Math.round(s.hitRate * 100)}% hit rate`].join("\n");
    return { content: [{ type: "text" as const, text }] };
  });

  return server;
}

function fmtWeather(d: WeatherData, cached: boolean): string {
  const tag = cached ? " (cached)" : "";
  return [`Weather for ${d.city}, ${d.country}${tag}`,
    `Temp: ${d.temperature.current} (feels like ${d.temperature.feelsLike})`,
    `${d.conditions.main}: ${d.conditions.description}`,
    `Humidity: ${d.humidity}% | Wind: ${d.wind.speed} m/s`].join("\n");
}

function fmtForecast(d: ForecastData, cached: boolean): string {
  const tag = cached ? " (cached)" : "";
  const rows = d.entries.map(e =>
    `[${e.datetime}] ${e.temperature.current} | ${e.conditions.description} | Precip: ${Math.round(e.precipitation * 100)}%`);
  return [`Forecast for ${d.city}, ${d.country}${tag}`, ...rows].join("\n");
}
```

### Task 2.3: Create the Entry Point

Create `src/index.ts`:

```typescript
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { createWeatherServer } from "./server.js";

const apiKey = process.env.OPENWEATHER_API_KEY;
if (!apiKey) { console.error("OPENWEATHER_API_KEY required."); process.exit(1); }

const server = createWeatherServer({
  apiKey,
  cacheTtlSeconds: parseInt(process.env.CACHE_TTL_SECONDS ?? "300", 10),
  rateLimitPerMinute: parseInt(process.env.RATE_LIMIT_PER_MINUTE ?? "30", 10),
  baseUrl: "https://api.openweathermap.org/data/2.5",
});
server.connect(new StdioServerTransport()).catch(e => { console.error(e); process.exit(1); });
```

---

## Phase 3: Caching & Error Handling (1.5 hours)

### Task 3.1: In-Memory Cache with TTL

Create `src/middleware/cache.ts`:

```typescript
import { CacheEntry } from "../types.js";

export class Cache {
  private store = new Map<string, CacheEntry<unknown>>();
  private ttlMs: number;
  private hits = 0; private misses = 0;

  constructor(ttlSeconds: number) {
    this.ttlMs = ttlSeconds * 1000;
    const t = setInterval(() => this.evictExpired(), 60_000);
    if (t.unref) t.unref();
  }

  get<T>(key: string): T | null {
    const e = this.store.get(key);
    if (!e) { this.misses++; return null; }
    if (Date.now() > e.expiresAt) { this.store.delete(key); this.misses++; return null; }
    this.hits++; return e.data as T;
  }

  set<T>(key: string, data: T): void {
    this.store.set(key, { data, expiresAt: Date.now() + this.ttlMs });
  }

  invalidate(key: string) { return this.store.delete(key); }
  clear() { this.store.clear(); this.hits = 0; this.misses = 0; }

  getStats() {
    const total = this.hits + this.misses;
    return { entries: this.store.size, hitRate: total ? this.hits / total : 0,
             hits: this.hits, misses: this.misses };
  }

  private evictExpired() {
    const now = Date.now();
    for (const [k, e] of this.store) if (now > e.expiresAt) this.store.delete(k);
  }
}
```

### Task 3.2: Sliding-Window Rate Limiter

Create `src/middleware/rateLimiter.ts`:

```typescript
export class RateLimiter {
  private timestamps: number[] = [];
  constructor(private max: number, private windowMs = 60_000) {}

  allowRequest(): boolean {
    const now = Date.now();
    this.timestamps = this.timestamps.filter(t => now - t < this.windowMs);
    if (this.timestamps.length >= this.max) return false;
    this.timestamps.push(now); return true;
  }

  remaining(): number {
    this.timestamps = this.timestamps.filter(t => Date.now() - t < this.windowMs);
    return Math.max(0, this.max - this.timestamps.length);
  }

  reset() { this.timestamps = []; }
}
```

### Task 3.3: Retry with Exponential Backoff

Create `src/utils/retry.ts`:

```typescript
export async function withRetry<T>(fn: () => Promise<T>, maxAttempts = 3): Promise<T> {
  let lastErr: Error | undefined;
  for (let i = 1; i <= maxAttempts; i++) {
    try { return await fn(); }
    catch (err) {
      lastErr = err instanceof Error ? err : new Error(String(err));
      if (/not found|invalid api key/i.test(lastErr.message)) throw lastErr; // no retry on 4xx
      if (i < maxAttempts) {
        const delay = Math.min(1000 * 2 ** (i - 1), 10000);
        await new Promise(r => setTimeout(r, delay + delay * 0.25 * (Math.random() * 2 - 1)));
      }
    }
  }
  throw lastErr ?? new Error("Retry failed");
}
```

Run `npx tsc --noEmit` to verify everything compiles. Common issue: missing `.js` extensions on relative imports.

---

## Phase 4: Testing (1.5 hours)

### Task 4.1: Configure Vitest

Create `vitest.config.ts` with `globals: true`, `environment: "node"`, and `include: ["tests/**/*.test.ts"]`.

### Task 4.2: Cache Unit Tests (`tests/middleware/cache.test.ts`)

Write 6 tests covering:
- Returns null for a cache miss
- Stores and retrieves values
- Expires entries after TTL (use `vi.useFakeTimers()` / `vi.advanceTimersByTime()`)
- Tracks hit/miss statistics and computes hit rate
- Clears all entries and resets stats
- Invalidates a specific key without affecting others

### Task 4.3: Rate Limiter Tests (`tests/middleware/rateLimiter.test.ts`)

Write 5 tests covering:
- Allows requests within the limit
- Rejects requests exceeding the limit
- Reports remaining requests correctly
- Allows requests after the sliding window passes (fake timers)
- Resets tracked timestamps

### Task 4.4: API Client Tests (`tests/tools/getWeather.test.ts`)

Mock `global.fetch` with `vi.fn()` and write 5 tests:

```typescript
const mockFetch = vi.fn();
global.fetch = mockFetch;

// Test: parses successful response into WeatherData
// Test: throws "City not found" for 404
// Test: throws "Invalid API key" for 401
// Test: throws "Rate limit exceeded" for 429
// Test: passes units parameter correctly in URL
```

### Task 4.5: Integration Tests (`tests/integration/server.test.ts`)

Use the MCP SDK's `InMemoryTransport` and `Client` to test the full server:

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { InMemoryTransport } from "@modelcontextprotocol/sdk/inMemory.js";
import { createWeatherServer } from "../../src/server.js";

// In beforeEach: create server, link transports, connect client
const [clientTransport, serverTransport] = InMemoryTransport.createLinkedPair();
const server = createWeatherServer(config);
await server.connect(serverTransport);
const client = new Client({ name: "test", version: "1.0.0" });
await client.connect(clientTransport);

// Test: listTools() returns all three tool names
// Test: callTool("get_weather") returns formatted weather text
// Test: repeated calls return "(cached)" and fetch is called only once
// Test: callTool("get_api_status") returns status text
```

### Task 4.6: Run the Suite

```bash
npx vitest run
```

Expected: 20 passing tests across 4 files. If tests fail, check mock setup (`global.fetch`), import paths (`.js` extensions), and timer cleanup (`vi.useRealTimers()`).

---

## Phase 5: Documentation & Integration (1 hour)

### Task 5.1: Configure Claude Code

Add to `~/.claude/settings.json` (global) or `.claude/settings.json` (project-level):

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["/absolute/path/to/weather-mcp-server/dist/index.js"],
      "env": { "OPENWEATHER_API_KEY": "your_key_here" }
    }
  }
}
```

### Task 5.2: Verify the Integration

Restart Claude Code and run through these checks:

1. **Tool discovery**: Ask "What MCP tools are available?" -- confirm all three appear
2. **get_weather**: Ask "What is the weather in New York?" -- returns formatted conditions
3. **search_forecast**: Ask "3-day forecast for Berlin" -- returns multi-day data
4. **get_api_status**: Ask "Check weather API status" -- shows health and cache stats
5. **Caching**: Ask the same question twice -- second response shows "(cached)"
6. **Error handling**: Ask "Weather in Xyzzyplugh?" -- returns "City not found" gracefully

### Task 5.3: Troubleshooting

| Problem | Fix |
|---|---|
| Tools not listed | Verify absolute path, run `npm run build`, check env block |
| "Invalid API key" | New keys take up to 2 hours to activate |
| Tools return errors | Check network, API status page, daily limit (1,000/day) |

---

## Deliverables

1. **MCP server** with 3 tools: `get_weather`, `search_forecast`, `get_api_status`
2. **API client** with authentication and response parsing
3. **Cache middleware** with TTL expiry and hit-rate tracking
4. **Rate limiter** using sliding-window algorithm
5. **Retry logic** with exponential backoff and jitter
6. **Test suite** with 20+ tests (unit, integration, error scenarios)
7. **Claude Code integration** verified with real queries

---

## Success Criteria

### Functionality
- [ ] Server starts without errors when `OPENWEATHER_API_KEY` is set
- [ ] `get_weather` returns current conditions for valid cities
- [ ] `search_forecast` returns multi-day forecasts with 3-hour intervals
- [ ] `get_api_status` reports health, response time, and cache stats
- [ ] Invalid cities return clear "City not found" messages
- [ ] Missing API key produces a helpful startup error

### Caching & Performance
- [ ] Repeated requests return cached data (verify "(cached)" marker)
- [ ] Cache entries expire after configured TTL
- [ ] Statistics track hits, misses, and hit rate accurately
- [ ] Expired entries are evicted (no unbounded growth)

### Rate Limiting & Resilience
- [ ] Rate limiter blocks requests exceeding per-minute threshold
- [ ] Limiter recovers after the sliding window passes
- [ ] Retry retries on 5xx/network errors, not on 4xx client errors
- [ ] Backoff uses exponential delay with jitter

### Testing
- [ ] All tests pass (`npx vitest run`)
- [ ] Cache: miss, hit, expiry, stats, clear, invalidate
- [ ] Rate limiter: within limit, exceeded, window slide, reset
- [ ] API client: success, 404, 401, 429, parameter passing
- [ ] Integration: tool registration, end-to-end calls, caching

### Integration
- [ ] Server configured in Claude Code settings
- [ ] Claude Code recognizes all three weather tools
- [ ] Natural language queries trigger the correct tools

---

## Enhancement Ideas

Once the base project is complete, consider these extensions:

**MCP Resources**: Expose a `weather://favorites` resource for frequently checked cities so Claude can read weather data without an explicit tool call.

**MCP Prompts**: Register a `travel-weather` prompt that asks Claude to check forecasts and advise on what to pack.

**Persistent Cache**: Replace the in-memory cache with SQLite so data survives server restarts.

**Multi-Provider Fallback**: Add WeatherAPI.com as a backup when OpenWeatherMap is down.

---

**Estimated time**: 6-8 hours
**Difficulty**: Advanced
**Skills practiced**: MCP server development, TypeScript, API integration, caching, rate limiting, testing, Claude Code configuration
