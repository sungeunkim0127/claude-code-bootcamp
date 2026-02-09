# Exercise 2: File Tools — Read, Write, Edit

**Time**: 45 minutes
**Prerequisites**: Lessons 6-8

## Objective
Practice reading, creating, and editing files through Claude Code.

## Setup
```bash
mkdir ~/claude-exercises/ex2
cd ~/claude-exercises/ex2

cat > server.js << 'EOF'
const http = require('http')
const PORT = 3000

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' })
  res.end('Hello World')
})

server.listen(PORT, () => {
  console.log(`Running on ${PORT}`)
})
EOF

cat > config.json << 'EOF'
{
  "port": 3000,
  "host": "localhost",
  "debug": false
}
EOF

claude
```

## Tasks

### Task 1: Read and Explain (5 min)
Ask Claude: "Read server.js and config.json and explain what this project does"
**Verify**: Claude reads both files and gives an accurate explanation

---

### Task 2: Create a New File (10 min)
Ask Claude: "Create a utils.js file with a function `formatResponse(data)` that wraps data in `{ success: true, data }` and a function `logRequest(method, url)` that logs with a timestamp"
**Verify**: utils.js exists with both functions and proper exports

---

### Task 3: Edit Existing File (10 min)
Ask Claude: "Update server.js to use the config.json port instead of hardcoded 3000, and use the formatResponse function from utils.js"
**Verify**: server.js imports from config.json and utils.js

---

### Task 4: Multi-Line Edit (10 min)
Ask Claude: "Add a /health endpoint to server.js that returns `{ status: 'ok', uptime: process.uptime() }` using formatResponse"
**Verify**: New route exists and uses formatResponse

---

### Task 5: Rename with Replace All (5 min)
Ask Claude: "Rename the variable `server` to `httpServer` everywhere in server.js"
**Verify**: All references updated, no leftover `server` variable

---

### Task 6: Review Changes (5 min)
Ask Claude: "Read all three files and explain how they work together now"
**Verify**: Claude gives an accurate summary of the updated project

## Completion Criteria
- [ ] Read files and got explanations
- [ ] Created new file with Write tool
- [ ] Edited existing file with Edit tool
- [ ] Performed multi-line edits
- [ ] Used replace_all for renaming
- [ ] Understand Read vs Write vs Edit tool selection
