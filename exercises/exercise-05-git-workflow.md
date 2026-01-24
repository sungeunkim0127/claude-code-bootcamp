# Exercise 5: Git Workflow with Claude

**Time**: 45 minutes
**Prerequisites**: Lessons 13-17, Git basics

## Objective
Master Git operations through Claude including commits, branches, and pull requests.

## Setup

```bash
mkdir ~/claude-exercises/ex5
cd ~/claude-exercises/ex5
git init
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit"
claude
```

## Tasks

### Task 1: Status and Diff (5 min)

Create a file and ask Claude to show the status:

```bash
echo "console.log('hello');" > app.js
```

Ask Claude: "Show me the current git status and what would be included in a commit"

**Expected**:
- Claude runs `git status`
- Shows untracked file: app.js
- Explains staging area concept

**Verify**: Output shows app.js as untracked

---

### Task 2: First Commit with Claude (10 min)

Ask Claude: "Create a commit for app.js with a good commit message"

**Expected**:
- Claude stages app.js
- Creates commit with descriptive message
- Shows commit confirmation

**Verify**: `git log --oneline` shows the new commit

---

### Task 3: Branch Creation (10 min)

Ask Claude: "Create a new branch called 'feature/add-greeting' and add a function that says hello to a user by name"

**Expected**:
- New branch created
- Function added to app.js
- Changes committed on new branch

**Verify**:
```bash
git branch  # Shows new branch
git log --oneline  # Shows commit
```

---

### Task 4: View Changes (5 min)

Ask Claude: "Show me all the differences between this branch and main"

**Expected**:
- Claude runs `git diff main`
- Explains the changes
- Shows added lines in green

**Verify**: Diff output matches your changes

---

### Task 5: Merge (5 min)

Ask Claude: "Switch to main and merge the feature branch"

**Expected**:
- Checkout main
- Merge feature/add-greeting
- Show merge result

**Verify**:
```bash
git log --oneline --graph  # Shows merge
cat app.js  # Shows merged code
```

---

### Task 6: Create a PR (10 min)

Note: Requires GitHub remote. Skip if not available.

```bash
# Setup remote first
git remote add origin https://github.com/youruser/test-repo.git
git push -u origin main
```

Create a new change and ask Claude: "Create a pull request for this change"

**Expected**:
- Branch created
- Changes committed
- PR created with description
- URL provided

**Verify**: PR visible on GitHub

---

## Completion Criteria

- [ ] Can check git status through Claude
- [ ] Created commits with Claude's help
- [ ] Created and switched branches
- [ ] Viewed diffs effectively
- [ ] Performed merge
- [ ] (Optional) Created PR

## Bonus Challenges

1. **Conflict Resolution**: Create a merge conflict and resolve it with Claude's help
2. **Interactive Rebase**: Clean up messy commit history
3. **Cherry-Pick**: Pick a specific commit from another branch

## Common Issues

**Issue**: Claude doesn't have git permission
**Solution**: Approve Bash commands for git operations

**Issue**: No remote configured
**Solution**: Set up GitHub remote before PR task

**Issue**: Merge conflicts
**Solution**: Ask Claude to help resolve them
