# Claude Code Bootcamp - Assessment Tests

Test your knowledge at each level to verify mastery before moving forward.

---

## 🎯 How to Use These Tests

### Before Starting a Level
- Take the pre-test to gauge current knowledge
- Identify weak areas to focus on
- Set learning goals

### After Completing a Level
- Take the post-test to verify mastery
- Must score 80%+ to pass
- Review incorrect answers
- Move to next level only after passing

### Test Format
- Multiple choice questions
- Practical exercises
- Code review tasks
- Real-world scenarios

---

## 🟢 Beginner Level Assessment

### Part 1: Multiple Choice (20 questions)

**Installation & Setup (Questions 1-4)**

1. Which command installs Claude Code on macOS?
   - a) `npm install -g claude`
   - b) `brew install anthropics/tap/claude`
   - c) `apt-get install claude`
   - d) `pip install claude`

2. After installation, what command authenticates Claude?
   - a) `claude login`
   - b) `claude auth login`
   - c) `claude start`
   - d) `claude --auth`

3. Which file stores user-level Claude settings?
   - a) `.claude/config.json`
   - b) `~/.clauderc`
   - c) `~/.claude/settings.json`
   - d) `/etc/claude/settings.json`

4. To start Claude in a specific directory, you should:
   - a) Always use absolute paths
   - b) `cd` to the directory first, then run `claude`
   - c) Use `claude --dir /path`
   - d) Set CLAUDE_DIR environment variable

**Tool Usage (Questions 5-12)**

5. Which tool should you use to create a new file?
   - a) Edit
   - b) Write
   - c) Create
   - d) Bash(touch)

6. Which tool should you use to modify an existing file?
   - a) Write
   - b) Modify
   - c) Edit
   - d) Update

7. When Edit tool fails with "string not found", the most common cause is:
   - a) File doesn't exist
   - b) Whitespace mismatch
   - c) File too large
   - d) Permission denied

8. To find all JavaScript files in a project, use:
   - a) Bash(find . -name "*.js")
   - b) Grep("*.js")
   - c) Glob("**/*.js")
   - d) Search("js files")

9. To search for a specific string in files, use:
   - a) Glob
   - b) Grep
   - c) Find
   - d) Search

10. The Read tool with no line range specified will:
    - a) Read first 100 lines only
    - b) Read entire file
    - c) Ask which lines to read
    - d) Fail with error

11. Which tool should you use to run terminal commands?
    - a) Terminal
    - b) Execute
    - c) Bash
    - d) Shell

12. To read multiple files efficiently, you should:
    - a) Read them one by one sequentially
    - b) Request parallel reads
    - c) Use Glob instead
    - d) Use Bash(cat)

**Permissions & Workflow (Questions 13-16)**

13. In normal permission mode, Claude will:
    - a) Execute all operations automatically
    - b) Ask for approval before each operation
    - c) Only ask for dangerous operations
    - d) Never ask for permission

14. Which permission mode should you use when learning?
    - a) auto-accept
    - b) normal
    - c) plan
    - d) expert

15. In plan mode, Claude will:
    - a) Execute immediately
    - b) Explore read-only, then present plan
    - c) Skip permission prompts
    - d) Use Opus model automatically

16. The basic workflow pattern is:
    - a) Code → Test → Deploy
    - b) Explore → Plan → Code → Verify
    - c) Read → Write → Execute
    - d) Plan → Build → Ship

**Git Integration (Questions 17-20)**

17. To create a commit with Claude, you should first:
    - a) Use Write to create commit message
    - b) Run git add manually
    - c) Ask Claude to stage files and create commit
    - d) Use /commit command

18. Claude will add this line to commit messages:
    - a) "Committed by Claude"
    - b) "Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
    - c) Nothing extra
    - d) "AI-assisted commit"

19. To create a pull request with Claude:
    - a) Use Write to create PR description
    - b) Run gh pr create manually
    - c) Ask Claude to create PR with description
    - d) PRs can't be created through Claude

20. When should Claude push to remote?
    - a) Automatically after each commit
    - b) Only when you explicitly ask
    - c) Never - always manual
    - d) When in auto-accept mode

### Part 2: Practical Exercises (5 tasks)

**Task 1: Basic File Operations**

Setup:
```bash
mkdir ~/claude-test
cd ~/claude-test
echo "console.log('test')" > app.js
claude
```

Requirements:
1. Read app.js
2. Add a function called `greet(name)` that returns `"Hello, {name}!"`
3. Update the console.log to call `greet("World")`
4. Verify changes work

**Task 2: Search and Modify**

Setup: Use a JavaScript project

Requirements:
1. Find all files containing "TODO"
2. Read one of those files
3. Replace a TODO comment with actual implementation
4. Verify using git diff

**Task 3: Git Workflow**

Setup: Any project with git

Requirements:
1. Check current git status
2. Create a new branch called "test-branch"
3. Make a small change
4. Create a commit with meaningful message
5. Show the commit log

**Task 4: CLAUDE.md Creation**

Requirements:
Create CLAUDE.md for a sample project with:
- Project description
- Tech stack
- Project structure
- Code conventions
- Common commands

**Task 5: Error Recovery**

Scenario: You asked Claude to edit a file but Edit failed

Requirements:
1. Diagnose why Edit failed
2. Re-read the file
3. Successfully complete the edit using exact string
4. Verify the change

### Part 3: Code Review (2 tasks)

**Review Task 1**

Review this Claude interaction and identify the mistake:

```
User: "Change the port in app.js"

Claude: [Uses Write on app.js]

Content: const PORT = 8080
```

What's wrong? What should have been done instead?

**Review Task 2**

Review this CLAUDE.md and suggest improvements:

```markdown
# My Project

This is a web app.

## Running

npm start
```

What's missing? How would you improve it?

### Scoring

**Multiple Choice**: 1 point each (20 points total)
**Practical Exercises**: 4 points each (20 points total)
**Code Review**: 5 points each (10 points total)

**Total**: 50 points
**Passing Score**: 40 points (80%)

### Answer Key

<details>
<summary>Click to reveal answers</summary>

**Multiple Choice**:
1. b, 2. b, 3. c, 4. b, 5. b, 6. c, 7. b, 8. c, 9. b, 10. b,
11. c, 12. b, 13. b, 14. b, 15. b, 16. b, 17. c, 18. b, 19. c, 20. b

**Practical Exercises**: Check against requirements

**Code Review Task 1**: Should have used Edit, not Write. Write replaces entire file, losing existing content.

**Code Review Task 2**: Missing tech stack, file structure, dependencies, development setup, conventions, etc.

</details>

---

## 🟡 Intermediate Level Assessment

### Part 1: Multiple Choice (20 questions)

**Advanced Tools (Questions 1-6)**

1. Task tool launches:
   - a) Background processes
   - b) Specialized subagents
   - c) Parallel operations
   - d) Remote sessions

2. Which subagent should you use for codebase exploration?
   - a) general-purpose
   - b) Plan
   - c) Explore
   - d) Search

3. To run a long operation in background, press:
   - a) Ctrl+B
   - b) Ctrl+G
   - c) Ctrl+T
   - d) Ctrl+Z

4. The Task management system allows you to:
   - a) Schedule future commands
   - b) Track multi-step work
   - c) Run tasks in parallel
   - d) Import tasks from Jira

5. To compact conversation history, use:
   - a) /compact
   - b) /compress
   - c) /clear
   - d) /reduce

6. Context tokens are used by:
   - a) Conversation only
   - b) Tool results only
   - c) Conversation + tool results + code read
   - d) Current message only

**Skills System (Questions 7-12)**

7. A skill is defined in a file named:
   - a) skill.yaml
   - b) SKILL.md
   - c) skill.json
   - d) .skill

8. Skill frontmatter is written in:
   - a) JSON
   - b) YAML
   - c) TOML
   - d) XML

9. To invoke a skill, use:
   - a) @skill-name
   - b) #skill-name
   - c) /skill-name
   - d) skill:name

10. Skills can be scoped at:
    - a) User level only
    - b) Project level only
    - c) User, project, enterprise, plugin
    - d) Global level only

11. In skill frontmatter, `autoInvoke: true` means:
    - a) Skill runs on startup
    - b) Skill triggers based on patterns
    - c) Skill runs automatically every hour
    - d) Skill requires no permission

12. To restrict a skill to specific tools:
    - a) Add `tools:` list in frontmatter
    - b) Add `allowedTools:` in instructions
    - c) Mention tools in description
    - d) Can't restrict tools

**Hooks System (Questions 13-20)**

13. PreToolUse hooks run:
    - a) After tool completes
    - b) Before tool executes
    - c) On tool error
    - d) Every 5 minutes

14. PostToolUse hooks are good for:
    - a) Validation
    - b) Auto-formatting
    - c) Blocking operations
    - d) Pre-flight checks

15. Hooks are configured in:
    - a) HOOKS.md
    - b) .hooks/
    - c) settings.json
    - d) claude.config

16. To auto-format code after writing, use:
    - a) PreToolUse hook
    - b) PostToolUse hook
    - c) Permission hook
    - d) Notification hook

17. To auto-approve all Read operations:
    - a) Set autoApprove: ["Read"] in settings
    - b) Use --auto-accept flag
    - c) Use permission hook with action: approve
    - d) Either a or c

18. Hook matchers can filter by:
    - a) Tool name only
    - b) File path only
    - c) Tool name, file path, command pattern
    - d) Time of day

19. If a PreToolUse hook fails with stopOnError: true:
    - a) Operation proceeds anyway
    - b) Operation is blocked
    - c) Claude retries
    - d) Hook is disabled

20. continueOnError: true in hooks means:
    - a) Errors are ignored
    - b) Tool proceeds even if hook fails
    - c) Hook is retried
    - d) Error is logged only

### Part 2: Practical Exercises (5 tasks)

**Task 1: Create a Useful Skill**

Create a skill that:
- Checks for console.log statements in JavaScript files
- Reports their locations
- Optionally removes them
- Has commands: find-logs, remove-logs

**Task 2: Configure Hooks**

Set up hooks that:
1. Auto-format JavaScript files after Write/Edit (prettier)
2. Validate JSON files before Write (jq)
3. Auto-stage files in src/ after Edit (git add)

**Task 3: Use Task Management**

For a feature "Add user profile":
1. Create task list with 5 subtasks
2. Mark tasks as in_progress as you work
3. Complete all tasks
4. Generate completion summary

**Task 4: Parallel Execution**

Optimize this workflow:
- Analyze 5 different components
- Do it in parallel (single message)
- Compare with sequential approach

**Task 5: Context Optimization**

In a large codebase:
1. Demonstrate token usage issue
2. Use /compact to reduce context
3. Use line ranges for targeted reading
4. Show improved efficiency

### Part 3: Skill Design (2 tasks)

**Design Task 1**

Design a skill for API endpoint generation:
- Name, description, commands
- Input parameters
- Steps it follows
- Output format
- Templates needed

**Design Task 2**

Design a hook system for code quality:
- What hooks are needed
- When they trigger
- What they check
- How they report issues

### Scoring

**Multiple Choice**: 1 point each (20 points)
**Practical Exercises**: 4 points each (20 points)
**Design Tasks**: 5 points each (10 points)

**Total**: 50 points
**Passing Score**: 40 points (80%)

---

## 🟠 Advanced Level Assessment

### Part 1: Multiple Choice (20 questions)

**MCP (Questions 1-8)**

1. MCP stands for:
   - a) Multi-Context Protocol
   - b) Model Context Protocol
   - c) Message Control Protocol
   - d) Model Command Protocol

2. MCP servers can be:
   - a) HTTP, SSE, stdio
   - b) TCP, UDP
   - c) WebSocket only
   - d) REST only

3. MCP servers are configured in:
   - a) .mcp/
   - b) settings.json
   - c) MCP.md
   - d) ~/.mcp/config

4. To use an MCP resource, you:
   - a) @-mention it
   - b) Use ToolSearch
   - c) Use Resource tool
   - d) Both a and b

5. MCP authentication can use:
   - a) OAuth only
   - b) API tokens only
   - c) OAuth, tokens, or other methods
   - d) No authentication

6. MCP scopes include:
   - a) User only
   - b) User and project
   - c) User, project, local
   - d) Global only

7. To build a custom MCP server, you:
   - a) Implement MCP protocol
   - b) Use MCP SDK
   - c) Extend base server class
   - d) Either a or b

8. MCP tools appear in Claude as:
   - a) Regular tools
   - b) Special MCP tools
   - c) Remote tools
   - d) Can't use them

**Custom Subagents (Questions 9-14)**

9. Subagents are defined in:
   - a) SUBAGENT.md
   - b) AGENT.md
   - c) agent.yaml
   - d) .agent/

10. Subagent vs skill main difference:
    - a) Subagents can use all tools
    - b) Subagents are full agents with own behavior
    - c) Subagents are faster
    - d) No difference

11. To choose a model for a subagent:
    - a) model: sonnet in frontmatter
    - b) --model flag
    - c) Can't choose
    - d) Always uses Opus

12. Subagents can have:
    - a) Preloaded skills
    - b) Custom hooks
    - c) Tool restrictions
    - d) All of the above

13. Background subagents:
    - a) Run with run_in_background: true
    - b) Can be monitored via output file
    - c) Continue while you work
    - d) All of the above

14. Subagent scopes:
    - a) User, project only
    - b) User, project, enterprise
    - c) Global only
    - d) No scoping

**Plan Mode & IDE (Questions 15-20)**

15. Plan mode is activated by:
    - a) /plan
    - b) --plan flag
    - c) Shift+Tab twice
    - d) b or c

16. In plan mode, Claude:
    - a) Executes immediately
    - b) Explores read-only then shows plan
    - c) Skips planning
    - d) Uses Opus model

17. Extended thinking mode:
    - a) Uses more tokens for reasoning
    - b) Is always enabled
    - c) Is free
    - d) Doesn't help quality

18. VS Code extension provides:
    - a) Inline diffs
    - b) @-mention autocomplete
    - c) Multiple conversations
    - d) All of the above

19. In IDE, you can reference:
    - a) Files with @-mentions
    - b) Line ranges
    - c) Symbols
    - d) All of the above

20. Remote development with Claude:
    - a) Not supported
    - b) Works with SSH, containers
    - c) Local only
    - d) Cloud only

### Part 2: Practical Exercises (5 tasks)

**Task 1: Install and Use MCP Server**

1. Install GitHub MCP server
2. Configure authentication
3. Use it to fetch issue data
4. Create a skill that uses MCP

**Task 2: Build Custom Subagent**

Create a subagent that:
- Specializes in test generation
- Uses Haiku model for speed
- Has test-gen skill preloaded
- Runs comprehensive test generation

**Task 3: Plan Mode Workflow**

Use plan mode for:
- Complex refactoring (callbacks to async/await)
- Review plan
- Approve and execute
- Verify results

**Task 4: IDE Integration**

In VS Code:
- Set up extension
- Use @-mentions for files
- Reference line ranges
- Review changes inline
- Manage multiple conversations

**Task 5: Build MCP Server**

Build simple MCP server for weather API:
- Implements getCurrentWeather tool
- Handles authentication
- Returns structured data
- Can be used by Claude

### Part 3: Architecture Design (2 tasks)

**Design Task 1: Enterprise MCP Strategy**

Design MCP strategy for 100-person company:
- What servers to deploy
- How to manage access
- Authentication approach
- Monitoring and logging

**Design Task 2: Multi-Agent System**

Design system with multiple specialized agents:
- Exploration agent
- Development agent
- Testing agent
- Deployment agent
- How they coordinate

### Scoring

**Total**: 50 points
**Passing Score**: 40 points (80%)

---

## 🔴 Expert Level Assessment

### Comprehensive Project

Build a complete enterprise deployment system:

1. **Custom Plugin** (20 points)
   - 5 skills
   - 5 hooks
   - 2 subagents
   - 1 MCP server
   - Complete documentation

2. **CI/CD Integration** (10 points)
   - GitHub Actions workflow using Claude
   - Automated code review
   - Automated testing
   - Deployment automation

3. **Performance Optimization** (10 points)
   - Profile application
   - Identify bottlenecks
   - Implement optimizations
   - Measure improvements

4. **Security Audit** (10 points)
   - Comprehensive security review
   - Vulnerability identification
   - Fix implementation
   - Verification

**Total**: 50 points
**Passing Score**: 40 points (80%)

---

## 📊 Certification Levels

### Beginner Certified
- Pass beginner assessment (80%+)
- Complete 5 beginner projects
- Create working CLAUDE.md
- Daily Claude usage (1 month)

### Intermediate Certified
- Pass intermediate assessment (80%+)
- Created 3+ custom skills
- Configured 5+ hooks
- Complete 5 intermediate projects

### Advanced Certified
- Pass advanced assessment (80%+)
- Connected 5+ MCP servers
- Built custom subagent
- Complete 5 advanced projects

### Expert Certified
- Pass expert assessment (80%+)
- Built custom MCP server
- Published plugin
- Contributed to community
- All verifications passed

---

**Use these assessments to verify your progress and ensure mastery at each level!**
