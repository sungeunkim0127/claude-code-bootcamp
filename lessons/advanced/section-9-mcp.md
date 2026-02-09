# Section 9: MCP - Model Context Protocol (Lessons 51-62)

## Overview
Master the Model Context Protocol (MCP) to extend Claude Code with external data sources, APIs, and services. MCP is the standard protocol for connecting AI assistants to the outside world.

---

## Lesson 51: MCP Overview

### Objectives
- Understand what MCP is and why it exists
- Learn about MCP server types
- Know common use cases
- Understand the MCP architecture

### Content

#### What is MCP?

The **Model Context Protocol (MCP)** is an open standard for connecting AI assistants to external tools and data sources. It lets Claude interact with services beyond its built-in capabilities.

**Without MCP**:
```
Claude can: Read files, edit code, run commands
Claude cannot: Query your database, check GitHub issues,
              search Slack, access your APIs
```

**With MCP**:
```
Claude can: All of the above PLUS
- Query live databases
- Manage GitHub issues and PRs
- Search Slack messages
- Call your custom APIs
- Access any service with an MCP server
```

#### MCP Architecture

```
┌──────────────┐     MCP Protocol     ┌──────────────────┐
│  Claude Code │ ◄──────────────────► │   MCP Server     │
│  (Client)    │   JSON-RPC / stdio   │  (Your Service)  │
└──────────────┘                      └──────────────────┘
                                              │
                                              ▼
                                      ┌──────────────────┐
                                      │  External API    │
                                      │  Database        │
                                      │  File System     │
                                      └──────────────────┘
```

**Key Components**:
- **Client** (Claude Code): Discovers and calls MCP tools
- **Server**: Exposes tools, resources, and prompts
- **Transport**: Communication layer (stdio, HTTP, SSE)

#### What MCP Servers Provide

**1. Tools** — Functions Claude can call:
```
github.create_issue(title, body)
postgres.query(sql)
slack.send_message(channel, text)
```

**2. Resources** — Data Claude can access via @-mentions:
```
@github:issue:123
@confluence:doc:456
@slack:channel:general
```

**3. Prompts** — Reusable prompt templates:
```
/github:summarize-pr 42
/postgres:explain-query "SELECT..."
```

#### Common Use Cases

| Use Case | MCP Server | What It Enables |
|----------|-----------|-----------------|
| GitHub management | GitHub server | Issues, PRs, repos |
| Database queries | Postgres/MySQL server | Direct SQL queries |
| Documentation | Confluence/Notion server | Read/search docs |
| Communication | Slack server | Read/send messages |
| File browsing | Filesystem server | Access remote files |
| API integration | Custom server | Any REST/GraphQL API |

### Exercise

**Task**: Understand MCP concepts

1. List 5 services you use daily that could benefit from MCP integration
2. For each, identify what tools, resources, and prompts would be useful
3. Rank them by how much time the integration would save

### Verification

Before moving on, ensure you understand:
- [ ] What MCP is and its purpose
- [ ] The client-server architecture
- [ ] The three MCP primitives (tools, resources, prompts)
- [ ] Common use cases for MCP

---

## Lesson 52: Installing MCP Servers

### Objectives
- Configure MCP servers in settings
- Install servers from npm packages
- Manage server lifecycle
- Troubleshoot installation issues

### Content

#### MCP Server Configuration

MCP servers are configured in your Claude Code settings files:

**User-level** (`~/.claude/settings.json`):
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

**Project-level** (`.claude/settings.json`):
```json
{
  "mcpServers": {
    "project-db": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost:5432/mydb"
      }
    }
  }
}
```

#### Configuration Fields

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",           // Command to start the server
      "args": ["-y", "package"],  // Arguments for the command
      "env": {                    // Environment variables
        "API_KEY": "value"
      },
      "cwd": "/path/to/dir"      // Working directory (optional)
    }
  }
}
```

#### Installing Popular Servers

**GitHub Server**:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

**Filesystem Server**:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y", "@modelcontextprotocol/server-filesystem",
        "/path/to/allowed/directory"
      ]
    }
  }
}
```

**PostgreSQL Server**:
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/dbname"
      }
    }
  }
}
```

#### Verifying Installation

After adding an MCP server to your settings:

```
You: "What MCP servers are connected?"

Claude: Lists available MCP servers and their tools
```

```
You: "What tools does the GitHub MCP server provide?"

Claude: Lists all GitHub tools (create_issue, list_repos, etc.)
```

#### Troubleshooting

**Server Won't Start**:
```
1. Check that the command exists (npx, node, python)
2. Verify the package name is correct
3. Check environment variables are set
4. Try running the command manually in terminal
```

**Authentication Errors**:
```
1. Verify tokens/keys are valid
2. Check token has required permissions
3. Ensure env variables are in the right settings file
4. Tokens in project settings should NOT be committed to git
```

**Server Crashes**:
```
1. Check server logs (stderr output)
2. Verify the server version compatibility
3. Try reinstalling: delete node_modules/.cache and retry
4. Check for port conflicts if using HTTP transport
```

### Exercise

**Task**: Install your first MCP server

1. **Create Settings File**:
```bash
mkdir -p ~/.claude
```

2. **Add Filesystem Server** (safest to start with):
```json
// Add to ~/.claude/settings.json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y", "@modelcontextprotocol/server-filesystem",
        "/tmp/mcp-test"
      ]
    }
  }
}
```

3. **Create Test Directory**:
```bash
mkdir -p /tmp/mcp-test
echo "Hello MCP!" > /tmp/mcp-test/test.txt
```

4. **Verify in Claude**:
```
claude
"What MCP servers are available?"
"Read the file test.txt using the filesystem MCP server"
```

### Verification

Before moving on, ensure you can:
- [ ] Add MCP servers to settings.json
- [ ] Configure environment variables
- [ ] Verify server installation
- [ ] Troubleshoot common issues

---

## Lesson 53: Using MCP Tools

### Objectives
- Discover available MCP tools
- Call MCP tools naturally
- Understand tool parameters
- Handle tool responses

### Content

#### Tool Discovery

Claude automatically discovers tools from connected MCP servers:

```
You: "What tools are available from the GitHub server?"

Claude: "The GitHub MCP server provides these tools:
- github_create_issue: Create a new issue
- github_list_issues: List repository issues
- github_create_pr: Create a pull request
- github_search_repos: Search repositories
- ..."
```

#### Calling MCP Tools

You don't need to know the exact tool names. Just describe what you want:

**Natural Language**:
```
You: "Create a GitHub issue for the login bug"

Claude:
  [MCP Tool: github_create_issue]
  → Created issue #45: "Login bug: 500 error on invalid password"
```

**With Details**:
```
You: "Create a GitHub issue in repo 'myorg/myapp' titled
'Fix pagination' with a description of the bug"

Claude:
  [MCP Tool: github_create_issue(
    repo: "myorg/myapp",
    title: "Fix pagination",
    body: "The pagination on /users endpoint..."
  )]
  → Created issue #46
```

#### Common MCP Tool Patterns

**GitHub Tools**:
```
"List all open issues in myorg/myapp"
"Search for PRs by @alice"
"Create a new branch 'feature/search' in the repo"
"Close issue #42 with a comment"
```

**Database Tools**:
```
"Show me all tables in the database"
"Query for users who signed up in the last 7 days"
"Explain the query plan for this slow query"
"Show me the schema of the users table"
```

**Filesystem Tools**:
```
"List files in /data/exports"
"Read the latest log file"
"Search for 'error' in all log files"
```

#### Understanding Tool Responses

MCP tools return structured data that Claude interprets:

```
You: "How many open issues are there?"

Claude:
  [MCP Tool: github_list_issues(state: "open")]
  → Returns JSON array of issues

  "There are 23 open issues. The 5 most recent are:
  1. #89 - Fix login timeout (opened yesterday)
  2. #88 - Update documentation (opened 2 days ago)
  ..."
```

#### Error Handling

When MCP tools fail:

```
You: "Query the database for user stats"

Claude:
  [MCP Tool: postgres_query(...)]
  → Error: Connection refused

  "The database MCP server returned an error:
  'Connection refused'. This usually means:
  1. The database server isn't running
  2. The connection URL in settings is incorrect
  3. Firewall is blocking the connection

  Would you like me to help troubleshoot?"
```

### Exercise

**Task**: Use MCP tools (with any installed server)

If you have a GitHub token:
```
"List my most recent repositories"
"Show open issues in [your-repo]"
"Create a test issue in [your-test-repo]"
```

If using filesystem server:
```
"List all files in the MCP-accessible directory"
"Read the contents of test.txt"
"Create a new file called hello.txt with 'Hello from MCP!'"
```

### Verification

Before moving on, ensure you can:
- [ ] Discover tools from MCP servers
- [ ] Call tools using natural language
- [ ] Understand tool responses
- [ ] Handle tool errors

---

## Lesson 54: MCP Resources and @-Mentions

### Objectives
- Understand MCP resources
- Use @-mentions to reference MCP data
- Access remote content inline
- Combine resources with tools

### Content

#### What Are MCP Resources?

Resources are data sources exposed by MCP servers that Claude can reference directly. Think of them as remote files or data that Claude can read.

**Examples**:
```
@github:issue:123       → Contents of GitHub issue #123
@postgres:table:users   → Schema of the users table
@slack:channel:general  → Recent messages from #general
```

#### Using @-Mentions for Resources

**Reference in Conversation**:
```
You: "Summarize @github:issue:123"

Claude:
  [Fetches issue #123 via MCP]
  "Issue #123 'Fix login timeout' was opened by @alice
  on Jan 15. It describes a 30-second timeout when..."
```

**Combine Resources**:
```
You: "Compare @github:issue:123 with our implementation
in src/auth.js"

Claude:
  [Fetches issue via MCP]
  [Reads src/auth.js locally]
  "The issue requests a 10-second timeout, but the
  current implementation uses 30 seconds..."
```

#### Resource Types

**Read-Only Resources** (most common):
- GitHub issues, PRs, files
- Database schemas and query results
- Documentation pages
- Slack messages

**Dynamic Resources** (computed on request):
- Database query results
- API response data
- Aggregated metrics

#### Benefits of Resources

**Inline Context**:
```
"Based on @github:pr:42, update the implementation in
src/search.js to match the approved design"
```

**Cross-Service References**:
```
"The bug described in @github:issue:99 might be related
to the discussion in @slack:thread:abc123"
```

### Exercise

**Task**: Use MCP resources

With GitHub server configured:
```
"Read @github:issue:1 from your test repository"
"Summarize the discussion on @github:pr:1"
```

**Practical workflow**:
```
"Read @github:issue:5, then implement the feature
it describes in src/feature.js"
```

### Verification

Before moving on, ensure you understand:
- [ ] What MCP resources are
- [ ] How to reference resources with @-mentions
- [ ] How to combine resources with local files
- [ ] When to use resources vs tools

---

## Lesson 55: MCP Prompts

### Objectives
- Understand MCP prompt templates
- Use server-provided prompts
- Combine prompts with tools
- Know when prompts add value

### Content

#### What Are MCP Prompts?

Prompts are reusable templates provided by MCP servers. They package common workflows into single commands.

**Example Prompts from GitHub Server**:
```
/github:summarize-pr 42     → Summarizes PR with key changes
/github:release-notes v2.0  → Generates release notes
/github:issue-triage         → Triages untagged issues
```

#### Using MCP Prompts

**Invoke a Prompt**:
```
You: /github:summarize-pr 42

Claude:
  [Executes prompt template]
  [Fetches PR data via MCP tools]
  "## PR #42 Summary
  **Title**: Add search functionality
  **Author**: @alice
  **Changes**: 5 files, +200/-30 lines
  **Key Changes**:
  - New search endpoint at /api/search
  - Elasticsearch integration
  - Search results pagination..."
```

#### Prompts vs Skills

| Aspect | MCP Prompts | Skills |
|--------|------------|--------|
| Source | MCP server | Local files |
| Scope | Server's domain | Any task |
| Data access | Server's data | Local files + tools |
| Installation | Comes with server | Created by you |

**Use Prompts When**: The MCP server provides a useful workflow for its domain.

**Use Skills When**: You need custom logic or cross-service workflows.

### Exercise

**Task**: Explore available MCP prompts

```
"What prompts are available from connected MCP servers?"
"Use the GitHub summarize-pr prompt on a recent PR"
```

### Verification

Before moving on, ensure you understand:
- [ ] What MCP prompts are
- [ ] How to invoke MCP prompts
- [ ] The difference between prompts and skills
- [ ] When prompts add value

---

## Lesson 56: MCP Authentication and Security

### Objectives
- Securely configure MCP credentials
- Understand token scoping
- Manage secrets safely
- Follow security best practices

### Content

#### Credential Management

**Environment Variables** (recommended):
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

**Security Rules**:
1. **Never commit tokens to git** — use user-level settings, not project settings
2. **Use minimal permissions** — create tokens with only needed scopes
3. **Rotate tokens regularly** — set calendar reminders
4. **Use separate tokens** — one per MCP server, easy to revoke

#### Token Scoping

**GitHub Token** — only grant what's needed:
```
repo (if you need repo access)
read:org (for organization data)
read:user (for user data)
```

**Database** — use read-only credentials for query servers:
```
CREATE USER mcp_reader WITH PASSWORD 'secure_password';
GRANT SELECT ON ALL TABLES TO mcp_reader;
-- No INSERT, UPDATE, or DELETE
```

#### Settings File Security

**User Settings** (`~/.claude/settings.json`):
- Contains your personal tokens
- Never committed to version control
- Secured by your OS file permissions

**Project Settings** (`.claude/settings.json`):
- Committed to git (shared with team)
- Should NOT contain secrets
- Use references to environment variables instead:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

#### Security Best Practices

1. **Principle of Least Privilege**: Give MCP servers minimum needed access
2. **Audit Regularly**: Review which servers have access to what
3. **Network Isolation**: Run MCP servers in sandboxed environments when possible
4. **Log Access**: Monitor what data MCP servers access
5. **Separate Environments**: Different tokens for dev/staging/production

### Exercise

**Task**: Set up secure MCP credentials

1. Create a GitHub personal access token with minimal scopes
2. Add it to user-level settings (not project settings)
3. Verify it works by listing your repositories
4. Document what permissions the token has

### Verification

Before moving on, ensure you can:
- [ ] Configure tokens securely in user settings
- [ ] Understand token scoping
- [ ] Keep secrets out of version control
- [ ] Follow the principle of least privilege

---

## Lesson 57: Popular MCP Servers

### Objectives
- Configure GitHub, Slack, and database servers
- Understand each server's capabilities
- Choose the right server for your needs
- Combine multiple servers

### Content

#### GitHub MCP Server

**Setup**:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

**Capabilities**:
- Create/read/update issues
- Create/review pull requests
- Search repositories and code
- Manage branches and releases
- Read file contents from repos

**Example Usage**:
```
"List all open bugs in myorg/backend"
"Create an issue for the login timeout bug"
"Show me the PR review comments on #42"
"Search for repositories about 'machine learning'"
```

#### PostgreSQL MCP Server

**Setup**:
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

**Capabilities**:
- Run SQL queries (SELECT)
- Inspect table schemas
- List databases and tables
- Explain query plans

**Example Usage**:
```
"Show me the schema of the users table"
"How many users signed up this week?"
"What's the slowest query hitting the orders table?"
"Show me all tables and their row counts"
```

#### Filesystem MCP Server

**Setup**:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y", "@modelcontextprotocol/server-filesystem",
        "/path/to/data"
      ]
    }
  }
}
```

**Capabilities**:
- Read/write files in allowed directories
- List directory contents
- Search file contents
- Create and delete files

**Use Case**: Access files outside your project directory (logs, data exports, shared drives).

#### Combining Multiple Servers

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "ghp_xxx" }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "DATABASE_URL": "postgresql://..." }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/data"]
    }
  }
}
```

**Cross-Server Workflows**:
```
"Check GitHub issue #42, then query the database to verify
the user count mentioned in the issue, and save the results
to a report file"
```

### Exercise

**Task**: Set up and use a popular MCP server

Choose one that matches your work:
1. **GitHub**: If you use GitHub regularly
2. **PostgreSQL**: If you have a local database
3. **Filesystem**: If you need access to external directories

Set it up, verify it works, and try 3 different operations.

### Verification

Before moving on, ensure you can:
- [ ] Configure at least one popular MCP server
- [ ] Use its tools naturally in conversation
- [ ] Understand its capabilities and limitations
- [ ] Combine with other tools and servers

---

## Lesson 58: MCP Scopes

### Objectives
- Understand user, project, and enterprise MCP scopes
- Configure server visibility
- Manage scope precedence
- Follow organizational policies

### Content

#### MCP Server Scopes

**User Scope** (`~/.claude/settings.json`):
- Available in all your projects
- Personal tokens and servers
- Not shared with team

**Project Scope** (`.claude/settings.json`):
- Available to everyone on the project
- Shared via git
- Project-specific services

**Enterprise Scope** (managed settings):
- Controlled by organization
- Applied to all team members
- Cannot be overridden

#### Scope Precedence

```
Enterprise settings  →  Highest priority (enforced)
User settings        →  Personal defaults
Project settings     →  Project-specific
```

If the same server name appears at multiple scopes, higher scopes win.

#### When to Use Each Scope

**User Scope**:
- Personal GitHub token
- Your private database access
- Experimental servers you're testing

**Project Scope**:
- Shared development database
- Project documentation service
- Team-specific APIs (without secrets in config)

**Enterprise Scope**:
- Approved server list
- Security scanning servers
- Compliance checking tools
- Restricted server configurations

### Exercise

**Task**: Configure MCP at different scopes

1. Add a server to user settings (personal use)
2. Add a server to project settings (team use)
3. Verify both are available in a Claude session
4. Test which takes precedence with the same name

### Verification

Before moving on, ensure you understand:
- [ ] The three MCP scopes
- [ ] When to use each scope
- [ ] How precedence works
- [ ] Security implications of each scope

---

## Lesson 59: Building Custom MCP Servers

### Objectives
- Understand the MCP server protocol
- Build a basic MCP server
- Implement tools, resources, and prompts
- Test your server with Claude

### Content

#### MCP Server Basics

An MCP server is a program that:
1. Communicates over stdio (stdin/stdout) or HTTP
2. Responds to JSON-RPC messages
3. Exposes tools, resources, and/or prompts

#### Minimal MCP Server (TypeScript)

**Setup**:
```bash
mkdir my-mcp-server && cd my-mcp-server
npm init -y
npm install @modelcontextprotocol/sdk
npm install -D typescript @types/node
```

**`src/index.ts`**:
```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server(
  { name: "my-server", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

// Define a tool
server.setRequestHandler("tools/list", async () => ({
  tools: [
    {
      name: "get_weather",
      description: "Get current weather for a city",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string", description: "City name" }
        },
        required: ["city"]
      }
    }
  ]
}));

// Handle tool calls
server.setRequestHandler("tools/call", async (request) => {
  if (request.params.name === "get_weather") {
    const city = request.params.arguments?.city;
    // In a real server, call a weather API here
    return {
      content: [
        {
          type: "text",
          text: `Weather in ${city}: 72°F, Sunny`
        }
      ]
    };
  }
  throw new Error(`Unknown tool: ${request.params.name}`);
});

// Start server
const transport = new StdioServerTransport();
await server.connect(transport);
```

**Build and Configure**:
```bash
npx tsc
```

```json
// In ~/.claude/settings.json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["path/to/my-mcp-server/dist/index.js"]
    }
  }
}
```

#### Adding Resources

```typescript
server.setRequestHandler("resources/list", async () => ({
  resources: [
    {
      uri: "weather://forecast/today",
      name: "Today's Forecast",
      description: "Current weather forecast"
    }
  ]
}));

server.setRequestHandler("resources/read", async (request) => {
  if (request.params.uri === "weather://forecast/today") {
    return {
      contents: [
        {
          uri: "weather://forecast/today",
          text: "Today: Sunny, high of 75°F, low of 55°F"
        }
      ]
    };
  }
  throw new Error(`Unknown resource: ${request.params.uri}`);
});
```

#### Adding Prompts

```typescript
server.setRequestHandler("prompts/list", async () => ({
  prompts: [
    {
      name: "weather-report",
      description: "Generate a weather report for a region",
      arguments: [
        { name: "region", description: "Geographic region", required: true }
      ]
    }
  ]
}));

server.setRequestHandler("prompts/get", async (request) => {
  if (request.params.name === "weather-report") {
    return {
      messages: [
        {
          role: "user",
          content: {
            type: "text",
            text: `Generate a detailed weather report for ${request.params.arguments?.region}`
          }
        }
      ]
    };
  }
  throw new Error(`Unknown prompt: ${request.params.name}`);
});
```

#### Testing Your Server

**Manual Test**:
```bash
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | node dist/index.js
```

**With Claude**:
```
claude
"What tools does my-server provide?"
"Get the weather for San Francisco"
```

### Exercise

**Task**: Build a simple custom MCP server

Create an MCP server that provides a single tool:
1. Choose a public API (jokes, quotes, trivia, etc.)
2. Create a tool that calls the API
3. Configure it in Claude settings
4. Test it in a Claude session

### Verification

Before moving on, ensure you can:
- [ ] Create a basic MCP server with the SDK
- [ ] Implement at least one tool
- [ ] Configure the server in settings
- [ ] Test the server with Claude

---

## Lesson 60: MCP Error Handling and Caching

### Objectives
- Handle errors gracefully in MCP servers
- Implement caching for API responses
- Manage rate limits
- Build resilient servers

### Content

#### Error Handling

**In Tool Handlers**:
```typescript
server.setRequestHandler("tools/call", async (request) => {
  try {
    const result = await callExternalAPI(request.params.arguments);
    return {
      content: [{ type: "text", text: JSON.stringify(result) }]
    };
  } catch (error) {
    // Return error as content, don't throw
    return {
      content: [
        {
          type: "text",
          text: `Error: ${error.message}. Please check your input and try again.`
        }
      ],
      isError: true
    };
  }
});
```

**Error Types to Handle**:
- Network errors (API unreachable)
- Authentication failures (expired token)
- Rate limiting (too many requests)
- Invalid input (missing required fields)
- Timeout (slow API response)

#### Caching

**Simple In-Memory Cache**:
```typescript
const cache = new Map<string, { data: any; expires: number }>();
const CACHE_TTL = 5 * 60 * 1000; // 5 minutes

function getCached(key: string): any | null {
  const entry = cache.get(key);
  if (entry && entry.expires > Date.now()) {
    return entry.data;
  }
  cache.delete(key);
  return null;
}

function setCache(key: string, data: any): void {
  cache.set(key, { data, expires: Date.now() + CACHE_TTL });
}
```

**Using Cache in Tools**:
```typescript
async function getWeather(city: string) {
  const cacheKey = `weather:${city}`;
  const cached = getCached(cacheKey);
  if (cached) return cached;

  const result = await fetchWeatherAPI(city);
  setCache(cacheKey, result);
  return result;
}
```

#### Rate Limiting

**Token Bucket Implementation**:
```typescript
class RateLimiter {
  private tokens: number;
  private lastRefill: number;
  private maxTokens: number;
  private refillRate: number; // tokens per second

  constructor(maxTokens: number, refillRate: number) {
    this.tokens = maxTokens;
    this.maxTokens = maxTokens;
    this.refillRate = refillRate;
    this.lastRefill = Date.now();
  }

  async acquire(): Promise<void> {
    this.refill();
    if (this.tokens <= 0) {
      const waitTime = (1 / this.refillRate) * 1000;
      await new Promise(resolve => setTimeout(resolve, waitTime));
      this.refill();
    }
    this.tokens--;
  }

  private refill(): void {
    const now = Date.now();
    const elapsed = (now - this.lastRefill) / 1000;
    this.tokens = Math.min(this.maxTokens, this.tokens + elapsed * this.refillRate);
    this.lastRefill = now;
  }
}
```

### Exercise

**Task**: Add error handling and caching to your MCP server

1. Add try/catch to all tool handlers
2. Implement a simple in-memory cache
3. Add rate limiting (max 10 requests/minute)
4. Test error scenarios (invalid input, API down)

### Verification

Before moving on, ensure you can:
- [ ] Handle errors without crashing the server
- [ ] Implement basic caching
- [ ] Add rate limiting
- [ ] Return helpful error messages to Claude

---

## Lesson 61: MCP in Enterprise Environments

### Objectives
- Deploy MCP servers for teams
- Manage server access controls
- Monitor MCP usage
- Follow compliance requirements

### Content

#### Enterprise MCP Deployment

**Managed Settings** (enterprise-enforced):
```json
{
  "mcpServers": {
    "company-api": {
      "command": "npx",
      "args": ["-y", "@company/mcp-api-server"],
      "env": {
        "API_URL": "https://internal-api.company.com",
        "API_KEY": "${COMPANY_API_KEY}"
      }
    }
  },
  "allowedMcpServers": [
    "company-api",
    "github",
    "postgres"
  ],
  "blockedMcpServers": [
    "untrusted-server"
  ]
}
```

#### Access Control

**Server Allowlists**:
- Only approved MCP servers can be used
- Enterprise settings override user preferences
- Prevents installation of unauthorized servers

**Credential Management**:
- Use secret management systems (Vault, AWS Secrets Manager)
- Rotate credentials automatically
- Audit credential usage

#### Monitoring

Track MCP usage across your organization:
- Which servers are most used
- Error rates per server
- Data access patterns
- Token usage and costs

#### Compliance Considerations

- **Data Residency**: Ensure MCP servers don't send data to unauthorized regions
- **Access Logging**: Log all MCP tool calls for audit trails
- **Data Classification**: Restrict which data MCP servers can access
- **Approval Workflow**: Require approval for new MCP server installations

### Exercise

**Task**: Design an enterprise MCP strategy

Create a document that covers:
1. Which MCP servers would your team use?
2. What access controls are needed?
3. How would credentials be managed?
4. What monitoring would you implement?

### Verification

Before moving on, ensure you understand:
- [ ] Enterprise MCP deployment patterns
- [ ] Access control mechanisms
- [ ] Monitoring and auditing requirements
- [ ] Compliance considerations

---

## Lesson 62: MCP Best Practices

### Objectives
- Follow MCP development best practices
- Optimize server performance
- Design good tool interfaces
- Plan for maintenance

### Content

#### Tool Design Best Practices

**1. Clear, Descriptive Names**:
```typescript
// Good
"get_user_by_email"
"create_github_issue"
"search_documentation"

// Bad
"getData"
"doThing"
"process"
```

**2. Complete Input Schemas**:
```typescript
{
  name: "search_issues",
  description: "Search GitHub issues by query, state, and labels",
  inputSchema: {
    type: "object",
    properties: {
      query: {
        type: "string",
        description: "Search query text"
      },
      state: {
        type: "string",
        enum: ["open", "closed", "all"],
        description: "Issue state filter (default: open)"
      },
      labels: {
        type: "array",
        items: { type: "string" },
        description: "Filter by label names"
      }
    },
    required: ["query"]
  }
}
```

**3. Helpful Error Messages**:
```typescript
// Good
"Authentication failed: GitHub token expired. Generate a new token at
https://github.com/settings/tokens and update GITHUB_TOKEN in settings."

// Bad
"Error: 401 Unauthorized"
```

#### Performance Best Practices

1. **Cache aggressively** — Most data doesn't change per-request
2. **Batch operations** — Combine multiple API calls when possible
3. **Paginate results** — Don't return 10,000 results at once
4. **Timeout appropriately** — Don't let slow APIs hang forever
5. **Use connection pooling** — For database servers

#### Maintenance Best Practices

1. **Version your server** — Use semver
2. **Document breaking changes** — In a changelog
3. **Test thoroughly** — Unit tests for every tool
4. **Monitor errors** — Track and fix failures
5. **Keep dependencies updated** — Security patches matter

#### Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Exposing write access unnecessarily | Default to read-only tools |
| Not validating input | Validate all parameters |
| Ignoring rate limits | Implement rate limiting |
| Hardcoding credentials | Use environment variables |
| No error handling | Catch all errors, return helpful messages |
| Returning too much data | Paginate and summarize |

### Exercise

**Task**: Review and improve your MCP server

Apply the best practices checklist to any MCP server you've built:
- [ ] All tools have clear names and descriptions
- [ ] Input schemas are complete with descriptions
- [ ] Errors return helpful messages
- [ ] Caching is implemented
- [ ] Rate limiting is in place
- [ ] Credentials are in environment variables
- [ ] Server is versioned
- [ ] Tests exist for each tool

### Verification

Before moving on, ensure you can:
- [ ] Design well-structured MCP tools
- [ ] Follow performance best practices
- [ ] Plan for maintenance and updates
- [ ] Avoid common MCP pitfalls

---

## Section 9 Summary

You've mastered the Model Context Protocol:

| Lesson | Topic | Key Takeaway |
|--------|-------|-------------|
| 51 | MCP Overview | Protocol for connecting Claude to external services |
| 52 | Installing Servers | Configure in settings.json with commands and env vars |
| 53 | Using Tools | Call MCP tools naturally in conversation |
| 54 | Resources | Access remote data via @-mentions |
| 55 | Prompts | Reusable templates from MCP servers |
| 56 | Authentication | Secure credential management |
| 57 | Popular Servers | GitHub, Postgres, Filesystem servers |
| 58 | Scopes | User, project, and enterprise scoping |
| 59 | Custom Servers | Build your own MCP server with the SDK |
| 60 | Error Handling | Caching, rate limiting, resilience |
| 61 | Enterprise | Team deployment and compliance |
| 62 | Best Practices | Design, performance, and maintenance |

### What's Next?

In **Section 10: Custom Subagents**, you'll learn how to build specialized AI agents with focused capabilities, controlled tool access, and optimized for specific workflows.
