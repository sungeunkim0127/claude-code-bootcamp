# Exercise 4: Web Tools & Configuration

**Time**: 45 minutes
**Prerequisites**: Lessons 18-25

## Objective
Use web tools for research and set up project configuration with CLAUDE.md and settings.

## Setup
```bash
mkdir -p ~/claude-exercises/ex4
cd ~/claude-exercises/ex4
git init
npm init -y
echo "const express = require('express')" > app.js
claude
```

## Tasks

### Task 1: Web Search for Current Info (10 min)
Ask Claude: "What are the current best practices for Express.js error handling in 2024? Search the web and summarize."
**Verify**: Claude uses WebSearch, provides sourced recommendations

Ask Claude: "Search for the latest version of Express.js and what changed"
**Verify**: Claude provides current version info with sources

---

### Task 2: Web Fetch for Documentation (10 min)
Ask Claude: "Fetch the Express.js getting started guide and create a basic app.js based on their recommended setup"
**Verify**: Claude fetches docs and creates a properly structured Express app

---

### Task 3: Create CLAUDE.md (10 min)
Ask Claude: "Create a CLAUDE.md for this project. It's a Node.js Express API that should use:
- ESM imports (import/export)
- Async/await for all async operations
- 2-space indentation
- Jest for testing
- Descriptive error messages"

**Verify**: CLAUDE.md created with project context, style rules, and conventions

---

### Task 4: Test CLAUDE.md Works (5 min)
Start a new Claude session in the same directory:
```bash
exit  # exit current session
claude  # start fresh — Claude reads CLAUDE.md
```

Ask Claude: "Create a new route file for products with GET and POST endpoints"
**Verify**: Claude follows the conventions in CLAUDE.md (ESM imports, async/await, 2-space indent)

---

### Task 5: Explore Settings (10 min)
Ask Claude: "Show me the .claude directory structure and explain what each file does"
**Verify**: Claude explains settings hierarchy

Ask Claude: "What permission mode am I currently using?"
**Verify**: Claude explains the current permission configuration

## Completion Criteria
- [ ] Used WebSearch to find current information
- [ ] Used WebFetch to read documentation
- [ ] Created a functional CLAUDE.md
- [ ] Verified Claude follows CLAUDE.md conventions
- [ ] Understand settings structure and scopes

## Bonus Challenge
Create a `.claude/settings.json` for the project with custom permission rules:
```
"Allow all git commands"
"Allow npm test and npm run build"
```
