# Section 3: Git Integration (Lessons 13-17)

## Overview
Learn how Claude Code integrates with Git to manage your entire version control workflow. From checking status and making commits to managing branches, creating pull requests, and performing code reviews — Claude handles Git operations safely and efficiently.

---

## Lesson 13: Git Basics with Claude Code

### Objectives
- Check repository status with Claude
- View diffs and understand changes
- Review commit history
- Understand Git staging area

### Content

#### How Claude Interacts with Git

Claude uses the **Bash tool** to run Git commands on your behalf. It follows strict safety protocols:

- Never force pushes without explicit permission
- Never runs destructive commands (`reset --hard`, `clean -f`) without asking
- Always creates new commits rather than amending (unless you ask)
- Prefers staging specific files over `git add -A`

#### Checking Repository Status

**Basic Status**:
```
You: "Show me the git status"

Claude:
  [Bash: git status]
  → "You have 3 modified files and 1 untracked file:
     Modified:
       src/app.js
       src/config.js
       tests/app.test.js
     Untracked:
       src/newFeature.js"
```

**Quick Status Check**:
```
You: "What's changed since my last commit?"

Claude:
  [Bash: git status]
  [Bash: git diff --stat]
  → "3 files changed, 45 insertions, 12 deletions"
```

#### Viewing Diffs

**See All Changes**:
```
You: "Show me the diff"

Claude:
  [Bash: git diff]
  → Shows unstaged changes with + and - markers
```

**Staged Changes**:
```
You: "What's staged for commit?"

Claude:
  [Bash: git diff --staged]
  → Shows only staged changes
```

**Specific File Diff**:
```
You: "Show me what changed in src/app.js"

Claude:
  [Bash: git diff src/app.js]
  → Shows changes in that file only
```

#### Reviewing Commit History

**Recent Commits**:
```
You: "Show me recent commits"

Claude:
  [Bash: git log --oneline -10]
  → a1b2c3d Fix login validation
  → d4e5f6g Add user profile page
  → h7i8j9k Update dependencies
```

**Detailed History**:
```
You: "Show me the last 3 commits with details"

Claude:
  [Bash: git log -3 --stat]
  → Shows author, date, message, and files changed
```

**History for a File**:
```
You: "Show me the history of changes to auth.js"

Claude:
  [Bash: git log --oneline -- src/auth.js]
  → Commits that touched auth.js
```

#### Understanding the Staging Area

```
Working Directory → Staging Area → Repository
    (modified)      (git add)     (git commit)
```

**Check What's Where**:
```
You: "What files are staged vs unstaged?"

Claude:
  [Bash: git status]
  → "Staged (ready to commit):
       src/app.js
     Not staged:
       src/config.js
     Untracked:
       src/newFile.js"
```

### Exercise

**Task**: Explore a Git repository with Claude

**Setup**:
```bash
mkdir -p ~/claude-practice/git-demo && cd ~/claude-practice/git-demo
git init
echo "# Git Demo" > README.md
git add README.md && git commit -m "Initial commit"
echo "const app = 'hello'" > app.js
echo "const config = {}" > config.js
git add app.js && git commit -m "Add app.js"
echo "// updated" >> app.js
echo "const utils = {}" > utils.js
git add config.js
claude
```

**Tasks**:

1. **Check Status**:
```
"Show me the git status and explain what each section means"
```

2. **View Diffs**:
```
"Show me what's changed in the working directory vs what's staged"
```

3. **Review History**:
```
"Show me all commits in this repository"
```

4. **File History**:
```
"Show me the commit history for app.js"
```

5. **Full Overview**:
```
"Give me a complete overview of this repository:
status, recent changes, and commit history"
```

### Verification

Before moving on, ensure you can:
- [ ] Ask Claude to show git status
- [ ] View diffs (staged and unstaged)
- [ ] Review commit history
- [ ] Understand the staging area
- [ ] Get a repository overview

### Common Mistakes

1. **Not Checking Status Before Committing**:
   - Always review what will be committed
   - Ask Claude to show status first

2. **Confusing Staged vs Unstaged**:
   - Staged = ready for commit (`git add`)
   - Unstaged = modified but not yet added

3. **Ignoring Untracked Files**:
   - New files must be explicitly added
   - Check status to see untracked files

---

## Lesson 14: Making Commits

### Objectives
- Create well-structured commits with Claude
- Write effective commit messages
- Understand the Co-Authored-By convention
- Stage specific files selectively

### Content

#### How Claude Creates Commits

When you ask Claude to commit, it follows a careful protocol:

```
1. Run git status → see all changes
2. Run git diff   → review what will be committed
3. Run git log    → match commit message style
4. Stage files    → add specific files (not -A)
5. Create commit  → with descriptive message
6. Verify         → run git status after
```

#### Staging Files

**Specific Files** (preferred):
```
You: "Stage app.js and config.js"

Claude:
  [Bash: git add src/app.js src/config.js]
  → "Staged 2 files"
```

**By Pattern**:
```
You: "Stage all JavaScript files in src/"

Claude:
  [Bash: git add src/*.js]
```

**Review Before Staging**:
```
You: "Show me what would be committed, then stage it"

Claude:
  [Bash: git status]
  [Bash: git diff]
  → "Here's what would be committed..."
  [Bash: git add src/app.js src/utils.js]
```

#### Writing Commit Messages

Claude writes commit messages that:
- Focus on **why**, not just what
- Are concise (1-2 sentences for summary)
- Follow the repository's existing style
- End with `Co-Authored-By` tag

**Good Commit Messages**:
```
Fix null pointer in user lookup when user not found

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```

```
Add rate limiting to /api/login endpoint to prevent brute force attacks

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```

**Bad Commit Messages**:
```
fixed stuff          ← Too vague
update app.js        ← Describes what, not why
WIP                  ← Not descriptive
```

#### The Co-Authored-By Convention

Every commit Claude creates includes:
```
Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```

**Why This Matters**:
- Transparency about AI-assisted work
- Tracks which commits had AI involvement
- Required by some team policies
- Good practice for accountability

#### Complete Commit Workflow

```
You: "Commit the changes to the auth module"

Claude:
  [Bash: git status]
  → Modified: src/auth.js, src/middleware/auth.js

  [Bash: git diff]
  → Shows the changes

  [Bash: git log --oneline -5]
  → Recent commit style

  [Bash: git add src/auth.js src/middleware/auth.js]

  [Bash: git commit -m "Fix token validation to reject expired tokens

  Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"]

  [Bash: git status]
  → "Clean working tree — commit successful"
```

#### Handling Pre-commit Hooks

If a pre-commit hook fails:
```
Claude:
  [Bash: git commit -m "..."]
  → Hook failed: linting errors

  [Read: failing file]
  [Edit: fix linting issues]
  [Bash: git add fixed-file.js]
  [Bash: git commit -m "..."]  ← New commit, NOT --amend
```

**Important**: Claude always creates a **new** commit after hook failure, never amends.

### Exercise

**Task**: Practice making commits with Claude

**Setup**:
```bash
cd ~/claude-practice/git-demo
echo "function login(user) { return true }" > auth.js
echo "function getUser(id) { return null }" > users.js
echo "PORT=3000" > .env
claude
```

**Tasks**:

1. **Simple Commit**:
```
"Stage auth.js and commit it with a descriptive message"
```

2. **Multiple Files**:
```
"Stage users.js, commit with a message explaining what it does"
```

3. **Selective Staging**:
```
"Show me the status. Stage only the source files,
NOT the .env file. Commit the staged files."
```

4. **Review Then Commit**:
```
"Show me what would be committed, then create the commit"
```

5. **Verify History**:
```
"Show me all commits to verify everything looks correct"
```

### Verification

Before moving on, ensure you can:
- [ ] Ask Claude to create commits
- [ ] Stage specific files (not all files)
- [ ] Write descriptive commit messages
- [ ] Understand Co-Authored-By convention
- [ ] Verify commits after creation

### Common Mistakes

1. **Committing Sensitive Files**:
   - Never commit `.env`, credentials, or secrets
   - Claude warns about these automatically

2. **Staging Everything**:
   - `git add -A` can include unwanted files
   - Always stage specific files

3. **Vague Commit Messages**:
   - "fix bug" doesn't help future you
   - Describe what was fixed and why

### Pro Tips

**Batch Related Changes**:
```
"Group the auth changes into one commit and the
UI changes into a separate commit"
```

**Commit After Each Task**:
```
"After fixing each bug, create a commit so we have
a clean history"
```

---

## Lesson 15: Branch Management

### Objectives
- Create and switch branches with Claude
- Understand feature branch workflow
- Merge branches safely
- Handle merge conflicts

### Content

#### Branch Basics with Claude

**Create a Branch**:
```
You: "Create a new branch called feature/user-profile"

Claude:
  [Bash: git checkout -b feature/user-profile]
  → "Created and switched to branch 'feature/user-profile'"
```

**Switch Branches**:
```
You: "Switch to the main branch"

Claude:
  [Bash: git checkout main]
  → "Switched to branch 'main'"
```

**List Branches**:
```
You: "Show me all branches"

Claude:
  [Bash: git branch -a]
  → "Local branches:
       main
     * feature/user-profile
       feature/auth
     Remote branches:
       origin/main"
```

#### Feature Branch Workflow

The standard workflow Claude supports:

```
main ─────────────────────────────────────
        \                        /
         feature/xyz ──────────
         (create)   (work)   (merge)
```

**Complete Workflow**:
```
You: "Create a feature branch for the new search feature"

Claude:
  [Bash: git checkout -b feature/search]
  → "Created feature/search from main"

You: [... make changes with Claude ...]

You: "Merge this back into main"

Claude:
  [Bash: git checkout main]
  [Bash: git merge feature/search]
  → "Merged feature/search into main"
```

#### Branch Naming Conventions

Claude follows common patterns:
```
feature/description    → New features
bugfix/description     → Bug fixes
hotfix/description     → Urgent fixes
chore/description      → Maintenance tasks
docs/description       → Documentation updates
```

**Examples**:
```
feature/user-authentication
bugfix/login-500-error
hotfix/security-patch
chore/update-dependencies
```

#### Merging Branches

**Simple Merge**:
```
You: "Merge feature/search into main"

Claude:
  [Bash: git checkout main]
  [Bash: git merge feature/search]
  → "Merge successful, no conflicts"
```

**With Conflict Detection**:
```
You: "Merge feature/auth into main"

Claude:
  [Bash: git merge feature/auth]
  → "Conflict detected in src/app.js"

  [Read: src/app.js]  → See conflict markers
  [Edit: src/app.js]  → Resolve conflicts
  [Bash: git add src/app.js]
  [Bash: git commit]
  → "Merge conflict resolved and committed"
```

#### Handling Merge Conflicts

When conflicts arise, Claude:

1. **Identifies** the conflicting files
2. **Reads** each file to see the conflict markers
3. **Understands** both sides of the conflict
4. **Resolves** by choosing the best combination
5. **Stages** the resolved files
6. **Commits** the merge

**Conflict Markers**:
```
<<<<<<< HEAD
const port = 3000
=======
const port = process.env.PORT || 3000
>>>>>>> feature/config
```

Claude resolves by choosing the right version (usually the more complete one):
```
const port = process.env.PORT || 3000
```

#### Deleting Branches

**After Merge**:
```
You: "Delete the feature/search branch"

Claude:
  [Bash: git branch -d feature/search]
  → "Deleted branch feature/search"
```

**Note**: Claude uses `-d` (safe delete) not `-D` (force delete) unless explicitly asked.

### Exercise

**Task**: Complete a feature branch workflow

**Setup**:
```bash
cd ~/claude-practice/git-demo
git checkout main  # make sure you're on main
claude
```

**Tasks**:

1. **Create Feature Branch**:
```
"Create a new branch called feature/greeting and switch to it"
```

2. **Make Changes**:
```
"Create a file called greeting.js that exports a greet(name)
function, then commit it"
```

3. **Switch and Create Another Branch**:
```
"Switch to main, then create another branch called
feature/farewell"
```

4. **Make Conflicting Changes**:
```
"Create a farewell.js file, then also modify app.js.
Commit the changes."
```

5. **Merge Both Features**:
```
"Switch to main, merge feature/greeting first,
then merge feature/farewell. Handle any conflicts."
```

### Verification

Before moving on, ensure you can:
- [ ] Create and switch branches
- [ ] Follow feature branch workflow
- [ ] Merge branches
- [ ] Understand how Claude handles merge conflicts
- [ ] Delete branches after merging

### Common Mistakes

1. **Working on Main Directly**:
   - Always create a feature branch
   - Keeps main clean and deployable

2. **Not Committing Before Switching**:
   - Uncommitted changes carry over or cause conflicts
   - Commit or stash before switching

3. **Force Deleting Unmerged Branches**:
   - `-d` is safe (only deletes if merged)
   - `-D` force deletes — use with caution

---

## Lesson 16: Creating Pull Requests

### Objectives
- Generate pull requests with Claude
- Write effective PR descriptions
- Use the gh CLI tool
- Understand PR best practices

### Content

#### How Claude Creates PRs

Claude uses the **GitHub CLI (`gh`)** to create pull requests. The workflow:

```
1. Check current branch and status
2. View diff against base branch
3. Review all commits in the branch
4. Push to remote (if needed)
5. Create PR with title and description
```

#### PR Creation Workflow

**Complete Flow**:
```
You: "Create a pull request for this branch"

Claude:
  [Bash: git status]
  → Clean working tree

  [Bash: git log main..HEAD --oneline]
  → 3 commits on this branch

  [Bash: git diff main...HEAD]
  → Shows all changes

  [Bash: git push -u origin feature/search]
  → Pushed to remote

  [Bash: gh pr create --title "Add search functionality"
    --body "## Summary
    - Add full-text search to /api/search endpoint
    - Index user names and email addresses
    - Add search results pagination

    ## Test plan
    - [ ] Verify search returns correct results
    - [ ] Test pagination with large datasets
    - [ ] Verify empty search handling

    🤖 Generated with Claude Code"]
  → PR #42 created
```

#### Writing Good PR Descriptions

Claude structures PR descriptions with:

**Title** (under 70 characters):
```
Add user search with pagination support
Fix authentication timeout on slow connections
Refactor database queries for better performance
```

**Body**:
```markdown
## Summary
- [1-3 bullet points describing what changed and why]

## Test plan
- [ ] Manual testing steps
- [ ] Automated test coverage
- [ ] Edge cases verified

🤖 Generated with Claude Code
```

#### Customizing PR Creation

**With Specific Reviewers**:
```
You: "Create a PR and request review from @alice and @bob"

Claude:
  [Bash: gh pr create --title "..." --body "..."
    --reviewer alice,bob]
```

**With Labels**:
```
You: "Create a PR with the 'enhancement' label"

Claude:
  [Bash: gh pr create --title "..." --body "..."
    --label enhancement]
```

**Draft PR**:
```
You: "Create a draft PR for this work-in-progress"

Claude:
  [Bash: gh pr create --draft --title "WIP: ..." --body "..."]
```

#### Viewing and Managing PRs

**View PR Details**:
```
You: "Show me PR #42"

Claude:
  [Bash: gh pr view 42]
  → Title, description, status, checks
```

**List Open PRs**:
```
You: "Show me all open pull requests"

Claude:
  [Bash: gh pr list]
  → List of open PRs with status
```

**View PR Comments**:
```
You: "Show me the review comments on PR #42"

Claude:
  [Bash: gh api repos/owner/repo/pulls/42/comments]
  → All review comments
```

### Exercise

**Task**: Create your first pull request with Claude

**Setup** (requires a GitHub repository):
```bash
# Clone a test repo or use an existing one
cd ~/claude-practice/git-demo
git remote add origin <your-repo-url>  # if needed
git checkout -b feature/practice-pr
echo "console.log('Hello PR!')" > pr-demo.js
git add pr-demo.js
git commit -m "Add PR demo file

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
claude
```

**Tasks**:

1. **Create Basic PR**:
```
"Create a pull request for this branch"
```

2. **Custom PR Description**:
```
"Create a PR with a detailed description explaining what
this branch adds and how to test it"
```

3. **View the PR**:
```
"Show me the PR we just created"
```

4. **List All PRs**:
```
"Show me all open PRs in this repository"
```

### Verification

Before moving on, ensure you can:
- [ ] Ask Claude to create pull requests
- [ ] Understand PR description structure
- [ ] View and manage existing PRs
- [ ] Customize PRs (reviewers, labels, draft)
- [ ] Know when to use `gh` CLI

### Common Mistakes

1. **Pushing to Wrong Remote**:
   - Verify remote with `git remote -v`
   - Claude checks this automatically

2. **Missing PR Description**:
   - Always include summary and test plan
   - Claude generates these by default

3. **Forgetting to Push**:
   - PR requires code on remote
   - Claude pushes before creating PR

### Pro Tips

**PR from Conversation**:
```
"We've been working on the auth fix. Create a PR
summarizing all the changes we've made."
```

**Link Issues**:
```
"Create a PR that closes issue #15"
→ Claude adds "Closes #15" to the description
```

---

## Lesson 17: Code Review with Claude

### Objectives
- Use Claude to review pull requests
- Identify security issues, bugs, and style problems
- Generate review feedback
- Understand review workflows

### Content

#### How Claude Reviews Code

Claude can review code at multiple levels:

**1. Quick Review** — Surface-level check:
```
You: "Quick review of this PR"
Claude: Checks for obvious issues, typos, missing tests
```

**2. Deep Review** — Thorough analysis:
```
You: "Do a thorough code review of PR #42"
Claude: Analyzes security, performance, architecture, tests
```

**3. Focused Review** — Specific concerns:
```
You: "Review PR #42 specifically for security issues"
Claude: Deep dive into security patterns only
```

#### Reviewing a Pull Request

**Full PR Review**:
```
You: "Review PR #42"

Claude:
  [Bash: gh pr view 42]
  → Gets PR details

  [Bash: gh pr diff 42]
  → Gets all code changes

  [Read: changed files for context]

  → "## Code Review: PR #42

  ### Summary
  This PR adds user search functionality.

  ### Issues Found

  🔴 **Critical: SQL Injection**
  File: src/search.js:15
  The search query is not parameterized.
  Fix: Use prepared statements.

  🟡 **Warning: Missing Error Handling**
  File: src/search.js:22
  No try/catch around database query.
  Fix: Wrap in try/catch, return 500 on error.

  🔵 **Suggestion: Performance**
  File: src/search.js:8
  Consider adding an index on the search column.

  ### Positive Notes
  ✅ Good test coverage
  ✅ Clean code structure
  ✅ Documentation updated"
```

#### Review Checklist

Claude checks for:

**Security**:
- SQL injection
- XSS vulnerabilities
- Authentication/authorization issues
- Hardcoded secrets
- Input validation

**Code Quality**:
- Clear naming
- Reasonable complexity
- DRY (Don't Repeat Yourself)
- Proper error handling
- Consistent style

**Performance**:
- N+1 queries
- Unnecessary computation
- Missing caching opportunities
- Large data handling

**Testing**:
- Adequate test coverage
- Edge cases covered
- Test quality and readability
- Integration tests for critical paths

**Best Practices**:
- Following project conventions
- Documentation updated
- No debugging code left in
- Proper logging

#### Reviewing Local Changes

You don't need a PR to get a review:

**Review Current Changes**:
```
You: "Review all changes I've made since the last commit"

Claude:
  [Bash: git diff]
  [Read: modified files for context]
  → Provides review feedback
```

**Review a Specific File**:
```
You: "Review src/auth.js for security issues"

Claude:
  [Read: src/auth.js]
  → Focused security review
```

**Review a Commit**:
```
You: "Review the last commit"

Claude:
  [Bash: git show HEAD]
  → Reviews the committed changes
```

#### Responding to Review Feedback

**Fix Issues from Review**:
```
You: "Fix all the issues you found in the review"

Claude:
  [Edit: src/search.js]  → Fix SQL injection
  [Edit: src/search.js]  → Add error handling
  [Bash: npm test]        → Verify fixes
  [Bash: git add ...]
  [Bash: git commit -m "Address code review feedback"]
```

**Discuss Trade-offs**:
```
You: "I disagree about the caching suggestion. The data
changes frequently. What do you think?"

Claude: "Good point. If data changes every few seconds,
caching might cause stale results. Consider a short
TTL (5-10 seconds) as a compromise, or skip caching
and optimize the query instead."
```

### Exercise

**Task**: Perform a code review with Claude

**Setup**:
```bash
cd ~/claude-practice
mkdir -p review-demo/src

cat > review-demo/src/api.js << 'EOF'
const express = require('express')
const mysql = require('mysql')
const app = express()

const db = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'password123',
  database: 'myapp'
})

app.get('/users', (req, res) => {
  const query = "SELECT * FROM users WHERE name = '" + req.query.name + "'"
  db.query(query, (err, results) => {
    res.json(results)
  })
})

app.post('/login', (req, res) => {
  const { username, password } = req.body
  const query = `SELECT * FROM users WHERE username='${username}' AND password='${password}'`
  db.query(query, (err, result) => {
    if (result.length > 0) {
      res.json({ token: 'abc123' })
    }
  })
})

app.listen(3000)
EOF

cd review-demo
claude
```

**Tasks**:

1. **General Review**:
```
"Review src/api.js for any issues"
```

2. **Security Focus**:
```
"Do a security-focused review of src/api.js"
```

3. **Fix the Issues**:
```
"Fix all the security and quality issues you found"
```

4. **Verify Fixes**:
```
"Show me the diff of all changes you made and explain
each fix"
```

5. **Final Review**:
```
"Review the fixed code one more time to make sure
nothing was missed"
```

### Verification

Before moving on, ensure you can:
- [ ] Ask Claude to review code or PRs
- [ ] Understand review severity levels
- [ ] Request focused reviews (security, performance)
- [ ] Have Claude fix review findings
- [ ] Review local changes (not just PRs)

### Common Mistakes

1. **Not Providing Context**:
   - "Review this" — review what?
   - Specify the PR number, file, or scope

2. **Ignoring Warnings**:
   - Don't dismiss suggestions without considering them
   - Discuss trade-offs with Claude if you disagree

3. **Not Re-reviewing After Fixes**:
   - Fixes can introduce new issues
   - Always do a final review pass

### Pro Tips

**Pre-commit Review**:
```
"Before I commit, review all my changes and flag
any issues"
```

**Team Standards**:
```
"Review this PR against our team's coding standards
documented in CLAUDE.md"
```

**Learn from Reviews**:
```
"Explain the SQL injection vulnerability you found
so I understand it better"
```

---

## Section 3 Summary

You now know how Claude handles the full Git workflow:

| Task | What Claude Does |
|------|-----------------|
| **Status** | Shows modified, staged, and untracked files |
| **Diffs** | Displays changes with context |
| **Commits** | Stages specific files, writes descriptive messages |
| **Branches** | Creates, switches, merges, and deletes branches |
| **Pull Requests** | Pushes code, creates PRs with descriptions |
| **Code Review** | Analyzes security, quality, performance, tests |

### Key Safety Principles

- Claude stages **specific files**, not everything
- Claude writes **new commits**, not amends (unless asked)
- Claude uses **safe deletes** (`-d` not `-D`)
- Claude **never force pushes** without explicit permission
- Claude **warns about sensitive files** (`.env`, credentials)

### What's Next?

In **Section 4: Web Tools**, you'll learn how Claude searches the web and fetches content to help you solve problems with up-to-date information.
