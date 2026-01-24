# Claude Code Cheat Sheet

Quick reference for commands, patterns, and best practices.

---

## 🚀 CLI Commands

### Starting Claude
```bash
claude                          # Start in current directory
claude --session "name"         # Named session
claude --resume                 # Resume last session
claude --auto-accept            # Skip permission prompts
claude --plan                   # Start in plan mode
claude --model opus             # Use Opus model
claude --model haiku            # Use Haiku model
```

### Authentication
```bash
claude auth login               # Login
claude auth logout              # Logout
claude auth whoami              # Show current user
```

### Configuration
```bash
claude config list              # Show all settings
claude config set key value     # Set a config value
```

---

## 💬 In-Session Commands

### Navigation
```
/help          Show available commands
/exit          Exit session (or Ctrl+C)
/clear         Clear conversation history
/history       Show past sessions
/resume        Resume a previous session
```

### Context Management
```
/compact       Compress conversation history
/settings      View current settings
```

### Model Selection
```
/model sonnet  Switch to Sonnet (default)
/model opus    Switch to Opus (more capable)
/model haiku   Switch to Haiku (faster, cheaper)
```

---

## 🛠️ Tool Usage Patterns

### File Reading
```
Read specific file:
"Read src/app.js"

Read multiple files in parallel:
"Read app.js, config.js, and utils.js in parallel"

Read specific lines:
"Read src/app.js lines 50-100"

Use @-mentions:
"Explain @src/auth.js"
```

### File Writing
```
Create new file:
"Create src/utils/logger.js with a basic logging utility"

Create multiple files:
"Set up an Express API with index.js, app.js, and routes.js"

Use templates:
"Create a React component following our standard template"
```

### File Editing
```
Simple edit:
"Change the port in app.js from 3000 to 8080"

Multi-line edit:
"Update the authenticate function to add error handling"

Rename (replace all):
"Rename variable 'oldName' to 'newName' throughout the file"

Add new function:
"Add a validateEmail function after the validateUser function"
```

### Searching
```
Find files:
"Find all test files"
"Find all .tsx files in src/"

Search content:
"Search for 'TODO' in all JavaScript files"
"Find functions named 'handle*' in src/"

Search with context:
"Search for 'authentication' and show 5 lines before and after"
```

### Running Commands
```
Git operations:
"Show git status"
"Create a commit with message 'Add feature'"
"Create a new branch called 'feature-name'"

Package management:
"Install express and mongoose"
"Update dependencies"
"Run npm test"

Build & run:
"Build the project"
"Start the development server"
"Run the linter"
```

---

## 📝 Prompting Patterns

### Basic Pattern
```
Context + Request + Requirements

"I'm building a user authentication system.
Add password reset functionality.
Must use JWT tokens and send email via SendGrid."
```

### PREP Framework (Advanced)
```
**Problem**: API response times are 2-3 seconds
**Requirements**: Must stay under 1MB memory, maintain backwards compatibility
**Examples**: Similar to how we optimized the user service in commit abc123
**Parameters**: Express 4.x, Node 18+, MongoDB 6.0, Redis for caching
```

### Iterative Refinement
```
1. "Add user authentication"
   [Claude implements]

2. "Good. Now add refresh tokens"
   [Claude adds feature]

3. "Extract token generation into a separate module"
   [Claude refactors]
```

---

## 🎯 Best Practices

### Do's ✅
- ✅ Read files before editing
- ✅ Review diffs before approving
- ✅ Give specific, clear instructions
- ✅ Use plan mode for complex tasks
- ✅ Request parallel operations when possible
- ✅ Track progress in task lists
- ✅ Create CLAUDE.md for projects
- ✅ Test changes after applying
- ✅ Use meaningful commit messages
- ✅ Resume sessions for ongoing work

### Don'ts ❌
- ❌ Skip reading before editing
- ❌ Auto-approve without review
- ❌ Give vague instructions ("make it better")
- ❌ Use Write when Edit is appropriate
- ❌ Ignore permission prompts
- ❌ Forget to verify results
- ❌ Mix unrelated changes in commits
- ❌ Use auto-accept for important work

---

## ⚡ Keyboard Shortcuts

### VS Code Extension
```
Cmd/Ctrl + Shift + P  → Claude: Open
Cmd/Ctrl + L          → Send message
Cmd/Ctrl + K          → Clear conversation
Cmd/Ctrl + N          → New conversation
Tab twice (Shift+Tab) → Enter plan mode
```

### Terminal
```
Ctrl + C              → Exit Claude
Ctrl + D              → Exit Claude
Ctrl + B              → Run task in background
```

---

## 🔧 Common Workflows

### Feature Development
```
1. Start session: claude --session "feature-name"
2. Explore: "Analyze the relevant files"
3. Plan: "Here's what I want to add..."
4. Implement: Follow Claude's suggestions
5. Test: "Run the tests"
6. Commit: "Create a commit for this feature"
7. PR: "Generate a PR description"
```

### Bug Fixing
```
1. Reproduce: "Here's the error: [paste]"
2. Investigate: "Let's find where this comes from"
3. Read relevant code
4. Fix: "Here's the fix..."
5. Verify: "Run the tests to verify"
6. Commit: "Create a commit for this fix"
```

### Code Review
```
1. "Review the changes in PR #123"
2. Review Claude's analysis
3. "Check for security issues"
4. "Suggest improvements"
5. Post review comment
```

### Refactoring
```
1. Plan mode: claude --plan
2. "I want to refactor X to Y"
3. Review plan
4. Approve execution
5. "Run tests after each change"
6. Verify all tests pass
```

---

## 🎨 Skill Invocation

```
/skill-name              Invoke skill
/skill-name arg1 arg2    Invoke with arguments
/code-review             Review code
/commit                  Create commit
/deploy                  Deploy application
```

---

## 🔐 Permission Modes

### Normal (Default)
```
claude
```
- Asks before each action
- Best for: Learning, important work

### Auto-Accept
```
claude --auto-accept
```
- Executes without asking
- Best for: Experimentation, trusted operations
- ⚠️ Use carefully!

### Plan Mode
```
claude --plan
```
- Explores first (read-only)
- Shows plan for approval
- Best for: Complex features, refactoring

---

## 📋 Task Management

### Create Task
```
"Create a task list for implementing authentication:
1. Set up user model
2. Add login endpoint
3. Add JWT middleware
4. Add tests"
```

### Update Task Status
```
Task commands (implicit):
- "Mark task 1 as in progress"
- "Complete task 2"
- "Show task list"
```

---

## 🌐 Web Tools

### Web Search
```
"Search for the latest React 19 features and show me how to use them"
"What are the current best practices for Node.js security in 2026?"
```

### Web Fetch
```
"Fetch the documentation from https://example.com/api-docs and implement the user endpoint"
```

---

## 🐛 Debugging

### View Logs
```
claude --verbose         Start with verbose logging
```

### Check Permissions
```
Review .claude/settings.json
Check autoApprove and autoDeny rules
```

### Reset Context
```
/clear                   Clear conversation
/compact                 Compress history
Start new session        Fresh start
```

---

## 📊 Context Management

### Token Limits
- Sonnet: ~200K tokens
- Each file read: ~2-3K tokens (for 500 lines)
- Monitor usage with `/compact`

### Optimization
```
Read specific lines instead of whole files:
"Read app.js lines 1-50"

Use Grep before Read:
"Search for 'auth' then read only relevant files"

Compact when needed:
"/compact"

Start new session for new topic:
"Let's start fresh for this new feature"
```

---

## 🚨 Error Recovery

### Edit Fails
```
1. Re-read the file
2. Copy exact string from Read output
3. Try again
4. If still fails, use Write instead
```

### Tool Timeout
```
1. Break into smaller operations
2. Use simpler commands
3. Check network connection
```

### Permission Denied
```
1. Check file permissions
2. Review .gitignore
3. Use sudo if necessary (carefully!)
```

---

## 💡 Pro Tips

### Parallel Operations
```
"Read these files in parallel: [list]"
"Search for these patterns simultaneously: [list]"
```

### Background Tasks
```
"Run tests in background while we continue"
Ctrl+B to background current operation
```

### Git Worktrees
```
# Terminal 1
cd ~/project-feature-a
claude --session "feature-a"

# Terminal 2
cd ~/project-feature-b
claude --session "feature-b"

# Work on both features simultaneously
```

### Session Organization
```
Use descriptive session names:
--session "fix-auth-bug"
--session "add-payment-integration"
--session "refactor-database-layer"
```

---

## 📖 Quick Reference URLs

- **Docs**: https://code.claude.com/docs
- **API Docs**: https://platform.claude.com/docs
- **MCP Docs**: https://modelcontextprotocol.io
- **Awesome List**: https://github.com/hesreallyhim/awesome-claude-code

---

## 🎓 Learning Resources

### In This Bootcamp
- `README.md` - Overview
- `QUICK-START.md` - 30-min intro
- `CURRICULUM.md` - All lessons
- `EXPERT-MASTERY-GUIDE.md` - Advanced techniques
- `templates/` - Ready-to-use templates

### Practice
- `EXERCISES-WORKBOOK.md` - 20 exercises
- `projects/` - Project templates

---

**Print this cheat sheet or keep it open for quick reference!**

*Pro tip: Add this to your CLAUDE.md:*
```markdown
## Quick Commands

See ~/bootcamp/resources/CHEAT-SHEET.md for full reference
```
