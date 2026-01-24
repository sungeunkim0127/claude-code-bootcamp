# Section 10: Custom Subagents (Lessons 63-70)

## Overview
Build specialized AI agents with focused capabilities, controlled tool access, and optimized for specific workflows.

---

## Lesson 63: Subagent Architecture

### Objectives
- Understand subagent vs skills differences
- Learn subagent lifecycle and execution
- Know when custom subagents add value
- Understand subagent scope and isolation

### Subagent vs Skills

| Aspect | Subagents | Skills |
|--------|-----------|--------|
| Execution | Independent process | Same conversation |
| Context | Fresh context | Inherits context |
| Tools | Configurable access | Uses main tools |
| Model | Can specify different | Uses main model |
| Use Case | Complex autonomous work | Guided workflows |

### Subagent Lifecycle

```
Main conversation requests subagent
            ↓
    Subagent spawned
            ↓
    Works independently
            ↓
    Returns results
            ↓
    Main conversation continues
```

### When to Create Custom Subagents

**Create subagent when:**
- Task is autonomous and complex
- Different tool permissions needed
- Different model appropriate
- Work should be isolated

**Use skill instead when:**
- Work is interactive
- Context sharing important
- Simple workflow sufficient

### Subagent Scopes

```
~/.claude/agents/           # User subagents
.claude/agents/             # Project subagents
plugin/agents/              # Plugin subagents
```

### Exercise 63: Analyze Built-in Subagents

**Task**: Understand existing subagent design

1. Explore how the Explore agent works
2. Identify its tool restrictions
3. Note its specialized behavior
4. Document design patterns

**Verification**:
- [ ] Understand Explore agent design
- [ ] Know its tool limitations
- [ ] Can identify design patterns

---

## Lesson 64: Creating Basic Subagents

### Objectives
- Create SUBAGENT.md file structure
- Write effective subagent instructions
- Configure subagent frontmatter
- Test and iterate on subagent

### Subagent File Structure

```
.claude/agents/
└── code-reviewer/
    ├── SUBAGENT.md        # Main instructions
    └── templates/         # Optional supporting files
        └── review-template.md
```

### Basic SUBAGENT.md

```markdown
---
name: code-reviewer
description: Reviews code for quality, security, and best practices
model: sonnet
tools:
  - Read
  - Glob
  - Grep
---

# Code Review Agent

You are a specialized code review agent. Your job is to thoroughly review code for:

## Review Criteria

1. **Code Quality**
   - Readability and clarity
   - Proper naming conventions
   - Appropriate comments

2. **Security**
   - Input validation
   - Authentication/authorization
   - Sensitive data handling

3. **Performance**
   - Efficient algorithms
   - Resource management
   - Caching opportunities

4. **Best Practices**
   - SOLID principles
   - DRY (Don't Repeat Yourself)
   - Error handling

## Review Process

1. Read the file(s) to be reviewed
2. Analyze against each criteria
3. Identify issues with severity levels
4. Provide specific recommendations
5. Return structured review report

## Output Format

```markdown
# Code Review Report

## Summary
[Brief overview]

## Issues Found

### Critical
- [Issue]: [Location] - [Recommendation]

### Major
- [Issue]: [Location] - [Recommendation]

### Minor
- [Issue]: [Location] - [Recommendation]

## Positive Notes
- [What was done well]

## Recommendations
- [Prioritized improvements]
```
```

### Exercise 64: Create Your First Subagent

**Task**: Build a code review subagent

1. Create `.claude/agents/code-reviewer/SUBAGENT.md`
2. Define review criteria and process
3. Test on a code file
4. Refine based on results

**Verification**:
- [ ] Subagent file created
- [ ] Can invoke subagent
- [ ] Produces useful reviews

---

## Lesson 65: Subagent Tool Access

### Objectives
- Grant specific tools to subagents
- Create read-only subagents safely
- Allow write operations selectively
- Understand security implications

### Tool Access Configuration

```yaml
---
tools:
  - Read         # Can read files
  - Glob         # Can search for files
  - Grep         # Can search in files
  # No Edit, Write, or Bash = read-only
---
```

### Read-Only Subagent

```yaml
---
name: security-auditor
tools:
  - Read
  - Glob
  - Grep
# Cannot modify anything - safe for auditing
---
```

### Selective Write Access

```yaml
---
name: test-generator
tools:
  - Read
  - Glob
  - Grep
  - Write    # Only creates new files
  # No Edit = cannot modify existing
---
```

### Full Access (Careful!)

```yaml
---
name: refactorer
tools:
  - Read
  - Glob
  - Grep
  - Edit
  - Write
  - Bash    # Full capability - use with caution
---
```

### Security Considerations

1. **Principle of least privilege** - Only grant needed tools
2. **Read-only for analysis** - Auditing shouldn't modify
3. **Bash is powerful** - Limit carefully
4. **Test thoroughly** - Before production use

### Exercise 65: Build a Safe Auditor

**Task**: Create a security-focused read-only subagent

1. Create a security auditor subagent
2. Only allow Read, Glob, Grep
3. Instruct it to find security issues
4. Verify it cannot modify files

**Verification**:
- [ ] Subagent is read-only
- [ ] Finds security issues
- [ ] Cannot make changes

---

## Lesson 66: Subagent Model Selection

### Objectives
- Choose appropriate models for tasks
- Optimize for cost vs capability
- Use Haiku for simple tasks
- Use Opus for complex reasoning

### Model Options

| Model | Best For | Cost | Speed |
|-------|----------|------|-------|
| haiku | Simple tasks | Low | Fast |
| sonnet | Most work | Medium | Medium |
| opus | Complex reasoning | High | Slower |

### Model Configuration

```yaml
---
name: quick-formatter
model: haiku
tools:
  - Read
  - Edit
---

# Simple Code Formatter
Format code according to project standards.
```

```yaml
---
name: architecture-advisor
model: opus
tools:
  - Read
  - Glob
  - Grep
---

# Architecture Analysis Agent
Provide deep architectural analysis and recommendations.
```

### Cost Optimization Strategy

1. **Haiku** for:
   - Simple formatting
   - Basic validation
   - Quick lookups
   - Repetitive tasks

2. **Sonnet** for:
   - Code review
   - Feature implementation
   - General development
   - Most tasks

3. **Opus** for:
   - Complex refactoring
   - Architecture decisions
   - Difficult debugging
   - Multi-system analysis

### Exercise 66: Model-Appropriate Agents

**Task**: Create three agents with different models

1. Haiku agent for quick file formatting
2. Sonnet agent for code review
3. Opus agent for architecture analysis
4. Compare outputs and costs

**Verification**:
- [ ] Each agent uses appropriate model
- [ ] Tasks complete successfully
- [ ] Understand cost/capability tradeoffs

---

## Lesson 67: Subagent Preloaded Skills

### Objectives
- Load skills into subagents automatically
- Create specialized expert agents
- Combine skills for powerful workflows
- Optimize skill loading

### Preloading Skills

```yaml
---
name: full-stack-developer
model: sonnet
tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
preloaded_skills:
  - code-review
  - test-generator
  - documentation
---

# Full-Stack Development Agent

You have access to specialized skills for code review, test generation,
and documentation. Use them appropriately during development.
```

### Skill Combinations

**Testing Agent:**
```yaml
preloaded_skills:
  - test-generator
  - coverage-analyzer
  - mock-builder
```

**Documentation Agent:**
```yaml
preloaded_skills:
  - api-documenter
  - readme-generator
  - changelog-updater
```

### Benefits

1. Specialized expertise immediately available
2. Consistent behavior across invocations
3. Complex workflows automated
4. Domain knowledge encapsulated

### Exercise 67: Expert Agent Creation

**Task**: Build an agent with multiple skills

1. Create or identify 2-3 related skills
2. Create an agent that preloads them
3. Test the combined capabilities
4. Document the workflow

**Verification**:
- [ ] Skills loaded successfully
- [ ] Agent uses skills appropriately
- [ ] Combined workflow is effective

---

## Lesson 68: Subagent Hooks

### Objectives
- Configure hooks specific to subagents
- Isolate hook behavior per agent
- Share common hooks across agents
- Override parent hooks when needed

### Agent-Specific Hooks

```yaml
---
name: strict-developer
tools:
  - Read
  - Write
  - Edit
hooks:
  PostToolUse:
    - matcher: "Write|Edit:.*\\.ts$"
      command: "npx eslint --fix $FILE_PATH && npx prettier --write $FILE_PATH"
---
```

### Inheriting vs Overriding

By default, subagents inherit project/user hooks. Override with:

```yaml
hooks:
  inherit: false  # Don't inherit parent hooks
  PostToolUse:
    - ...  # Only these hooks apply
```

### Common Hook Patterns

**Quality enforcement:**
```yaml
hooks:
  PostToolUse:
    - matcher: "Write|Edit:.*"
      command: "./scripts/quality-check.sh $FILE_PATH"
```

**Logging:**
```yaml
hooks:
  PostToolUse:
    - matcher: ".*"
      command: "echo '$TOOL_NAME: $FILE_PATH' >> agent.log"
```

### Exercise 68: Agent with Custom Hooks

**Task**: Create an agent with specialized hooks

1. Create an agent for strict TypeScript development
2. Add hooks for formatting and linting
3. Add logging for all operations
4. Test the enforcement

**Verification**:
- [ ] Hooks trigger correctly
- [ ] Formatting/linting enforced
- [ ] Operations logged

---

## Lesson 69: Background Subagents

### Objectives
- Run subagents in background
- Monitor long-running agents
- Handle agent failures gracefully
- Resume interrupted agents

### Background Execution

```
"Run the security-auditor agent in the background on the entire codebase"
```

Or programmatically:
```yaml
---
name: comprehensive-analyzer
background: true
timeout: 3600  # 1 hour max
---
```

### Monitoring Progress

```
"What's the status of the security audit?"
/tasks
```

### Handling Failures

```yaml
---
name: resilient-worker
on_error: continue  # Don't stop on individual file errors
retry: 3            # Retry failed operations
---
```

### Resuming Agents

```
"Resume the security-auditor agent from where it left off"
```

Agents save checkpoints for resumption.

### Exercise 69: Long-Running Analysis

**Task**: Run a comprehensive background analysis

1. Create an analysis agent for large codebase
2. Run it in background
3. Continue other work
4. Check progress periodically
5. Review final results

**Verification**:
- [ ] Agent runs in background
- [ ] Can check progress
- [ ] Results retrieved successfully

---

## Lesson 70: Advanced Subagent Patterns

### Objectives
- Create agent chains for complex workflows
- Implement agent delegation patterns
- Build specialized agent teams
- Optimize agent coordination

### Agent Chains

```yaml
---
name: feature-implementer
chain:
  - agent: planner
    output: implementation_plan
  - agent: coder
    input: $implementation_plan
  - agent: tester
    input: $implementation_plan
  - agent: documenter
---
```

### Agent Delegation

```yaml
---
name: project-lead
can_delegate_to:
  - code-reviewer
  - security-auditor
  - performance-analyzer
---

# Project Lead Agent

When you encounter specific needs, delegate to specialized agents:
- Code quality issues → code-reviewer
- Security concerns → security-auditor
- Performance problems → performance-analyzer
```

### Agent Teams

```yaml
---
name: development-team
agents:
  - name: frontend-dev
    focus: "React components and UI"
  - name: backend-dev
    focus: "API endpoints and database"
  - name: qa-engineer
    focus: "Testing and quality"
coordination: sequential  # or parallel
---
```

### Coordination Patterns

**Sequential:**
```
Agent A completes → Agent B starts → Agent C finishes
```

**Parallel:**
```
Agent A ─┐
Agent B ─┼→ Results combined
Agent C ─┘
```

**Hierarchical:**
```
Lead Agent
    ├── Worker A
    ├── Worker B
    └── Worker C
```

### Exercise 70: Multi-Agent Workflow

**Task**: Build a coordinated agent system

1. Create 3 specialized agents
2. Create a coordinator agent
3. Define the workflow
4. Execute on a real task
5. Evaluate effectiveness

**Verification**:
- [ ] Agents work together
- [ ] Coordination is effective
- [ ] Results are comprehensive

---

## Section 10 Summary

### Key Takeaways

1. **Subagents** run independently with their own context
2. **Tool access** should follow least privilege
3. **Model selection** balances cost and capability
4. **Skills** can be preloaded for expertise
5. **Agent patterns** enable complex workflows

### Subagent Components
- SUBAGENT.md - Core instructions
- Frontmatter - Configuration
- Tools - Capability grants
- Hooks - Automation rules
- Skills - Preloaded expertise

### Design Patterns
- Read-only auditors
- Model-appropriate agents
- Agent chains
- Delegation hierarchies
- Parallel teams

### Next Steps
Proceed to Section 11: Plan Mode & Thinking for strategic development workflows.
