# Exercise 1: First Steps with Claude Code

**Time**: 30 minutes
**Prerequisites**: Claude Code installed

## Setup
```bash
mkdir ~/claude-exercises/ex1
cd ~/claude-exercises/ex1
echo "console.log('Hello')" > app.js
claude
```

## Tasks

### Task 1: Basic Reading
Ask Claude: "Read app.js and explain what it does"
**Verify**: Claude reads and explains the file

### Task 2: File Creation
Ask Claude: "Create utils.js with a greet(name) function"
**Verify**: utils.js exists with correct function

### Task 3: File Editing
Ask Claude: "Add error handling to greet() function"
**Verify**: Function has try-catch or validation

### Task 4: Multiple Operations
Ask Claude: "Read both files and explain how they could work together"
**Verify**: Claude reads both and provides integration suggestions

### Task 5: Git Basics
Ask Claude: "Initialize git and create first commit"
**Verify**: `.git` exists, commit created

## Completion Criteria
- [ ] All 5 tasks completed
- [ ] All verifications passed
- [ ] Comfortable with basic Claude interaction
