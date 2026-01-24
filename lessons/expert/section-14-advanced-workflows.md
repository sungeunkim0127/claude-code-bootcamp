# Section 14: Advanced Workflows (Lessons 87-92)

## Overview
Master professional-grade workflows for parallel development, CI/CD integration, and multi-repository operations.

---

## Lesson 87: Git Worktrees for Parallel Development

### Objectives
- Understand git worktrees concept
- Set up parallel development environment
- Run multiple Claude instances effectively
- Coordinate work across worktrees

### What Are Git Worktrees?

Worktrees let you have multiple branches checked out simultaneously:

```
project/                    # Main worktree (main branch)
project-feature-a/          # Worktree for feature A
project-feature-b/          # Worktree for feature B
project-bugfix/             # Worktree for bug fixes
```

### Creating Worktrees

```bash
# Create worktree for a new feature
git worktree add ../project-feature-auth -b feature/auth

# Create worktree for existing branch
git worktree add ../project-bugfix bugfix/login-error

# List worktrees
git worktree list
```

### Running Parallel Claude Sessions

**Terminal 1:**
```bash
cd ../project-feature-auth
claude --session "feature-auth"
```

**Terminal 2:**
```bash
cd ../project-feature-b
claude --session "feature-b"
```

### Coordination Strategy

```
Main worktree: Integration, reviews
Feature A worktree: Auth development
Feature B worktree: UI development
Bugfix worktree: Critical fixes
```

### Benefits

1. No branch switching interruption
2. Run tests in one, develop in another
3. Compare implementations side-by-side
4. Isolate risky experiments

### Cleanup

```bash
# Remove worktree
git worktree remove ../project-feature-auth

# Prune stale worktrees
git worktree prune
```

### Exercise 87: Parallel Development Setup

**Task**: Work on two features simultaneously

1. Create worktrees for two features
2. Start Claude in each
3. Work on both in parallel
4. Test in one while developing in other
5. Clean up when done

**Verification**:
- [ ] Worktrees created
- [ ] Claude sessions running in parallel
- [ ] No interference between features
- [ ] Cleanup successful

---

## Lesson 88: Claude as Unix Utility

### Objectives
- Use stdin/stdout with Claude
- Pipe data to and from Claude
- Chain Claude with other tools
- Build automated pipelines

### Piping to Claude

```bash
# Analyze log file
cat error.log | claude "What errors are in this log?"

# Review diff
git diff | claude "Review these changes"

# Process list
find . -name "*.ts" | claude "Which of these are test files?"
```

### Claude in Pipelines

```bash
# Generate and save
claude "Generate a React component for user profile" > Profile.tsx

# Transform data
cat data.json | claude "Convert to CSV format" > data.csv

# Filter and analyze
grep -r "TODO" src/ | claude "Categorize these TODOs by priority"
```

### One-Shot Mode

```bash
# Single question, output only
claude --print "What is the best way to parse JSON in Python?"

# With file context
claude --print "Explain this code" < complex.py
```

### Scripting Examples

**Batch analysis script:**
```bash
#!/bin/bash
for file in src/*.ts; do
    echo "=== $file ===" >> analysis.md
    cat "$file" | claude --print "Rate code quality 1-10 with brief explanation" >> analysis.md
done
```

**Git hook integration:**
```bash
#!/bin/bash
# pre-commit hook
git diff --cached | claude --print "Any security issues in these changes?" > /tmp/security.txt
if grep -q "CRITICAL" /tmp/security.txt; then
    cat /tmp/security.txt
    exit 1
fi
```

### Exercise 88: Pipeline Creation

**Task**: Build a Claude-powered pipeline

1. Create script that analyzes all changed files
2. Pipe git diff to Claude
3. Output analysis to file
4. Test on real changes

**Script:**
```bash
#!/bin/bash
git diff HEAD~1 | claude --print "
Review these changes:
1. List potential bugs
2. Note any security concerns
3. Suggest improvements
" > code-review.md
echo "Review saved to code-review.md"
```

**Verification**:
- [ ] Pipeline works end-to-end
- [ ] Output is useful
- [ ] Can be automated

---

## Lesson 89: CI/CD Integration

### Objectives
- Use Claude in CI pipelines
- Automate code reviews
- Generate documentation in CI
- Create intelligent build checks

### GitHub Actions Integration

```yaml
name: Claude Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude CLI
        run: npm install -g @anthropic-ai/claude-cli

      - name: Review Changes
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          git diff origin/main | claude --print "
          Review this PR for:
          - Code quality issues
          - Security vulnerabilities
          - Performance concerns
          - Missing tests
          Output as markdown checklist.
          " > review.md

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = fs.readFileSync('review.md', 'utf8');
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: review
            });
```

### Automated Documentation

```yaml
name: Generate Docs
on:
  push:
    branches: [main]
    paths: ['src/**']

jobs:
  docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Generate API Docs
        run: |
          for file in src/api/*.ts; do
            claude --print "Generate API documentation for this file" < "$file" >> docs/API.md
          done

      - name: Commit Docs
        run: |
          git add docs/
          git commit -m "docs: auto-generate API documentation"
          git push
```

### Quality Gates

```yaml
- name: Security Check
  run: |
    result=$(git diff origin/main | claude --print "
    Analyze for security issues. Output only:
    - PASS if no issues
    - FAIL: [reason] if issues found
    ")
    if [[ "$result" == FAIL* ]]; then
      echo "$result"
      exit 1
    fi
```

### Exercise 89: CI Integration

**Task**: Add Claude to CI workflow

1. Create GitHub Actions workflow
2. Add automated code review
3. Add documentation generation
4. Test on a PR

**Verification**:
- [ ] Workflow runs on PR
- [ ] Review comment posted
- [ ] Documentation updated
- [ ] Quality gate works

---

## Lesson 90: Context Management Strategies

### Objectives
- Optimize for large codebases
- Use auto-compaction effectively
- Balance context vs token cost
- Develop strategic conversation patterns

### Large Codebase Strategy

```
1. Start with high-level exploration
2. Narrow to specific areas
3. Compact before deep dives
4. Use @-mentions for precision
```

### Auto-Compaction Settings

```json
{
  "behavior": {
    "autoCompact": true,
    "compactThreshold": 75,
    "preserveDecisions": true
  }
}
```

### Context-Efficient Patterns

**Bad: Loads everything**
```
"Read all the files in src and explain the architecture"
```

**Good: Targeted exploration**
```
"Use Explore agent to understand the high-level architecture"
"Now focus on the auth module specifically"
```

### Session Strategies

**One topic per session:**
```
Session 1: Feature A development
Session 2: Bug investigation
Session 3: Refactoring
```

**Compact at milestones:**
```
"Let's /compact since we've finished the first phase"
```

### CLAUDE.md for Persistent Context

Move stable context to CLAUDE.md:
- Architecture decisions
- Coding standards
- Common patterns
- Project-specific knowledge

### Exercise 90: Context Optimization

**Task**: Develop context management strategy

1. Work on a complex task until context grows
2. Use /compact at appropriate points
3. Move repeated context to CLAUDE.md
4. Measure token efficiency

**Verification**:
- [ ] Compaction preserves key info
- [ ] CLAUDE.md reduces repetition
- [ ] Workflow remains smooth

---

## Lesson 91: Multi-Repository Workflows

### Objectives
- Work across multiple repositories
- Coordinate changes between repos
- Manage dependencies effectively
- Use monorepo strategies

### Multi-Repo Setup

```
workspace/
├── frontend/          # React app
├── backend/           # Node API
├── shared/            # Shared types
└── infrastructure/    # Terraform
```

### Cross-Repo Sessions

**Option 1: Multiple sessions**
```bash
# Terminal 1
cd frontend && claude --session "frontend"

# Terminal 2
cd backend && claude --session "backend"
```

**Option 2: Workspace session**
```bash
cd workspace
claude --session "full-stack"
# Can access all repos
```

### Coordinating Changes

```
"I need to add a new API endpoint. Show me:
1. The frontend code that will call it
2. The backend route structure
3. The shared types used"
```

### Dependency Management

```
"Update the shared types in @shared/types.ts
Then show me all places in @frontend and @backend
that need updating"
```

### Monorepo Pattern

For monorepos, use CLAUDE.md at root:
```markdown
# Monorepo Structure

## Packages
- packages/web - React frontend
- packages/api - Express backend
- packages/shared - Shared utilities

## Cross-Package Development
When modifying shared types:
1. Update packages/shared first
2. Run type check across all packages
3. Update consumers as needed
```

### Exercise 91: Cross-Repo Feature

**Task**: Implement feature across repositories

1. Identify a feature needing frontend + backend changes
2. Set up multi-repo session
3. Coordinate changes
4. Verify integration

**Verification**:
- [ ] Changes coordinated across repos
- [ ] Types/contracts stay in sync
- [ ] Integration works

---

## Lesson 92: Advanced Git Workflows

### Objectives
- Handle complex merge conflicts with Claude
- Perform sophisticated rebasing
- Manage complex branch operations
- Master git history manipulation

### Merge Conflict Resolution

```
"I have a merge conflict in @src/auth.ts.
Show me both versions and help me resolve it correctly."
```

Claude can:
- Show both sides clearly
- Explain the conflict
- Suggest resolution
- Apply the fix

### Interactive Rebasing

```
"Help me clean up the last 5 commits:
- Squash the WIP commits
- Reword commit messages
- Reorder logically"
```

Note: Use `git rebase` without `-i` as interactive isn't supported.

### Complex Branch Operations

**Cherry-picking:**
```
"Find the commit that fixed the auth bug and cherry-pick it to release branch"
```

**Branch comparison:**
```
"Compare feature/auth with main and summarize all differences"
```

### History Analysis

```
"Analyze the git history for @src/api.ts:
- When was it created?
- Who modified it most?
- What were the major changes?"
```

### Safe Operations

Claude helps with safety:
```
"Before force pushing, show me exactly what would be lost"
"Create a backup branch before rebasing"
```

### Exercise 92: Git History Cleanup

**Task**: Clean up messy commit history

1. Create a branch with messy commits
2. Use Claude to plan cleanup
3. Perform rebase and squash
4. Verify history is clean

**Verification**:
- [ ] History analyzed correctly
- [ ] Cleanup plan is sound
- [ ] History cleaned without data loss

---

## Section 14 Summary

### Key Takeaways

1. **Git worktrees** enable true parallel development
2. **Pipeline integration** automates Claude usage
3. **CI/CD** brings Claude to team workflows
4. **Context management** is crucial at scale
5. **Multi-repo** work requires coordination

### Commands & Tools
- `git worktree add/remove/list`
- `claude --print` for scripting
- GitHub Actions integration
- `/compact` for context management

### Patterns
- Parallel worktree sessions
- Unix pipeline integration
- Automated code review
- Cross-repo coordination
- Safe git operations

### Next Steps
Proceed to Section 15: Performance & Optimization for maximum efficiency.
