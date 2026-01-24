# Claude Code Exercises Workbook

Hands-on exercises to reinforce your learning. Complete these alongside the lessons.

---

## 🟢 Beginner Exercises

### Exercise 1: First Steps
**Time**: 30 minutes
**Prerequisites**: Lessons 1-3

**Setup**:
```bash
mkdir ~/claude-exercises/ex1-first-steps
cd ~/claude-exercises/ex1-first-steps
```

**Tasks**:
1. Create a simple JavaScript file with a function
2. Ask Claude to explain the code
3. Ask Claude to add documentation
4. Ask Claude to add error handling
5. Have Claude create a test file

**Verification**:
- [ ] Claude successfully read and explained code
- [ ] Documentation was added
- [ ] Error handling is present
- [ ] Test file exists and tests work

---

### Exercise 2: File Operations
**Time**: 45 minutes
**Prerequisites**: Lessons 6-8

**Setup**:
```bash
mkdir ~/claude-exercises/ex2-file-ops
cd ~/claude-exercises/ex2-file-ops
```

**Tasks**:
1. Have Claude create a config.json file with sample data
2. Ask Claude to read and explain the structure
3. Have Claude edit the file to add new fields
4. Ask Claude to create a JavaScript module that reads the config
5. Have Claude write a README.md explaining the setup

**Verification**:
- [ ] config.json created with valid JSON
- [ ] Fields added correctly
- [ ] Module successfully reads config
- [ ] README is comprehensive

---

### Exercise 3: Search & Find
**Time**: 30 minutes
**Prerequisites**: Lessons 9-10

**Setup**:
Use an existing project or clone a sample repository:
```bash
git clone https://github.com/expressjs/express.git ~/claude-exercises/ex3-search
cd ~/claude-exercises/ex3-search
```

**Tasks**:
1. Find all JavaScript files using Glob
2. Search for "middleware" using Grep
3. Find files containing "router"
4. Count occurrences of "app.use"
5. Find all test files

**Verification**:
- [ ] Found correct file count
- [ ] Located middleware implementations
- [ ] Identified router files
- [ ] Counted app.use correctly

---

### Exercise 4: Git Workflow
**Time**: 1 hour
**Prerequisites**: Lessons 13-17

**Setup**:
```bash
mkdir ~/claude-exercises/ex4-git
cd ~/claude-exercises/ex4-git
git init
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit"
```

**Tasks**:
1. Create a new feature branch with Claude
2. Add a new file with Claude's help
3. Make several commits with meaningful messages
4. Create a pull request description
5. Review the changes
6. Merge the branch

**Verification**:
- [ ] Feature branch created
- [ ] Multiple meaningful commits
- [ ] PR description is comprehensive
- [ ] Successfully merged

---

### Exercise 5: CLAUDE.md Creation
**Time**: 45 minutes
**Prerequisites**: Lesson 21

**Setup**:
Choose one of your projects

**Tasks**:
1. Create CLAUDE.md from template
2. Document tech stack
3. Add project structure
4. Define code conventions
5. Test that Claude uses it

**Verification**:
- [ ] CLAUDE.md exists and is comprehensive
- [ ] Claude references it in conversations
- [ ] Conventions are clear
- [ ] Structure is documented

---

## 🟡 Intermediate Exercises

### Exercise 6: Task Management
**Time**: 1 hour
**Prerequisites**: Lesson 31

**Setup**:
Any project

**Tasks**:
1. Create a task list for implementing a feature
2. Mark tasks as in_progress as you work
3. Complete tasks and mark them done
4. Use task dependencies (blockedBy)
5. Generate a completion report

**Verification**:
- [ ] Created 5+ tasks
- [ ] Used status updates correctly
- [ ] Dependencies worked properly
- [ ] All tasks completed

---

### Exercise 7: First Custom Skill
**Time**: 2 hours
**Prerequisites**: Lessons 33-36

**Setup**:
```bash
mkdir -p ~/.claude/skills/my-first-skill
```

**Tasks**:
1. Create a skill that reviews code for common issues
2. Add YAML frontmatter with metadata
3. Define clear instructions
4. Include examples
5. Test the skill on real code

**Skill Should Check**:
- Console.log statements (should be removed)
- TODO comments
- Long functions (>50 lines)
- Missing error handling
- Hardcoded values

**Verification**:
- [ ] Skill file created correctly
- [ ] Frontmatter is valid
- [ ] Skill executes successfully
- [ ] Finds actual issues

---

### Exercise 8: Hook Configuration
**Time**: 1.5 hours
**Prerequisites**: Lessons 43-49

**Setup**:
Create project-level settings:
```bash
mkdir -p .claude
```

**Tasks**:
1. Create hook that auto-formats JavaScript files after writing
2. Create hook that validates JSON before writing
3. Create hook that auto-stages git changes
4. Create permission hook that auto-approves Read operations
5. Test all hooks

**Verification**:
- [ ] Auto-formatting works
- [ ] JSON validation prevents bad writes
- [ ] Git staging happens automatically
- [ ] Reads don't require approval
- [ ] No unexpected side effects

---

### Exercise 9: Parallel Execution
**Time**: 1 hour
**Prerequisites**: Lesson 29

**Setup**:
Use a multi-component project

**Tasks**:
1. Ask Claude to analyze 5 files in parallel
2. Have Claude search for multiple patterns simultaneously
3. Request multiple code explanations at once
4. Compare parallel vs sequential performance

**Verification**:
- [ ] Observed parallel tool calls
- [ ] All tasks completed
- [ ] Noted performance improvement

---

### Exercise 10: Advanced Skill with Templates
**Time**: 2 hours
**Prerequisites**: Lessons 37-38

**Setup**:
```bash
mkdir -p ~/.claude/skills/api-generator/{templates,examples}
```

**Tasks**:
1. Create skill that generates CRUD API endpoints
2. Include route template
3. Include controller template
4. Include test template
5. Add dynamic context injection

**The skill should**:
- Accept entity name as argument
- Generate routes, controller, tests
- Follow REST conventions
- Include validation

**Verification**:
- [ ] Skill generates all files
- [ ] Templates are used correctly
- [ ] Generated code is valid
- [ ] Tests run successfully

---

## 🟠 Advanced Exercises

### Exercise 11: MCP Server Connection
**Time**: 2 hours
**Prerequisites**: Lessons 51-57

**Setup**:
Install MCP servers:
```bash
npx @modelcontextprotocol/create-server
```

**Tasks**:
1. Install GitHub MCP server
2. Configure authentication
3. Use GitHub tools through Claude
4. Install filesystem MCP server
5. Connect to a database MCP server

**Verification**:
- [ ] 3+ MCP servers installed
- [ ] Authentication working
- [ ] Can use MCP tools
- [ ] Resources accessible

---

### Exercise 12: Custom Subagent
**Time**: 3 hours
**Prerequisites**: Lessons 63-67

**Setup**:
```bash
mkdir -p ~/.claude/subagents/test-runner
```

**Tasks**:
Create a subagent that:
1. Finds all test files
2. Runs tests
3. Analyzes failures
4. Suggests fixes
5. Generates coverage report

**Features**:
- Use Haiku model (fast, cheap)
- Limit to Read, Bash tools
- Include clear instructions
- Handle multiple test frameworks

**Verification**:
- [ ] Subagent finds tests
- [ ] Runs tests correctly
- [ ] Provides useful analysis
- [ ] Coverage report generated

---

### Exercise 13: Plan Mode Mastery
**Time**: 2 hours
**Prerequisites**: Lessons 71-74

**Setup**:
Use a complex refactoring task

**Tasks**:
1. Start Claude in plan mode
2. Request a significant refactoring
3. Review the exploration phase
4. Approve or modify the plan
5. Execute and verify

**Example Refactoring**:
- Convert callbacks to async/await
- Migrate from REST to GraphQL
- Refactor class components to hooks
- Extract shared logic to utils

**Verification**:
- [ ] Plan was comprehensive
- [ ] Exploration was thorough
- [ ] Had opportunity to modify plan
- [ ] Execution matched plan

---

### Exercise 14: IDE Integration
**Time**: 1 hour
**Prerequisites**: Lessons 75-78

**Setup**:
Install VS Code extension or JetBrains plugin

**Tasks**:
1. Complete a task entirely within IDE
2. Use @-mentions for files
3. Use line range references
4. Review changes in editor
5. Manage multiple conversations

**Verification**:
- [ ] IDE extension working
- [ ] Inline diffs visible
- [ ] References work
- [ ] Multiple tabs managed

---

### Exercise 15: Building Custom MCP Server
**Time**: 4 hours
**Prerequisites**: Lessons 59-60

**Setup**:
```bash
mkdir ~/claude-exercises/ex15-mcp-server
cd ~/claude-exercises/ex15-mcp-server
npm init -y
```

**Tasks**:
Build MCP server for a weather API:

1. Set up MCP server structure
2. Implement tools:
   - getCurrentWeather(location)
   - getForecast(location, days)
   - getAlerts(location)
3. Add caching (5 min TTL)
4. Handle rate limits
5. Add comprehensive error handling

**Verification**:
- [ ] MCP server starts
- [ ] All tools work
- [ ] Caching reduces API calls
- [ ] Errors handled gracefully
- [ ] Can use through Claude

---

## 🔴 Expert Exercises

### Exercise 16: Enterprise Plugin
**Time**: 6 hours
**Prerequisites**: Lessons 81-86

**Setup**:
```bash
mkdir -p ~/claude-exercises/ex16-enterprise-plugin/{skills,subagents,hooks,mcp}
```

**Tasks**:
Create comprehensive enterprise plugin:

1. **Skills**:
   - Code review
   - Security audit
   - Performance analysis

2. **Subagents**:
   - Test generator
   - Documentation writer

3. **Hooks**:
   - Auto-format
   - Auto-lint
   - Git staging

4. **MCP** (optional):
   - Company API access

**Verification**:
- [ ] All components work together
- [ ] Well documented
- [ ] Tested on real projects
- [ ] Shareable package

---

### Exercise 17: Git Worktree Workflow
**Time**: 2 hours
**Prerequisites**: Lesson 87

**Setup**:
```bash
cd ~/claude-exercises
git init ex17-worktree-main
cd ex17-worktree-main
git commit --allow-empty -m "Initial"
```

**Tasks**:
1. Create 3 worktrees for different features
2. Work on all 3 simultaneously with Claude
3. Coordinate changes
4. Merge all branches
5. Clean up worktrees

**Verification**:
- [ ] 3 parallel Claude sessions
- [ ] All features completed
- [ ] No conflicts
- [ ] Worktrees cleaned up

---

### Exercise 18: CI/CD Integration
**Time**: 3 hours
**Prerequisites**: Lesson 89

**Setup**:
Create GitHub repository with Actions

**Tasks**:
1. Create workflow that uses Claude for code review
2. Auto-generate docs on PR
3. Run security scan with Claude
4. Post results as PR comments
5. Block merge on critical issues

**Verification**:
- [ ] Workflow runs on PR
- [ ] Claude analysis appears
- [ ] Docs updated
- [ ] Security scan works
- [ ] Blocking works

---

### Exercise 19: Performance Optimization Project
**Time**: 4 hours
**Prerequisites**: Lessons 93-94

**Setup**:
Use a slow application

**Tasks**:
1. Profile the application with Claude
2. Identify bottlenecks
3. Optimize database queries
4. Implement caching
5. Benchmark improvements

**Target**:
- 50% reduction in response time
- Reduced database queries
- Lower memory usage

**Verification**:
- [ ] Profiling completed
- [ ] Bottlenecks identified
- [ ] Optimizations implemented
- [ ] Benchmarks show improvement
- [ ] No functionality broken

---

### Exercise 20: Complete Project
**Time**: 8+ hours
**Prerequisites**: All lessons

**Setup**:
Build a real-world application from scratch

**Project Ideas**:
- Task management API
- Blog platform
- E-commerce backend
- Real-time chat
- Analytics dashboard

**Requirements**:
1. Complete application architecture
2. Use Claude for entire development
3. Implement using all advanced techniques
4. Custom skills for project
5. Hooks for workflow
6. MCP integration
7. Comprehensive tests (>90% coverage)
8. Full documentation
9. CI/CD pipeline
10. Production deployment

**Must Use**:
- [ ] Plan mode for architecture
- [ ] Custom skills (3+)
- [ ] Hooks (5+)
- [ ] Subagents
- [ ] MCP servers (2+)
- [ ] Task management
- [ ] Git workflow
- [ ] IDE integration

**Verification**:
- [ ] Application is production-ready
- [ ] All requirements met
- [ ] Tests pass
- [ ] Deployed successfully
- [ ] Documentation complete

---

## 📊 Exercise Tracking

### Beginner (5 exercises)
- [ ] Exercise 1: First Steps
- [ ] Exercise 2: File Operations
- [ ] Exercise 3: Search & Find
- [ ] Exercise 4: Git Workflow
- [ ] Exercise 5: CLAUDE.md Creation

### Intermediate (5 exercises)
- [ ] Exercise 6: Task Management
- [ ] Exercise 7: First Custom Skill
- [ ] Exercise 8: Hook Configuration
- [ ] Exercise 9: Parallel Execution
- [ ] Exercise 10: Advanced Skill with Templates

### Advanced (5 exercises)
- [ ] Exercise 11: MCP Server Connection
- [ ] Exercise 12: Custom Subagent
- [ ] Exercise 13: Plan Mode Mastery
- [ ] Exercise 14: IDE Integration
- [ ] Exercise 15: Building Custom MCP Server

### Expert (5 exercises)
- [ ] Exercise 16: Enterprise Plugin
- [ ] Exercise 17: Git Worktree Workflow
- [ ] Exercise 18: CI/CD Integration
- [ ] Exercise 19: Performance Optimization Project
- [ ] Exercise 20: Complete Project

---

## 💡 Tips for Exercises

### Before Starting
- Read the corresponding lessons
- Set up clean environment
- Allocate enough time
- Have reference materials ready

### During Exercise
- Follow instructions step-by-step
- Take notes on learnings
- Don't skip verification
- Ask Claude for help if stuck

### After Completing
- Review what you learned
- Document any issues
- Share with community
- Move to next exercise

---

## 🏆 Exercise Completion Badges

Earn badges by completing exercise sets:

- **Beginner Badge**: Complete exercises 1-5
- **Intermediate Badge**: Complete exercises 6-10
- **Advanced Badge**: Complete exercises 11-15
- **Expert Badge**: Complete exercises 16-20
- **Master Badge**: Complete all 20 exercises

---

**Start with Exercise 1 and work your way up!**
