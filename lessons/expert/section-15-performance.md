# Section 15: Performance & Optimization (Lessons 93-96)

## Overview
Optimize Claude Code for maximum efficiency, cost-effectiveness, and developer productivity.

---

## Lesson 93: Performance Optimization

### Objectives
- Optimize tool usage patterns
- Minimize redundant operations
- Leverage parallel execution
- Implement strategic caching

### Tool Usage Optimization

**Inefficient:**
```
You: "Read file1.js"
You: "Read file2.js"
You: "Read file3.js"
```

**Efficient:**
```
You: "Read @file1.js, @file2.js, and @file3.js"
```

Claude reads all three in parallel.

### Avoiding Redundant Operations

**Inefficient:**
```
You: "What's in this file?"
You: "Now tell me about the functions"
You: "What about the imports?"
```

**Efficient:**
```
You: "Analyze this file: explain the purpose, list functions, and summarize imports"
```

Single operation covers all questions.

### Parallel Execution Patterns

**Best for parallel:**
- Multiple file reads
- Independent searches
- Separate analyses
- Non-conflicting edits

**Keep sequential:**
- Dependent operations
- State-changing commands
- Ordered workflows

### Glob vs Grep Strategy

```
Finding files: Use Glob
  "Find all *.test.ts files"

Searching content: Use Grep
  "Find files containing 'useState'"

Both: Chain them
  "Find test files, then search for mocking patterns"
```

### Response Size Optimization

```
You: "Give me a brief summary" → Short response
You: "Explain in detail" → Long response
```

Match request to need.

### Exercise 93: Optimize Common Workflow

**Task**: Measure and improve a workflow

1. Time a common task (e.g., code review)
2. Identify redundant operations
3. Restructure for efficiency
4. Time the optimized version
5. Calculate improvement

**Verification**:
- [ ] Baseline measured
- [ ] Inefficiencies identified
- [ ] Optimization applied
- [ ] Improvement quantified

---

## Lesson 94: Cost Management

### Objectives
- Understand token-based pricing
- Monitor usage effectively
- Choose appropriate models
- Balance cost vs capability

### Understanding Costs

```
Tokens = Input + Output + Thinking

Input: Files read, context, history
Output: Claude's responses
Thinking: Extended thinking mode
```

### Model Cost Comparison

| Model | Input Cost | Output Cost | Best For |
|-------|------------|-------------|----------|
| Haiku | $ | $ | Simple tasks |
| Sonnet | $$ | $$ | Most work |
| Opus | $$$$ | $$$$ | Complex tasks |

### Cost Reduction Strategies

**1. Model Selection:**
```
Simple: "Format this code" → Haiku
Normal: "Review this PR" → Sonnet
Complex: "Architect this system" → Opus
```

**2. Context Management:**
```bash
/compact  # Reduce context regularly
```

**3. Efficient Prompting:**
```
Avoid: "Can you maybe possibly look at..."
Better: "Review this function for bugs"
```

### Monitoring Usage

```bash
# Check current session usage
/usage

# Review historical usage
claude usage --last-month
```

### Budget Controls

```json
{
  "budget": {
    "daily": 10.00,
    "monthly": 200.00,
    "warningThreshold": 0.8
  }
}
```

### Exercise 94: Cost Analysis

**Task**: Analyze and reduce costs

1. Review last week's usage
2. Identify high-cost sessions
3. Determine if appropriate model was used
4. Implement cost-saving changes
5. Track improvement

**Verification**:
- [ ] Usage reviewed
- [ ] High-cost areas identified
- [ ] Optimizations applied
- [ ] Costs reduced

---

## Lesson 95: Debugging Claude Code

### Objectives
- Use verbose mode effectively
- Enable and interpret logging
- Debug hook issues
- Troubleshoot MCP problems

### Verbose Mode

```bash
# Enable verbose output
claude --verbose

# Shows:
# - Tool calls being made
# - API requests
# - Hook execution
# - Timing information
```

### Logging Configuration

```json
{
  "logging": {
    "level": "debug",
    "file": "~/.claude/debug.log",
    "includeToolCalls": true,
    "includeTokenCounts": true
  }
}
```

### Common Issues

**Issue: Edit tool fails with "string not found"**
```
Debug:
1. Read file to see exact content
2. Check for whitespace differences
3. Use replace_all for multiple occurrences
4. Copy exact string from Read output
```

**Issue: Hooks not triggering**
```
Debug:
1. Enable verbose mode
2. Check matcher pattern
3. Verify settings.json syntax
4. Test command manually
```

**Issue: MCP connection fails**
```
Debug:
1. Check server is running
2. Verify configuration path
3. Test with simple MCP tool
4. Check authentication
```

### Debugging Workflow

```
1. Reproduce the issue
2. Enable verbose mode
3. Check logs
4. Isolate the problem
5. Test fix
6. Verify resolution
```

### Exercise 95: Debug a Problem

**Task**: Practice systematic debugging

1. Create intentional issue (wrong matcher pattern)
2. Observe the failure
3. Enable verbose mode
4. Identify the problem in logs
5. Fix and verify

**Verification**:
- [ ] Issue reproduced
- [ ] Verbose mode used
- [ ] Problem identified in logs
- [ ] Fix verified

---

## Lesson 96: Terminal Configuration

### Objectives
- Optimize terminal settings
- Configure shell integration
- Set up efficient aliases
- Master keyboard shortcuts

### Terminal Recommendations

**Font:**
- Monospace with ligatures
- Good unicode support
- Clear at small sizes

**Size:**
- Width: 120+ columns
- Height: 40+ rows

**Colors:**
- High contrast theme
- Distinguishable diff colors

### Shell Integration

**Zsh setup (.zshrc):**
```bash
# Claude aliases
alias c="claude"
alias cr="claude --resume"
alias cp="claude --plan"

# Quick functions
cq() { claude --print "$*"; }
cf() { claude --print "Explain this file" < "$1"; }

# Completion
eval "$(claude completion zsh)"
```

**Bash setup (.bashrc):**
```bash
alias c="claude"
alias cr="claude --resume"

cq() { claude --print "$*"; }

# Completion
eval "$(claude completion bash)"
```

### Useful Aliases

```bash
# Session management
alias cs="claude --session"
alias ch="claude --history"

# Mode shortcuts
alias cv="claude --verbose"
alias ct="claude --thinking"

# Quick operations
alias cgit="git status && git diff | claude --print 'Review these changes'"
alias cdoc="claude --print 'Generate documentation for this code' <"
```

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel/interrupt |
| `Ctrl+D` | End input |
| `Ctrl+L` | Clear screen |
| `Ctrl+R` | Search history |
| `Tab` | Auto-complete |

### iTerm2/Terminal.app/Windows Terminal

**iTerm2 profile:**
- Enable shell integration
- Set working directory persistence
- Configure hotkey window

**Windows Terminal:**
- Use WSL2 for best experience
- Configure starting directory
- Set color scheme

### Exercise 96: Terminal Optimization

**Task**: Perfect your terminal setup

1. Add 5 Claude aliases
2. Configure shell completion
3. Test keyboard shortcuts
4. Optimize terminal appearance
5. Document your setup

**Verification**:
- [ ] Aliases working
- [ ] Completion enabled
- [ ] Shortcuts memorized
- [ ] Terminal comfortable

---

## Section 15 Summary

### Key Takeaways

1. **Tool efficiency** through parallel operations
2. **Cost management** with appropriate models
3. **Debugging** with verbose mode and logs
4. **Terminal optimization** for smooth workflow

### Optimization Checklist

- [ ] Use parallel operations
- [ ] Choose appropriate model
- [ ] Compact regularly
- [ ] Efficient prompting
- [ ] Shell aliases set up
- [ ] Completion enabled
- [ ] Keyboard shortcuts learned

### Performance Metrics

Track:
- Average response time
- Token usage per session
- Cost per task type
- Error frequency

### Next Steps
Proceed to Section 16: Enterprise & Security for organization-wide deployment.
