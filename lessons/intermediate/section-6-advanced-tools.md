# Section 6: Advanced Tool Usage (Lessons 26-32)

## Overview
Master subagents, parallel execution, background tasks, and context management for complex workflows.

---

## Lesson 26: Task Tool & Subagents Introduction

### Objectives
- Understand what subagents are and when Claude uses them
- Learn built-in subagent types
- Know when Claude auto-delegates vs manual invocation
- Launch subagents explicitly

### What Are Subagents?

Subagents are specialized Claude instances launched for specific tasks. They:
- Have focused capabilities
- Can run independently
- Return results to the main conversation
- May have different tool access

### Built-in Subagent Types

| Type | Purpose | Tools |
|------|---------|-------|
| **Explore** | Codebase analysis | Read-only (Glob, Grep, Read) |
| **Plan** | Implementation planning | Read-only + planning |
| **general-purpose** | Complex multi-step tasks | Full tool access |
| **Bash** | Command execution | Bash only |

### When Claude Auto-Delegates

Claude automatically uses subagents for:
- Large codebase exploration
- Complex research tasks
- Multi-step operations
- Parallel work

### Manual Invocation

```
"Use the Explore agent to map out the authentication system"
"Launch a subagent to research error handling patterns"
"Start a background agent to analyze all test files"
```

### Exercise 26: Launch Your First Subagent

**Task**: Explicitly use the Task tool

1. Ask Claude: "Use the Explore agent to find all API endpoints in this project"
2. Observe how the subagent works
3. Review the returned results

**Verification**:
- [ ] Subagent launched successfully
- [ ] Results returned to main conversation
- [ ] Understand the delegation pattern

---

## Lesson 27: Explore Subagent Deep Dive

### Objectives
- Use Explore for comprehensive codebase analysis
- Set appropriate thoroughness levels
- Understand read-only restrictions
- Get actionable architectural insights

### Thoroughness Levels

```
"Use Explore with quick thoroughness to find the main entry point"
"Use Explore with medium thoroughness to understand the routing"
"Use Explore with very thorough to map the entire architecture"
```

| Level | Use Case | Depth |
|-------|----------|-------|
| quick | Specific file/function | Surface level |
| medium | Module understanding | Related files |
| very thorough | Full architecture | Comprehensive |

### Read-Only Safety

Explore agents can only:
- Read files
- Search with Glob/Grep
- Analyze structure

They cannot:
- Write or edit files
- Run commands
- Make changes

This makes them safe for exploration without risk.

### Effective Explore Prompts

**Good prompts:**
```
"Explore how authentication flows through the application"
"Map the database models and their relationships"
"Find all places where user permissions are checked"
"Understand the testing strategy and patterns used"
```

**Less effective:**
```
"Look at the code" (too vague)
"Find stuff" (no direction)
```

### Exercise 27: Deep Codebase Analysis

**Task**: Use Explore for architectural understanding

1. Ask Claude: "Use Explore agent with 'very thorough' to analyze the project structure and identify:
   - Entry points
   - Core modules
   - Data flow patterns
   - Testing approach"

2. Review the comprehensive analysis
3. Ask follow-up questions about findings

**Verification**:
- [ ] Received detailed architectural overview
- [ ] Key patterns identified
- [ ] Can ask informed follow-up questions

---

## Lesson 28: General-Purpose Subagent

### Objectives
- Launch subagents for complex tasks
- Provide clear, complete task descriptions
- Monitor subagent progress
- Resume interrupted subagents

### When to Use General-Purpose

- Multi-file refactoring
- Complex feature implementation
- Tasks requiring many tool calls
- Independent work streams

### Crafting Task Descriptions

**Effective description:**
```
"Launch a subagent to:
1. Find all deprecated API calls in src/api/
2. Update them to use the new API format
3. Update related tests
4. Document changes made"
```

**Weak description:**
```
"Fix the API stuff"
```

### Monitoring Progress

Subagents report:
- What they're doing
- Files modified
- Issues encountered
- Completion status

### Resuming Subagents

If interrupted:
```
"Resume the previous subagent that was refactoring APIs"
```

### Exercise 28: Delegate Complex Work

**Task**: Use a subagent for multi-step work

1. Create a clear, multi-step task:
   "Launch a subagent to:
   - Find all console.log statements
   - Replace with proper logger calls
   - Add logger import where missing
   - Report what was changed"

2. Monitor the subagent's progress
3. Review completed work

**Verification**:
- [ ] Task delegated successfully
- [ ] Progress visible
- [ ] Work completed correctly

---

## Lesson 29: Parallel Execution

### Objectives
- Run multiple tool calls simultaneously
- Launch multiple subagents in parallel
- Understand when parallelization helps
- Handle parallel results effectively

### Automatic Parallelization

Claude automatically parallelizes when operations are independent:

```
You: "Read package.json, check git status, and list all TypeScript files"

Claude: [Runs all three in parallel, returns combined results]
```

### Explicit Parallel Requests

```
"In parallel:
1. Search for authentication code
2. Search for authorization code
3. Search for session management"
```

### Parallel Subagents

```
"Launch three Explore agents in parallel:
1. Analyze the frontend architecture
2. Analyze the backend architecture
3. Analyze the shared utilities"
```

### When to Parallelize

**Good for parallel:**
- Independent searches
- Multiple file reads
- Separate analysis tasks
- Non-conflicting work

**Keep sequential:**
- Dependent operations
- Read-then-modify patterns
- Ordered workflows

### Exercise 29: Parallel Analysis

**Task**: Run parallel operations

1. Ask Claude: "In parallel, use three Explore agents to analyze:
   - The authentication module
   - The database layer
   - The API routes"

2. Compare results from all three
3. Synthesize into overall understanding

**Verification**:
- [ ] Three analyses ran in parallel
- [ ] Results returned together
- [ ] Understand parallelization benefits

---

## Lesson 30: Background Tasks

### Objectives
- Run long-running tasks in background (Ctrl+B)
- Check on background task progress
- Use run_in_background parameter
- Manage multiple background tasks

### Starting Background Tasks

**Keyboard shortcut:**
- Press `Ctrl+B` when confirming a tool

**Explicit request:**
```
"Run the full test suite in the background"
"Start a background agent to analyze all 500 files"
```

### Checking Progress

```
"What's the status of my background task?"
"Check on the test run"
"/tasks"
```

### Use Cases

- Full test suites
- Large codebase analysis
- Long builds
- Comprehensive linting

### Managing Multiple Tasks

```
/tasks              # List all background tasks
/task <id>          # Check specific task
```

### Exercise 30: Background Test Run

**Task**: Use background execution

1. Start a long-running command in background:
   "Run npm test in the background so I can continue working"

2. Continue with other tasks
3. Check on the test progress
4. Review results when complete

**Verification**:
- [ ] Task started in background
- [ ] Could continue working
- [ ] Results retrieved successfully

---

## Lesson 31: Task Management System

### Objectives
- Use Claude's built-in TODO tracking
- Create and organize tasks
- Mark tasks in_progress and completed
- Coordinate complex work

### Creating Tasks

Claude can track work:
```
"Create tasks for:
1. Implement user login
2. Add password reset
3. Set up email verification
4. Add OAuth providers"
```

### Task Workflow

```
pending → in_progress → completed
```

### Viewing Tasks

```
"Show my current tasks"
"What's in progress?"
"List pending work"
```

### Updating Tasks

```
"Mark 'Implement user login' as in progress"
"Complete the password reset task"
"Add a new task: Set up 2FA"
```

### Benefits

- Tracks complex projects
- Maintains focus
- Shows progress
- Coordinates with subagents

### Exercise 31: Manage a Mini-Project

**Task**: Use task tracking for a feature

1. Break down a feature into tasks:
   "Create tasks for adding a user profile feature:
   - Create profile component
   - Add profile API endpoint
   - Write profile tests
   - Add profile navigation"

2. Work through tasks, updating status
3. Complete all tasks

**Verification**:
- [ ] Tasks created
- [ ] Status updates work
- [ ] Progress visible

---

## Lesson 32: Context & Token Management

### Objectives
- Understand token limits and their impact
- Use /compact effectively
- Monitor token usage
- Use @-mentions for efficient context

### Token Basics

- Claude has context limits
- More tokens = more capability but higher cost
- Long conversations can exceed limits
- Compaction preserves key information

### Signs of Context Issues

- Claude forgets earlier decisions
- Responses become less coherent
- Errors about context length

### Using /compact

```
/compact              # Compress conversation
/compact --aggressive # Maximum compression
```

**When to compact:**
- After completing a major task
- When context warning appears
- Before starting new work area
- Regularly in long sessions

### Efficient @-mentions

Instead of:
```
"Look at the authentication code in src/auth/login.js"
```

Use:
```
"Review @src/auth/login.js"
```

Benefits:
- Direct file reference
- Cleaner context
- Faster loading

### Best Practices

1. Compact after milestones
2. Use specific @-mentions
3. Start fresh sessions for new topics
4. Reference CLAUDE.md for persistent context

### Exercise 32: Context Management

**Task**: Practice context optimization

1. Have a moderate conversation (10+ exchanges)
2. Use `/compact` to compress
3. Verify key context preserved
4. Practice efficient @-mentions

**Verification**:
- [ ] Compaction successful
- [ ] Key information retained
- [ ] Efficient @-mention usage

---

## Section 6 Summary

### Key Takeaways

1. **Subagents** handle specialized tasks independently
2. **Explore agent** is safe for read-only analysis
3. **Parallel execution** speeds up independent work
4. **Background tasks** let you continue working
5. **Context management** maintains effectiveness

### Tools & Commands
- Task tool for subagent delegation
- Explore, Plan, general-purpose subagents
- Ctrl+B for background tasks
- `/compact` for context management
- `/tasks` for task tracking

### Next Steps
Proceed to Section 7: Skills System to create reusable expertise.
