# Exercise 9: Advanced Hooks & Matchers

**Time**: 1 hour
**Prerequisites**: Lessons 48-50

## Objective
Build sophisticated hook configurations using specific tool matchers, regex patterns, hook chaining, scoped configurations, and a complete project hook system.

## Setup

```bash
mkdir -p ~/claude-exercises/ex9/src/api ~/claude-exercises/ex9/src/models ~/claude-exercises/ex9/src/config ~/claude-exercises/ex9/tests ~/claude-exercises/ex9/logs ~/claude-exercises/ex9/scripts
cd ~/claude-exercises/ex9
git init
npm init -y > /dev/null 2>&1
npm install --save-dev prettier eslint > /dev/null 2>&1

cat > src/api/users.js << 'EOF'
const express = require('express')
const router = express.Router()
router.get('/', (req, res) => res.json([]))
router.post('/', (req, res) => res.status(201).json(req.body))
module.exports = router
EOF

cat > src/api/products.js << 'EOF'
const express = require('express')
const router = express.Router()
router.get('/', (req, res) => res.json([]))
router.post('/', (req, res) => res.status(201).json(req.body))
module.exports = router
EOF

cat > src/models/user.js << 'EOF'
class User {
  constructor(name, email) { this.name = name; this.email = email }
  validate() { return this.name && this.email }
}
module.exports = User
EOF

cat > src/config/database.js << 'EOF'
module.exports = {
  host: 'localhost',
  port: 5432,
  database: 'myapp',
  pool: { min: 2, max: 10 }
}
EOF

cat > tests/user.test.js << 'EOF'
const User = require('../src/models/user')
test('validates user', () => {
  const u = new User('Alice', 'alice@test.com')
  expect(u.validate()).toBe(true)
})
test('rejects incomplete user', () => {
  const u = new User('', '')
  expect(u.validate()).toBe(false)
})
EOF

cat > scripts/validate-json.sh << 'EOF'
#!/bin/bash
python3 -c "import json,sys; json.load(open(sys.argv[1]))" "$1" 2>/dev/null
if [ $? -ne 0 ]; then
  echo "Invalid JSON in $1"
  exit 1
fi
echo "JSON valid: $1"
EOF
chmod +x scripts/validate-json.sh

cat > .prettierrc << 'EOF'
{ "semi": false, "singleQuote": true, "tabWidth": 2 }
EOF

mkdir -p .claude
claude
```

## Tasks

### Task 1: Create Hooks with Specific Tool Matchers (10 min)

Create hooks that target specific tools by name, not just file patterns.

Add to `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "echo \"[$(date '+%H:%M:%S')] BASH: $CLAUDE_BASH_COMMAND\" >> logs/bash-commands.log"
      },
      {
        "matcher": "Write",
        "command": "echo \"[$(date '+%H:%M:%S')] WRITE: $CLAUDE_FILE_PATH\" >> logs/file-writes.log"
      },
      {
        "matcher": "Edit",
        "command": "echo \"[$(date '+%H:%M:%S')] EDIT: $CLAUDE_FILE_PATH\" >> logs/file-edits.log"
      }
    ]
  }
}
```

Test each matcher individually:

1. Ask Claude: "Run `ls -la src/`" -- should log to `bash-commands.log`
2. Ask Claude: "Create a new file src/helpers.js with an isEmpty function" -- should log to `file-writes.log`
3. Ask Claude: "Add a trim function to src/helpers.js" -- should log to `file-edits.log`

**Verify**: Check each log file independently:
```bash
cat logs/bash-commands.log
cat logs/file-writes.log
cat logs/file-edits.log
```
Each should contain only the relevant operation type.

---

### Task 2: Use Regex Patterns in Matchers (12 min)

Build matchers that use regex to target specific file types and paths.

Update `.claude/settings.json` by adding these PostToolUse hooks:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "echo \"[$(date '+%H:%M:%S')] BASH: $CLAUDE_BASH_COMMAND\" >> logs/bash-commands.log"
      },
      {
        "matcher": "Write",
        "command": "echo \"[$(date '+%H:%M:%S')] WRITE: $CLAUDE_FILE_PATH\" >> logs/file-writes.log"
      },
      {
        "matcher": "Edit",
        "command": "echo \"[$(date '+%H:%M:%S')] EDIT: $CLAUDE_FILE_PATH\" >> logs/file-edits.log"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.test\\.js$",
        "command": "echo 'Test file modified -- consider running npm test' >> logs/test-changes.log"
      },
      {
        "matcher": "Write|Edit:src/api/.*\\.js$",
        "command": "echo \"[$(date '+%H:%M:%S')] API route changed: $CLAUDE_FILE_PATH\" >> logs/api-changes.log"
      },
      {
        "matcher": "Write|Edit:src/config/.*",
        "command": "echo \"WARNING: Config file modified: $CLAUDE_FILE_PATH\" >> logs/config-audit.log"
      },
      {
        "matcher": "Write|Edit:.*\\.(js|jsx|ts|tsx)$",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
      }
    ]
  }
}
```

Test the regex matchers:

1. Ask Claude: "Add a new test to tests/user.test.js that checks email format"
   Check: `logs/test-changes.log` should have an entry

2. Ask Claude: "Add a PATCH endpoint to src/api/users.js"
   Check: `logs/api-changes.log` should have an entry

3. Ask Claude: "Add a connection timeout setting to src/config/database.js"
   Check: `logs/config-audit.log` should have a WARNING entry

4. Ask Claude: "Create src/models/product.js with a Product class"
   Check: File should be formatted, but no entry in api-changes or config-audit logs

**Verify**: Each regex matches only its intended target:
```bash
echo "--- Test changes ---" && cat logs/test-changes.log
echo "--- API changes ---" && cat logs/api-changes.log
echo "--- Config audit ---" && cat logs/config-audit.log
```

---

### Task 3: Chain Multiple Hooks Together (10 min)

Create a sequence of hooks that work together for a validation pipeline.

Add these PreToolUse hooks that form a chain for JSON files:

```json
{
  "matcher": "Write:.*\\.json$",
  "command": "echo \"JSON write detected: $CLAUDE_FILE_PATH\" >> logs/json-pipeline.log"
},
{
  "matcher": "Write:package\\.json$",
  "command": "echo 'AUDIT: package.json modification attempted' >> logs/json-pipeline.log && echo 'Checking for prohibited packages...' >> logs/json-pipeline.log"
}
```

Also add a PostToolUse hook:

```json
{
  "matcher": "Write:.*\\.json$",
  "command": "bash scripts/validate-json.sh \"$CLAUDE_FILE_PATH\" >> logs/json-pipeline.log 2>&1"
}
```

Test the chain by asking Claude: "Update package.json to add a 'build' script that runs 'tsc'"

**Expected**:
- PreToolUse: Both JSON hooks fire (general JSON + package.json specific)
- The write proceeds
- PostToolUse: JSON validation runs on the result

**Verify**:
```bash
cat logs/json-pipeline.log
```
The log should show the full pipeline: detection, audit, and validation entries in order.

---

### Task 4: Configure Hooks at Different Scopes (12 min)

Set up hooks at user scope and project scope to understand precedence and layering.

1. **Project-level hooks** (already configured in `.claude/settings.json`):
   These apply only to this project.

2. **User-level hooks**: Ask Claude to add a global hook:
   ```
   "Read ~/.claude/settings.json and add a PostToolUse hook with matcher '.*' that appends to ~/claude-global-activity.log with the format: '[timestamp] GLOBAL: tool=$TOOL_NAME file=$CLAUDE_FILE_PATH'. Preserve any existing settings."
   ```

3. Test both scopes firing by asking Claude:
   ```
   "Add a findById static method to src/models/user.js"
   ```

4. Verify scope layering:
   ```bash
   # Project-level logs should have entries
   cat logs/file-edits.log

   # User-level log should also have an entry
   cat ~/claude-global-activity.log
   ```

5. Test that project hooks do NOT affect other directories:
   ```bash
   mkdir -p ~/claude-exercises/ex9-other
   ```
   Open Claude in `~/claude-exercises/ex9-other` and ask it to create a file. The project-level hooks from ex9 should not fire.

**Verify**: Project hooks are isolated to the project, while user hooks apply globally across all projects.

---

### Task 5: Build a Complete Hook System for a Project (16 min)

Combine everything into a production-quality hook configuration for the project.

Ask Claude to help you design and implement a comprehensive hook system:

```
"Create a complete .claude/settings.json hook configuration for this Node.js project with these requirements:

PreToolUse hooks:
1. Log all tool usage with timestamps to logs/tool-activity.log
2. Block any Write or Edit to files matching *.env* or *secret* or *credential*
3. Warn (but allow) when config files are modified

PostToolUse hooks:
4. Auto-format .js/.jsx/.ts/.tsx files with prettier after Write or Edit
5. Log API route changes (src/api/*) to logs/api-audit.log
6. Log test file changes to logs/test-audit.log
7. Run JSON validation after any .json file is written
8. Log all Bash commands with their exit context to logs/bash-audit.log

Notification hooks:
9. Append all notifications to logs/session.log with timestamps"
```

After Claude creates the configuration, verify it by running through this test sequence:

1. "Create a new API route file src/api/orders.js with GET and POST endpoints"
2. "Add a test file tests/orders.test.js with basic tests"
3. "Try to create a .env.local file with API_KEY=test123"
4. "Run npm test"
5. "Update package.json to add a 'start' script"

**Verify**:
```bash
echo "=== Tool Activity ===" && wc -l logs/tool-activity.log
echo "=== API Audit ===" && cat logs/api-audit.log
echo "=== Test Audit ===" && cat logs/test-audit.log
echo "=== Bash Audit ===" && cat logs/bash-audit.log
echo "=== Session ===" && cat logs/session.log
echo "=== Blocked? ===" && ls .env.local 2>/dev/null || echo ".env.local blocked successfully"
```

All logs should have relevant entries, the `.env.local` file should have been blocked, and all `.js` files should be properly formatted.

---

### Task 6: Review and Document the Hook System (5 min)

Ask Claude to audit the final configuration:

```
"Read .claude/settings.json and produce a summary of all active hooks:
- List each hook with its event type, matcher pattern, and purpose
- Identify any potential issues (overlapping matchers, performance concerns)
- Suggest any improvements or missing hooks"
```

**Expected**:
- Clear summary table of all hooks
- Any overlapping matchers identified
- Performance considerations noted (e.g., running prettier on every edit)
- Suggestions for improvement

**Verify**: The summary accurately describes all hooks in the configuration and provides actionable feedback.

---

## Completion Criteria

- [ ] Created hooks with tool-specific matchers (Bash, Write, Edit)
- [ ] Used regex patterns to target specific file types and paths
- [ ] Chained multiple hooks into a validation pipeline
- [ ] Configured hooks at both project and user scopes
- [ ] Built a comprehensive hook system with 8+ hooks covering PreToolUse, PostToolUse, and Notification
- [ ] Verified that blocking hooks prevent sensitive file modifications
