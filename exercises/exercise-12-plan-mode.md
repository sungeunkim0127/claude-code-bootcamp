# Exercise 12: Mastering Plan Mode
**Time**: 45 minutes
**Prerequisites**: Lessons 71-74

## Objective
Use plan mode to break down complex features into reviewable steps before execution, and compare planning strategies across difficulty levels.

## Setup
```bash
mkdir -p ~/plan-mode-lab/src ~/plan-mode-lab/tests
cd ~/plan-mode-lab
git init
cat > src/users.js << 'EOF'
const users = [];

function addUser(name, email) {
  users.push({ name, email, createdAt: new Date() });
}

function findUser(email) {
  return users.find(u => u.email === email);
}

module.exports = { addUser, findUser, users };
EOF
cat > src/server.js << 'EOF'
const http = require("http");
const { addUser, findUser } = require("./users");

const server = http.createServer((req, res) => {
  if (req.method === "POST" && req.url === "/users") {
    let body = "";
    req.on("data", chunk => body += chunk);
    req.on("end", () => {
      const { name, email } = JSON.parse(body);
      addUser(name, email);
      res.writeHead(201);
      res.end(JSON.stringify({ status: "created" }));
    });
  } else if (req.method === "GET" && req.url.startsWith("/users/")) {
    const email = decodeURIComponent(req.url.split("/")[2]);
    const user = findUser(email);
    res.writeHead(user ? 200 : 404);
    res.end(JSON.stringify(user || { error: "not found" }));
  }
});

server.listen(3000);
module.exports = server;
EOF
npm init -y
git add -A && git commit -m "Initial user service"
```

## Tasks

### Task 1: Enter Plan Mode for a Complex Feature (10 min)
Ask Claude to plan (but not execute) adding authentication to the user service. Use the `--plan` flag or shift+tab to enter plan mode.

```bash
cd ~/plan-mode-lab
claude --plan "Add JWT-based authentication to this user service. Include signup with password hashing, login that returns a token, and middleware that protects the GET /users/:email endpoint."
```

Review the plan Claude generates. It should include:
- A list of files to create or modify.
- The order of operations (dependencies installed first, then utilities, then integration).
- No actual code changes yet -- only a structured outline.

**Verify**: Confirm that `git status` shows no changes. The plan is a proposal, not an execution.

---

### Task 2: Review and Modify the Plan (10 min)
Read through the generated plan and request modifications before approval:

1. If the plan stores passwords in plain text, ask Claude to revise the plan to include bcrypt hashing.
2. If the plan lacks input validation, ask Claude to add a validation step for email format and password length.
3. If the plan does not include tests, ask Claude to add a testing phase.

Use follow-up prompts in plan mode to iterate:
```
"Revise the plan: add input validation for email and password, and include a testing step using Node's built-in assert module."
```

**Verify**: The revised plan explicitly mentions bcrypt (or argon2), input validation rules, and a test file.

---

### Task 3: Approve the Plan and Execute (10 min)
Switch from plan mode to execution mode and approve the plan. Claude will now implement each step.

Watch the execution and note:
- Does Claude follow the planned order of operations?
- Are all planned files created?
- Does the implementation match what was outlined?

```bash
git diff --stat
```

**Verify**: Run `git diff --stat` and confirm the changed/created files match the plan. Run `node -e "require('./src/server.js')"` to verify the server starts without syntax errors (then kill the process).

---

### Task 4: Compare Results With and Without Plan Mode (10 min)
Reset the repo and try the same task without plan mode to compare:

```bash
cd ~/plan-mode-lab
git checkout -- .
git clean -fd
```

Now run the same request directly without planning:
```bash
claude --print "Add JWT-based authentication to this user service. Include signup with password hashing, login that returns a token, and middleware that protects the GET /users/:email endpoint."
```

Compare the two approaches:
- Did the direct approach miss any steps the plan caught?
- Was the file structure as well organized?
- Were there any errors that planning would have prevented?

**Verify**: Write a short comparison note in `~/plan-mode-lab/comparison.txt` documenting at least two differences you observed.

---

### Task 5: Use Extended Thinking for Architectural Planning (5 min)
Use extended thinking to plan a more complex architectural change:

```bash
cd ~/plan-mode-lab
git checkout -- .
git clean -fd
claude --plan "Refactor this application from a monolithic single-file server into a layered architecture with separate route handlers, a service layer, a data access layer, and middleware. Include error handling and request logging."
```

Observe how extended thinking affects the plan:
- Does the plan show deeper consideration of trade-offs?
- Are edge cases and error scenarios addressed?
- Is the dependency graph between layers clearly defined?

**Verify**: The plan includes at least four distinct layers (routes, services, data access, middleware) and specifies the dependency direction between them.

## Completion Criteria
- [ ] Plan mode generated a structured outline without modifying any files
- [ ] Plan was revised to include validation, hashing, and tests before approval
- [ ] Execution followed the planned steps and produced working code
- [ ] Comparison note documents concrete differences between planned and unplanned runs
- [ ] Extended thinking produced a layered architecture plan with clear layer boundaries
