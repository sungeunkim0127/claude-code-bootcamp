# Exercise 6: Working with Subagents

**Time**: 45 minutes
**Prerequisites**: Lessons 26-32

## Objective
Learn to delegate work to specialized subagents for codebase analysis, complex tasks, and parallel execution.

## Setup

```bash
mkdir -p ~/claude-exercises/ex6/src/auth ~/claude-exercises/ex6/src/api ~/claude-exercises/ex6/src/db ~/claude-exercises/ex6/tests
cd ~/claude-exercises/ex6

cat > src/auth/login.js << 'EOF'
const jwt = require('jsonwebtoken')
const bcrypt = require('bcrypt')
const { findUserByEmail } = require('../db/users')

async function login(email, password) {
  const user = await findUserByEmail(email)
  if (!user) throw new Error('User not found')
  const valid = await bcrypt.compare(password, user.passwordHash)
  if (!valid) throw new Error('Invalid password')
  return jwt.sign({ userId: user.id, role: user.role }, process.env.JWT_SECRET)
}

module.exports = { login }
EOF

cat > src/auth/middleware.js << 'EOF'
const jwt = require('jsonwebtoken')
// TODO: Add token refresh logic
function authMiddleware(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1]
  if (!token) return res.status(401).json({ error: 'No token' })
  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET)
    next()
  } catch (err) {
    res.status(403).json({ error: 'Invalid token' })
  }
}
module.exports = { authMiddleware }
EOF

cat > src/api/users.js << 'EOF'
const router = require('express').Router()
const { findAllUsers, createUser, findUserById } = require('../db/users')
const { authMiddleware } = require('../auth/middleware')
// FIXME: Add rate limiting
router.get('/', authMiddleware, async (req, res) => {
  const users = await findAllUsers()
  res.json(users)
})
router.get('/:id', authMiddleware, async (req, res) => {
  const user = await findUserById(req.params.id)
  if (!user) return res.status(404).json({ error: 'Not found' })
  res.json(user)
})
router.post('/', async (req, res) => {
  const user = await createUser(req.body)
  res.status(201).json(user)
})
module.exports = router
EOF

cat > src/api/posts.js << 'EOF'
const router = require('express').Router()
const { authMiddleware } = require('../auth/middleware')
// TODO: Add pagination
router.get('/', async (req, res) => { res.json([]) })
router.post('/', authMiddleware, async (req, res) => { res.status(201).json({}) })
module.exports = router
EOF

cat > src/db/users.js << 'EOF'
const users = []
async function findAllUsers() { return users }
async function createUser(data) { const u = { id: users.length + 1, ...data }; users.push(u); return u }
async function findUserById(id) { return users.find(u => u.id === Number(id)) }
async function findUserByEmail(email) { return users.find(u => u.email === email) }
module.exports = { findAllUsers, createUser, findUserById, findUserByEmail }
EOF

cat > src/app.js << 'EOF'
const express = require('express')
const userRoutes = require('./api/users')
const postRoutes = require('./api/posts')
const app = express()
app.use(express.json())
app.use('/api/users', userRoutes)
app.use('/api/posts', postRoutes)
app.listen(3000, () => console.log('Server running on port 3000'))
module.exports = app
EOF

cat > tests/auth.test.js << 'EOF'
const { login } = require('../src/auth/login')
test('login throws on missing user', async () => {
  await expect(login('nobody@test.com', 'pass')).rejects.toThrow('User not found')
})
EOF

cat > package.json << 'EOF'
{ "name": "ex6", "scripts": { "test": "jest", "start": "node src/app.js" } }
EOF

claude
```

## Tasks

### Task 1: Launch an Explore Agent (8 min)

Ask Claude to use an Explore subagent to analyze the project architecture:

```
"Use the Explore agent with 'very thorough' to analyze this project. Identify:
- The entry point and request flow
- How authentication is implemented
- The database layer design
- Any security concerns"
```

**Expected**:
- Explore agent launched with read-only tools (Glob, Grep, Read)
- Comprehensive analysis of the architecture returned
- Security issues identified (e.g., missing rate limiting, no token refresh)

**Verify**: The analysis covers all four bullet points and identifies at least two TODO/FIXME items

---

### Task 2: General-Purpose Subagent for Complex Work (10 min)

Delegate a multi-step task to a general-purpose subagent:

```
"Launch a subagent to:
1. Find all TODO and FIXME comments in the codebase
2. Fix each issue appropriately (add rate limiting, pagination, token refresh)
3. Add relevant comments explaining what was added
4. Report a summary of all changes made"
```

**Expected**:
- Subagent receives clear multi-step instructions
- Works independently across multiple files
- Reports back with a summary of changes

**Verify**: Run `grep -r "TODO\|FIXME" src/` -- all original TODO/FIXME comments should be resolved

---

### Task 3: Run Parallel Subagents (8 min)

Ask Claude to analyze different parts of the codebase simultaneously:

```
"In parallel, launch three Explore agents:
1. Analyze the authentication module in src/auth/
2. Analyze the API routes in src/api/
3. Analyze the database layer in src/db/
Then synthesize the findings into a single architecture summary."
```

**Expected**:
- Three subagents launched simultaneously
- Each focuses on its assigned module
- Results combined into a unified summary

**Verify**: The output includes separate analysis for each module and a combined summary

---

### Task 4: Background Task with Progress Checking (8 min)

Launch a longer-running background task:

```
"Start a background subagent to refactor the database layer:
- Add input validation to all db functions
- Add JSDoc comments to every exported function
- Create a new db/index.js that re-exports everything
Let me know when it's done."
```

While it works, ask Claude: "What is the background agent working on right now?"

**Expected**:
- Background task running independently
- Progress updates available
- Work completed without blocking the main conversation

**Verify**: Check that `src/db/users.js` has JSDoc comments and validation, and `src/db/index.js` exists

---

### Task 5: Task Management and Breakdown (6 min)

Use Claude's task management to plan and execute a feature:

```
"Break down the following feature into subtasks, then execute each one:
Add a user roles system with:
- A roles definition file
- Middleware that checks roles
- Protected admin-only routes
- Tests for role checking"
```

**Expected**:
- Claude creates a structured plan with subtasks
- Each subtask is executed in order
- Dependencies between tasks are respected

**Verify**: The roles system works end-to-end: role definitions exist, middleware checks roles, routes are protected

---

### Task 6: Choosing the Right Subagent (5 min)

Test your understanding by asking Claude to handle these requests and observe which subagent type it selects:

1. "How does the authentication flow work in this project?" (should use Explore)
2. "Add error handling to all API routes and write tests for them" (should use general-purpose)
3. "Run npm test and show me the results" (should use Bash or direct tool)

**Expected**:
- Different subagent types selected for different tasks
- Claude explains why each type was chosen

**Verify**: Each request uses an appropriate subagent type matching the task requirements

---

## Completion Criteria

- [ ] Successfully launched an Explore agent for codebase analysis
- [ ] Delegated a complex multi-step task to a general-purpose subagent
- [ ] Ran parallel subagents and received combined results
- [ ] Used a background task and checked its progress
- [ ] Broke down a feature into subtasks using task management
- [ ] Understand when to use Explore vs general-purpose vs Bash subagents
