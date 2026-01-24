# Section 11: Plan Mode & Extended Thinking (Lessons 71-74)

## Overview
Master strategic development workflows with plan mode for complex tasks and extended thinking for deep problem-solving.

---

## Lesson 71: Understanding Plan Mode

### Objectives
- Learn when plan mode adds value
- Understand read-only exploration phase
- Review and refine plans before execution
- Configure default plan mode behavior

### What Is Plan Mode?

Plan mode separates exploration from execution:

```
Normal Mode:
  Explore → Execute → Explore → Execute (interleaved)

Plan Mode:
  Explore → Explore → Plan → [User Review] → Execute
```

### When to Use Plan Mode

**Best for:**
- Complex refactoring
- Architecture changes
- Large features
- Unfamiliar codebases
- High-risk modifications

**Not needed for:**
- Simple bug fixes
- Small changes
- Familiar code
- Quick tasks

### Plan Mode Benefits

1. **Safety** - See plan before changes
2. **Understanding** - Deep exploration first
3. **Strategy** - Optimal approach developed
4. **Review** - Catch issues before execution
5. **Learning** - Understand codebase better

### Starting Plan Mode

```bash
# Start session in plan mode
claude --plan

# Or during session
Shift+Tab, Shift+Tab
```

### Exercise 71: First Plan Mode Experience

**Task**: Use plan mode for a complex task

1. Start a new session with `claude --plan`
2. Request: "Plan how to add user authentication to this project"
3. Watch the exploration phase
4. Review the generated plan
5. Approve and observe execution

**Verification**:
- [ ] Entered plan mode
- [ ] Exploration was read-only
- [ ] Plan was reviewable
- [ ] Execution followed plan

---

## Lesson 72: Plan Mode Workflow

### Objectives
- Master the Shift+Tab activation
- Work effectively with plan files
- Approve, modify, or reject plans
- Transition smoothly to execution

### Plan Mode Phases

```
┌─────────────────────────────────────────┐
│           EXPLORATION PHASE             │
│  (Read-only: Glob, Grep, Read only)     │
│                                         │
│  Claude explores codebase               │
│  Understands current state              │
│  Identifies patterns                    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│            PLANNING PHASE               │
│                                         │
│  Claude writes plan to file             │
│  Specifies steps and changes            │
│  Identifies files to modify             │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│            REVIEW PHASE                 │
│                                         │
│  You review the plan                    │
│  Can request modifications              │
│  Approve or reject                      │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│           EXECUTION PHASE               │
│  (Full tools available)                 │
│                                         │
│  Claude implements plan                 │
│  Makes changes as specified             │
│  Verifies completion                    │
└─────────────────────────────────────────┘
```

### Plan File Location

Plans are saved to:
```
~/.claude/plans/[unique-name].md
```

### Reviewing Plans

```
You: "Show me the plan"
You: "Explain step 3 in more detail"
You: "What files will this modify?"
You: "What are the risks?"
```

### Modifying Plans

```
You: "Change the approach for authentication to use JWT instead"
You: "Add a step to create migration files"
You: "Remove the caching component for now"
```

### Approving/Rejecting

```
You: "This plan looks good, proceed"
You: "I want to modify step 2 first"
You: "Let's abandon this plan and try a different approach"
```

### Exercise 72: Full Plan Cycle

**Task**: Complete a full plan mode workflow

1. Enter plan mode: `Shift+Tab` twice
2. Request a medium-complexity feature
3. Let Claude explore
4. Review the generated plan
5. Request one modification
6. Approve the modified plan
7. Watch execution

**Verification**:
- [ ] Completed exploration phase
- [ ] Plan was generated
- [ ] Modification was applied
- [ ] Execution succeeded

---

## Lesson 73: Extended Thinking

### Objectives
- Understand extended thinking benefits
- Enable and configure thinking mode
- Configure thinking budgets appropriately
- Compare with and without thinking

### What Is Extended Thinking?

Extended thinking gives Claude more "reasoning time" for complex problems:

```
Normal:
  Question → Answer (direct response)

Extended Thinking:
  Question → [Internal reasoning] → Answer (deeper analysis)
```

### When Thinking Helps

- Complex architectural decisions
- Debugging difficult issues
- Multi-step problem solving
- Trade-off analysis
- Security considerations

### Enabling Extended Thinking

```bash
# Via command line
claude --thinking

# In conversation
/think
```

### Thinking Budget

```bash
# Set thinking token budget
claude --thinking-budget 10000
```

Budget considerations:
- Higher budget = more thorough reasoning
- Uses additional tokens (cost)
- Slower response time
- Better for complex tasks

### Verbose Mode

```bash
# See thinking process
claude --verbose
```

Shows Claude's reasoning steps.

### Exercise 73: Compare Thinking Modes

**Task**: Solve same problem with/without thinking

1. Without thinking: Ask Claude to design a caching strategy
2. Note the response depth and quality
3. Enable thinking: Same question
4. Compare the analyses
5. Document differences

**Verification**:
- [ ] Both modes produced answers
- [ ] Extended thinking was more thorough
- [ ] Understand when to use each

---

## Lesson 74: Plan Mode Best Practices

### Objectives
- Know when plan mode adds value vs overhead
- Optimize the plan review process
- Combine with other features effectively
- Avoid over-planning pitfalls

### When Plan Mode Adds Value

```
Task Complexity Assessment:

Simple (skip plan mode):
- Fix typo
- Add console.log
- Update version number

Medium (optional):
- Add new endpoint
- Write tests
- Small refactor

Complex (use plan mode):
- Multi-file refactor
- New feature system
- Architecture changes
- Security modifications
```

### Efficient Plan Review

**Don't review everything:**
```
Focus on:
- Overall approach
- Files to be modified
- Potential risks
- Dependencies

Skip:
- Implementation details (trust Claude)
- Exact code (will review in diffs)
```

**Quick approval:**
```
You: "This approach makes sense, proceed"
```

### Combining with Other Features

**Plan Mode + Subagents:**
```
1. Enter plan mode
2. Use Explore agent for analysis
3. Generate plan
4. Execute with full capabilities
```

**Plan Mode + Skills:**
```
1. Plan the overall approach
2. Execute using relevant skills
3. Skills provide consistent implementation
```

**Plan Mode + Hooks:**
```
1. Plan identifies files to modify
2. Execution triggers formatting hooks
3. Quality maintained automatically
```

### Anti-Patterns

**Over-planning:**
```
❌ Using plan mode for 5-line changes
❌ Requesting 50 revision cycles
❌ Planning obvious tasks
```

**Under-planning:**
```
❌ Skipping plan mode for architecture changes
❌ Rushing approval without review
❌ Ignoring risk warnings
```

### Exercise 74: Strategic Plan Mode Usage

**Task**: Develop your plan mode workflow

1. Identify 3 upcoming tasks
2. Assess each for plan mode appropriateness
3. For the complex one, use plan mode
4. For others, use normal mode
5. Reflect on the efficiency

**Verification**:
- [ ] Correctly assessed task complexity
- [ ] Used plan mode appropriately
- [ ] Workflow felt efficient

---

## Section 11 Summary

### Key Takeaways

1. **Plan mode** separates exploration from execution
2. **Exploration phase** is read-only for safety
3. **Plans** can be reviewed and modified
4. **Extended thinking** improves complex reasoning
5. **Balance** - avoid over and under-planning

### Activation Methods
- `claude --plan` - Start in plan mode
- `Shift+Tab, Shift+Tab` - Enter plan mode
- `claude --thinking` - Enable extended thinking
- `/think` - Toggle thinking in session

### Decision Framework

```
Is the task complex, risky, or unfamiliar?
    YES → Plan Mode
    NO  → Normal Mode

Do I need deep reasoning?
    YES → Extended Thinking
    NO  → Standard Mode
```

### Next Steps
Proceed to Section 12: IDE Integration for seamless development workflows.
