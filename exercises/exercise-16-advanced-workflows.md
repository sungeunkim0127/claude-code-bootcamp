# Exercise 16: Advanced Git Workflows

**Time**: 1 hour
**Prerequisites**: Lessons 87-92

## Objective
Master worktrees, piped stdin, CI/CD integration, and multi-repo workflows with Claude Code.

## Setup
```bash
mkdir ~/advanced-workflows-lab && cd ~/advanced-workflows-lab
git init
mkdir src tests
echo '{ "name": "workflow-lab", "version": "1.0.0" }' > package.json
echo 'export function add(a, b) { return a + b; }' > src/math.js
echo 'import { add } from "../src/math.js"; console.assert(add(1,2)===3);' > tests/math.test.js
git add -A && git commit -m "Initial commit"
git branch feature/auth && git branch feature/api && git branch bugfix/validation
```

## Tasks
### Task 1: Git Worktrees for Parallel Development (15 min)
Create worktrees and run Claude in each simultaneously:
```bash
git worktree add ../wt-auth feature/auth
git worktree add ../wt-api feature/api
git worktree add ../wt-bugfix bugfix/validation
```
Open three terminals. Run `claude` in each worktree directory. Give each a different task:
- **wt-auth**: "Add src/auth.js with login/logout functions using JWT. Include JSDoc."
- **wt-api**: "Add src/api.js with GET/POST handlers returning JSON. Include error handling."
- **wt-bugfix**: "Fix src/math.js to validate inputs are numbers, throw TypeError if not."

**Verify**: `git worktree list` shows three worktrees. `git -C ../wt-auth diff` shows unique changes per branch.
---
### Task 2: Claude as a Unix Utility (10 min)
Pipe data to Claude in non-interactive mode:
```bash
cat src/math.js | claude -p "Review this code for edge cases"
git -C ../wt-auth diff | claude -p "Write a conventional commit message for these changes"
find src/ -name "*.js" -exec cat {} + | claude -p "List all exported functions"
echo "name,age\nAlice,30\nBob,25" | claude -p "Convert this CSV to JSON"
```
**Verify**: Each command returns output without entering interactive mode. Commit message uses `feat:`/`fix:` format.
---
### Task 3: CI/CD Automated Review Workflow (15 min)
```bash
mkdir -p .github/workflows
```
Ask Claude: "Create `.github/workflows/claude-review.yml` that triggers on pull_request, gets the diff, pipes it to `claude -p` for review, and posts a PR comment. Use ANTHROPIC_API_KEY from secrets."

Then add a pre-push hook:
```bash
cat > .git/hooks/pre-push << 'EOF'
#!/bin/bash
DIFF=$(git diff @{upstream}...HEAD 2>/dev/null)
[ -n "$DIFF" ] && echo "$DIFF" | claude -p "Any obvious bugs or security issues? YES or NO with explanation."
EOF
chmod +x .git/hooks/pre-push
```
**Verify**: `.github/workflows/claude-review.yml` exists with valid YAML. Hook is executable: `ls -la .git/hooks/pre-push`.
---
### Task 4: Multi-Repository Coordination (10 min)
```bash
mkdir ~/workflow-lab-backend && cd ~/workflow-lab-backend && git init && mkdir src
echo 'export function handleRequest(req) { return { status: 200 }; }' > src/server.js
git add -A && git commit -m "Initial backend"
```
Start Claude here and say: "Read ~/advanced-workflows-lab/src/api.js, then update src/server.js to implement matching endpoints with aligned request/response formats."

**Verify**: `claude -p "Compare ~/advanced-workflows-lab/src/api.js and ~/workflow-lab-backend/src/server.js -- do formats match?"`
---
### Task 5: Headless Batch Automation (10 min)
Create `batch-review.sh` that reviews all JS files:
```bash
#!/bin/bash
REPO="${1:-.}" && echo "# Review Report" > review-report.md
for f in $(find "$REPO/src" -name "*.js"); do
  echo "## $(basename $f)" >> review-report.md
  cat "$f" | claude -p "Rate 1-5. Format: **Rating: N/5** then bullet points." >> review-report.md
done
```
```bash
chmod +x batch-review.sh && ./batch-review.sh ~/advanced-workflows-lab
```
**Verify**: `review-report.md` contains a section with a rating for each `.js` file.

## Completion Criteria
- [ ] Three worktrees created with independent Claude sessions
- [ ] Piped stdin to Claude and received structured output
- [ ] CI/CD workflow and pre-push hook created
- [ ] Multi-repo coordination with matching API contracts
- [ ] Batch review script produces formatted report

## Bonus Challenges
- Write `worktree-flow.sh` that creates a worktree, runs Claude with a prompt file, commits, and cleans up in one command: `./worktree-flow.sh feature/cache "Add caching to api.js"`
- Extend `batch-review.sh` to produce an HTML report with color-coded ratings (green 4-5, yellow 3, red 1-2).
