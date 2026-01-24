# Section 9: MCP - Model Context Protocol (Lessons 51-62)

## Overview
Master MCP to extend Claude with external data sources and services.

---

## Lesson 51: MCP Overview

**What is MCP**: Protocol for connecting Claude to external tools and data
**Server Types**: HTTP, SSE, stdio
**Use Cases**: APIs, databases, file systems, cloud services

---

## Lesson 52: Installing MCP Servers

**Installation**:
```json
// settings.json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"]
    }
  }
}
```

**Exercise**: Install GitHub MCP server

---

## Lesson 53: Using MCP Tools

**Discovery**: Claude automatically discovers MCP tools
**Usage**: Use like native tools
**Example**:
```
"Fetch issue #123 from GitHub"
"Search repositories for 'claude'"
```

---

## Lesson 54: MCP Resources & @-mentions

**Resources**: Remote data accessible via @-mentions
**Example**:
```
"Summarize @github:issue:123"
"Compare @confluence:doc:456 with our implementation"
```

---

## Lessons 55-62: Advanced MCP

- **55**: MCP Prompts
- **56**: Authentication
- **57**: Popular servers (GitHub, Slack, Postgres)
- **58**: Scopes (user, project, enterprise)
- **59**: Building custom MCP servers
- **60**: MCP in plugins
- **61**: Enterprise management
- **62**: Best practices

**Exercise**: Build a custom MCP server for your API
