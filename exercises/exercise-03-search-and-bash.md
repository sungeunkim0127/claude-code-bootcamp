# Exercise 3: Search Tools & Bash — Glob, Grep, Bash

**Time**: 45 minutes
**Prerequisites**: Lessons 9-12

## Objective
Master file discovery, content searching, command execution, and multi-tool workflows.

## Setup
```bash
mkdir -p ~/claude-exercises/ex3/src/routes ~/claude-exercises/ex3/src/models ~/claude-exercises/ex3/tests
cd ~/claude-exercises/ex3

cat > src/app.js << 'EOF'
const express = require('express')
const userRoutes = require('./routes/users')
// TODO: Add authentication middleware
const app = express()
app.use('/api/users', userRoutes)
// FIXME: Add error handling middleware
app.listen(3000)
module.exports = app
EOF

cat > src/routes/users.js << 'EOF'
const router = require('express').Router()
const User = require('../models/User')
// TODO: Add input validation
router.get('/', async (req, res) => {
  const users = await User.findAll()
  res.json(users)
})
router.post('/', async (req, res) => {
  const user = await User.create(req.body)
  res.json(user)
})
module.exports = router
EOF

cat > src/models/User.js << 'EOF'
class User {
  static async findAll() { return [] }
  static async create(data) { return { id: 1, ...data } }
  static async findById(id) { return { id, name: 'Test' } }
}
module.exports = User
EOF

cat > tests/users.test.js << 'EOF'
const User = require('../src/models/User')
// TODO: Add more comprehensive tests
test('findAll returns array', async () => {
  const users = await User.findAll()
  expect(Array.isArray(users)).toBe(true)
})
EOF

cat > package.json << 'EOF'
{ "name": "ex3", "scripts": { "test": "jest", "start": "node src/app.js" } }
EOF

npm init -y > /dev/null 2>&1
claude
```

## Tasks

### Task 1: Find Files with Glob (5 min)
Ask Claude: "Find all JavaScript files in this project"
**Verify**: Claude uses Glob, finds files in src/ and tests/

Ask Claude: "Find all files in the routes directory"
**Verify**: Shows src/routes/users.js

---

### Task 2: Search Content with Grep (10 min)
Ask Claude: "Find all TODO and FIXME comments in the project"
**Verify**: Finds 3 TODO comments and 1 FIXME

Ask Claude: "Which files import or require the User model?"
**Verify**: Finds src/routes/users.js

Ask Claude: "How many times does 'async' appear in the codebase?"
**Verify**: Returns count per file

---

### Task 3: Run Commands with Bash (10 min)
Ask Claude: "Install jest as a dev dependency and run the tests"
**Verify**: jest installed, test results shown

Ask Claude: "Show me the project's package.json scripts"
**Verify**: Lists available npm scripts

---

### Task 4: Multi-Tool Workflow — Bug Fix (10 min)
Ask Claude: "The POST /api/users endpoint has no input validation. Find it, add validation for name and email fields, and create a test for it."
**Verify**: Claude uses Grep to find the route, Read to understand it, Edit to add validation, Write to create test, Bash to run tests

---

### Task 5: Multi-Tool Workflow — Audit (10 min)
Ask Claude: "Search the entire codebase for TODO/FIXME comments, read the surrounding code, then fix all the issues and remove the comments"
**Verify**: All TODOs/FIXMEs addressed, comments removed, tests still pass

## Completion Criteria
- [ ] Used Glob to find files by pattern
- [ ] Used Grep to search file contents
- [ ] Used Bash to run commands
- [ ] Completed a multi-tool workflow (search → read → edit)
- [ ] Understand when to use Glob vs Grep vs Bash
