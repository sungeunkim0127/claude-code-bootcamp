# Exercise 8: Hooks System Basics

**Time**: 1 hour
**Prerequisites**: Lessons 43-47

## Objective
Create and configure hooks that automatically respond to Claude's actions, including logging, formatting, and notification hooks.

## Setup

```bash
mkdir -p ~/claude-exercises/ex8/src ~/claude-exercises/ex8/tests ~/claude-exercises/ex8/logs
cd ~/claude-exercises/ex8
git init
npm init -y > /dev/null 2>&1
npm install --save-dev prettier > /dev/null 2>&1

cat > src/app.js << 'EOF'
const http = require('http')
function handleRequest(req, res) {
    const data = {message:"hello world",status:"ok"}
    res.writeHead(200,{'Content-Type':'application/json'})
    res.end(JSON.stringify(data))
}
const server = http.createServer(handleRequest)
server.listen(3000)
EOF

cat > src/utils.js << 'EOF'
function add(a,b){return a+b}
function multiply(a,b){return a*b}
function divide(a,b){if(b===0)throw new Error('Division by zero');return a/b}
module.exports={add,multiply,divide}
EOF

cat > tests/utils.test.js << 'EOF'
const {add,multiply,divide} = require('../src/utils')
test('add',()=>{expect(add(2,3)).toBe(5)})
test('multiply',()=>{expect(multiply(3,4)).toBe(12)})
test('divide by zero',()=>{expect(()=>divide(1,0)).toThrow()})
EOF

cat > .prettierrc << 'EOF'
{ "semi": false, "singleQuote": true, "tabWidth": 2, "trailingComma": "es5" }
EOF

mkdir -p .claude
claude
```

## Tasks

### Task 1: Create a PreToolUse Hook That Logs File Writes (10 min)

Create a hook that logs every time Claude writes or edits a file.

Add this configuration to `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "echo \"[$(date '+%Y-%m-%d %H:%M:%S')] PRE: $TOOL_NAME on $CLAUDE_FILE_PATH\" >> logs/tool-activity.log"
      }
    ]
  }
}
```

Test by asking Claude: "Add a subtract function to src/utils.js and export it"

**Expected**:
- The hook runs before the edit
- A log entry is written to `logs/tool-activity.log`
- The edit proceeds normally after logging

**Verify**: Check the log file:
```bash
cat logs/tool-activity.log
```
It should contain a timestamped entry showing the Write or Edit operation on `src/utils.js`.

---

### Task 2: Create a PostToolUse Hook That Auto-Formats Code (12 min)

Add a PostToolUse hook that runs Prettier after any JavaScript file is written or edited.

Update `.claude/settings.json` to add the PostToolUse section:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "echo \"[$(date '+%Y-%m-%d %H:%M:%S')] PRE: $TOOL_NAME on $CLAUDE_FILE_PATH\" >> logs/tool-activity.log"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.js$",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
      }
    ]
  }
}
```

Test by asking Claude: "Rewrite src/app.js to add a /health endpoint. Don't worry about formatting."

**Expected**:
- Claude writes the file (possibly with inconsistent formatting)
- Prettier runs automatically after the write
- The resulting file is cleanly formatted

**Verify**: Open `src/app.js` and confirm proper formatting (consistent quotes, semicolons match `.prettierrc` settings, proper indentation).

---

### Task 3: Create a Notification Hook (10 min)

Add a Notification hook that writes session events to a log file.

Update `.claude/settings.json` to add the Notification section:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "echo \"[$(date '+%Y-%m-%d %H:%M:%S')] PRE: $TOOL_NAME on $CLAUDE_FILE_PATH\" >> logs/tool-activity.log"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.js$",
        "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
      }
    ],
    "Notification": [
      {
        "matcher": ".*",
        "command": "echo \"[$(date '+%Y-%m-%d %H:%M:%S')] NOTIFY: $NOTIFICATION_MESSAGE\" >> logs/notifications.log"
      }
    ]
  }
}
```

Test by running a longer task that generates notifications:

```
"Create a new file src/calculator.js with add, subtract, multiply, and divide functions, then create comprehensive tests in tests/calculator.test.js, then run the tests"
```

**Expected**:
- Notification hook fires on relevant session events
- Messages are logged to `logs/notifications.log`

**Verify**:
```bash
cat logs/notifications.log
```
The log should contain notification entries from the session.

---

### Task 4: Configure Hooks in Settings Files (12 min)

Practice configuring hooks at different scopes and understanding precedence.

1. First, check the current project-level configuration:
   ```bash
   cat .claude/settings.json
   ```

2. Create a user-level hook that applies globally. Ask Claude:
   ```
   "Read my ~/.claude/settings.json file. Add a PostToolUse hook that logs all Bash commands to ~/claude-bash-audit.log with timestamps. Preserve any existing settings."
   ```

3. Verify the scope separation by asking Claude:
   ```
   "What hooks are currently active? Check both project-level (.claude/settings.json) and user-level (~/.claude/settings.json) configurations"
   ```

4. Test that both scopes work simultaneously:
   ```
   "Edit src/utils.js to add a modulo function, then run the tests with npm test"
   ```

**Expected**:
- Project-level hooks fire (logging, formatting)
- User-level hooks also fire (bash audit)
- Both logs are written to their respective locations

**Verify**: Check both log files:
```bash
cat logs/tool-activity.log
cat ~/claude-bash-audit.log
```
Both should have new entries from the last operation.

---

### Task 5: Test That Hooks Work Correctly (10 min)

Perform a systematic test of all configured hooks:

1. **Test PreToolUse logging**:
   Ask Claude: "Create a new file src/helpers.js with a debounce function"
   Check: `tail -1 logs/tool-activity.log` should show the Write event

2. **Test PostToolUse formatting**:
   Ask Claude: "Add this exact code to src/helpers.js: `function throttle(fn,delay){let timer;return function(...args){if(!timer){timer=setTimeout(()=>{timer=null},delay);fn.apply(this,args)}}}`"
   Check: `cat src/helpers.js` should show properly formatted code (not the one-liner)

3. **Test hook error handling**:
   Ask Claude: "Write a Python file at src/script.py with a hello world function"
   Check: The `.py` file should NOT trigger the Prettier hook (matcher is `.*\.js$`)

4. **Test multiple hooks firing**:
   Ask Claude: "Edit src/utils.js to add an 'abs' function"
   Check: Both `logs/tool-activity.log` (PreToolUse) and formatted output (PostToolUse) should be present

**Verify**: All four tests produce expected results. The log has multiple entries and JavaScript files are formatted.

---

### Task 6: Create a Blocking PreToolUse Hook (6 min)

Create a hook that blocks writes to protected files.

Add to the PreToolUse section in `.claude/settings.json`:

```json
{
  "matcher": "Write|Edit:.*\\.env.*",
  "command": "echo 'BLOCKED: Cannot modify .env files via Claude' && exit 1"
}
```

Test by asking Claude: "Create a .env file with DATABASE_URL=postgres://localhost/mydb"

**Expected**:
- The PreToolUse hook fires and detects the `.env` file pattern
- The hook exits with code 1, blocking the write
- Claude receives the block message

**Verify**: Confirm no `.env` file was created:
```bash
ls -la .env 2>/dev/null || echo "No .env file - hook blocked successfully"
```

---

## Completion Criteria

- [ ] Created a PreToolUse hook that logs file operations
- [ ] Created a PostToolUse hook that auto-formats JavaScript with Prettier
- [ ] Created a Notification hook that logs session events
- [ ] Configured hooks at both project and user scopes
- [ ] Verified all hooks fire correctly with systematic tests
- [ ] Created a blocking hook that prevents writes to sensitive files
