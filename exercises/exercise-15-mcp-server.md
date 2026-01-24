# Exercise 15: Connecting MCP Servers

**Time**: 1.5 hours
**Prerequisites**: Lessons 51-57

## Objective
Install, configure, and use MCP servers to extend Claude's capabilities.

## Part 1: Understanding MCP (20 min)

### What is MCP?
Model Context Protocol (MCP) allows Claude to connect to external services and tools.

### MCP Server Types
- **stdio**: Local processes communicating via stdin/stdout
- **SSE**: Server-Sent Events over HTTP
- **HTTP**: REST-like communication

### Configuration Location
```
~/.claude/settings.json           # User-level MCP
.claude/settings.json             # Project-level MCP
```

---

## Part 2: Install Your First MCP Server (30 min)

### Task 2.1: Filesystem MCP Server

The filesystem server gives Claude controlled access to directories.

**Install:**
```bash
npm install -g @anthropic-ai/mcp-server-filesystem
```

**Configure in ~/.claude/settings.json:**
```json
{
  "mcp": {
    "servers": {
      "filesystem": {
        "type": "stdio",
        "command": "mcp-server-filesystem",
        "args": ["/path/to/allowed/directory"],
        "description": "Access to project files"
      }
    }
  }
}
```

**Test:**
```
You: "List the files using the filesystem MCP server"
```

**Verify**: Claude uses the MCP server to list files

---

### Task 2.2: GitHub MCP Server

**Install:**
```bash
npm install -g @anthropic-ai/mcp-server-github
```

**Configure:**
```json
{
  "mcp": {
    "servers": {
      "github": {
        "type": "stdio",
        "command": "mcp-server-github",
        "env": {
          "GITHUB_TOKEN": "your-github-token"
        }
      }
    }
  }
}
```

**Test:**
```
You: "Use GitHub MCP to list my recent repositories"
You: "Search for issues labeled 'bug' in my-repo"
```

**Verify**: Claude retrieves GitHub data

---

## Part 3: Using MCP Tools (20 min)

### Discovering Available Tools

```
You: "What MCP tools are available?"
```

Claude will list tools from connected servers.

### Using MCP vs Built-in Tools

**Built-in Read:**
```
You: "Read package.json"
# Uses Claude's built-in Read tool
```

**MCP Filesystem Read:**
```
You: "Use the filesystem MCP to read package.json"
# Uses the MCP server
```

### @-Mentioning MCP Resources

```
You: "Explain @github:issues/123"
# References GitHub issue via MCP
```

---

## Part 4: Database MCP Server (30 min)

### Install PostgreSQL MCP (if you have PostgreSQL)

```bash
npm install -g @anthropic-ai/mcp-server-postgres
```

**Configure:**
```json
{
  "mcp": {
    "servers": {
      "postgres": {
        "type": "stdio",
        "command": "mcp-server-postgres",
        "env": {
          "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
        }
      }
    }
  }
}
```

### Using Database MCP

```
You: "Show me the schema of the users table"
You: "Find all users created in the last week"
You: "What are the most common values in the status column?"
```

### Safety Features

MCP database servers typically:
- Run as read-only by default
- Limit query complexity
- Log all queries
- Support allowlists

---

## Part 5: MCP Best Practices (10 min)

### Security Considerations

1. **Principle of Least Privilege**
   - Only grant necessary access
   - Use read-only when possible

2. **Credential Management**
   - Use environment variables
   - Never commit tokens

3. **Scope Control**
   - Project-level for project-specific servers
   - User-level for personal tools

### Performance Tips

1. Use MCP when external data needed
2. Prefer built-in tools for local files
3. Cache MCP results when appropriate

---

## Completion Criteria

- [ ] Installed at least one MCP server
- [ ] Configured in settings.json
- [ ] Successfully used MCP tools
- [ ] Understand MCP vs built-in tools
- [ ] Know security best practices

## Verification Questions

1. What command lists available MCP servers?
2. Where is user-level MCP configuration stored?
3. How do you reference MCP resources with @-mentions?
4. Why would you choose MCP over built-in tools?

## Troubleshooting

**MCP server not found:**
- Check npm global installation path
- Verify command in configuration

**Connection failed:**
- Check server is running
- Verify credentials
- Check network/firewall

**Permission denied:**
- Check file/directory permissions
- Verify API tokens

## Next Steps

After completing this exercise:
1. Try connecting a second MCP server
2. Create project-level MCP configuration
3. Explore MCP server documentation
4. Consider building a custom MCP server
