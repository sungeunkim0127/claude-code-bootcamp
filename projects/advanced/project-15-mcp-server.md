# Project 15: Build Custom MCP Server

**Level**: Advanced
**Time**: 6-8 hours
**Goal**: Create MCP server for external API

## Requirements

Build MCP server that:
- Connects to REST API (weather, stocks, etc.)
- Implements 3+ tools
- Handles authentication
- Caches responses
- Manages rate limits
- Comprehensive error handling

## Implementation

**Structure**:
```
my-mcp-server/
├── src/
│   ├── index.ts
│   ├── server.ts
│   ├── tools/
│   ├── auth.ts
│   └── cache.ts
├── package.json
└── README.md
```

**Tools to Implement**:
1. `getData(params)` - Fetch data
2. `search(query)` - Search API
3. `getStatus()` - API status

**Features**:
- OAuth2 or API key auth
- Redis or in-memory cache
- Rate limit handling
- Retry logic
- TypeScript types

**Testing**:
- Unit tests for each tool
- Integration tests
- Error scenario tests

**Documentation**:
- Installation guide
- Configuration
- Usage examples
- API reference

**Success Criteria**:
- Server starts and connects
- All tools work correctly
- Caching reduces API calls
- Errors handled gracefully
- Can be used by Claude
