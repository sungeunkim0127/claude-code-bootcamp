# Section 8: Hooks System (Lessons 43-50)

## Overview
Automate workflows with hooks that respond to Claude's actions. Create formatting, validation, and enforcement systems.

---

## Lesson 43: Introduction to Hooks

### Objectives
- Understand what hooks are and their purpose
- Learn hook lifecycle events
- Know when to use hooks vs skills
- Understand hook security implications

### What Are Hooks?

Hooks are automated commands that run in response to Claude's actions. They:
- Execute shell commands
- Run before or after tool use
- Filter or modify operations
- Enforce standards automatically

### Hook vs Skills

| Aspect | Hooks | Skills |
|--------|-------|--------|
| Trigger | Automatic on events | Manual invocation |
| Purpose | Automation/enforcement | Reusable expertise |
| Visibility | Background | Foreground |
| Example | Auto-format on save | Code review workflow |

### Hook Lifecycle Events

```
PreToolUse    → Before tool executes
PostToolUse   → After tool completes
Notification  → On notifications
Session       → On session events
```

### Basic Hook Structure

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.js$",
        "command": "npx prettier --write \"$FILE_PATH\""
      }
    ]
  }
}
```

### Exercise 43: Understand Hook Examples

**Task**: Analyze existing hook configurations

1. Review the example hooks in `resources/example-hooks/`
2. Identify which event each hook responds to
3. Understand what each hook accomplishes

**Verification**:
- [ ] Can identify hook events
- [ ] Understand matcher patterns
- [ ] Know what commands execute

---

## Lesson 44: PreToolUse Hooks

### Objectives
- Intercept operations before execution
- Validate parameters and inputs
- Modify tool inputs safely
- Block dangerous operations

### PreToolUse Flow

```
Claude decides to use tool
        ↓
  PreToolUse hook runs
        ↓
    ┌───┴───┐
    │ Pass? │
    └───┬───┘
    Yes │ No
        ↓   ↓
   Tool runs  Blocked
```

### Validation Example

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash:rm.*",
        "command": "echo 'Blocked: rm commands require explicit approval' && exit 1"
      }
    ]
  }
}
```

### Parameter Validation

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write:.*\\.json$",
        "command": "echo '$CONTENT' | python -m json.tool > /dev/null || (echo 'Invalid JSON' && exit 1)"
      }
    ]
  }
}
```

### Common PreToolUse Patterns

1. **Block dangerous commands**
2. **Validate file formats**
3. **Check file paths**
4. **Verify permissions**

### Exercise 44: Create Validation Hook

**Task**: Build a PreToolUse hook

1. Create a hook that validates JSON before Write
2. Test with valid JSON (should pass)
3. Test with invalid JSON (should block)

**Configuration:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write:.*\\.json$",
        "command": "echo '$CONTENT' | python -c 'import json,sys; json.load(sys.stdin)'"
      }
    ]
  }
}
```

**Verification**:
- [ ] Valid JSON writes succeed
- [ ] Invalid JSON writes blocked
- [ ] Error message displayed

---

## Lesson 45: PostToolUse Hooks

### Objectives
- Run commands after tool execution
- Format code automatically
- Log tool usage
- Trigger notifications

### PostToolUse Flow

```
Tool executes successfully
        ↓
  PostToolUse hook runs
        ↓
  Additional action taken
```

### Auto-Format Example

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.(js|jsx|ts|tsx)$",
        "command": "npx prettier --write \"$FILE_PATH\""
      },
      {
        "matcher": "Write|Edit:.*\\.py$",
        "command": "black \"$FILE_PATH\""
      },
      {
        "matcher": "Write|Edit:.*\\.go$",
        "command": "gofmt -w \"$FILE_PATH\""
      }
    ]
  }
}
```

### Logging Example

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": ".*",
        "command": "echo \"$(date): $TOOL_NAME on $FILE_PATH\" >> ~/.claude/tool.log"
      }
    ]
  }
}
```

### Linting After Edit

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit:.*\\.(ts|tsx)$",
        "command": "npx eslint --fix \"$FILE_PATH\""
      }
    ]
  }
}
```

### Exercise 45: Create Auto-Format Hook

**Task**: Set up automatic formatting

1. Create a PostToolUse hook for JavaScript formatting
2. Edit a JavaScript file with inconsistent formatting
3. Verify formatting is applied automatically

**Verification**:
- [ ] Hook configured correctly
- [ ] Formatting runs after Write/Edit
- [ ] File is properly formatted

---

## Lesson 46: Permission Hooks

### Objectives
- Customize permission request behavior
- Auto-approve safe operations
- Log permission decisions
- Implement custom approval logic

### Permission Hook Events

```json
{
  "hooks": {
    "Permission": [
      {
        "matcher": "Read:.*",
        "action": "allow"
      },
      {
        "matcher": "Bash:rm.*-rf.*",
        "action": "deny"
      }
    ]
  }
}
```

### Auto-Approval Patterns

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test*)",
      "Bash(npm run lint*)"
    ]
  }
}
```

### Logging Permissions

```json
{
  "hooks": {
    "Permission": [
      {
        "matcher": ".*",
        "command": "echo \"Permission: $TOOL_NAME $ACTION\" >> ~/.claude/permissions.log"
      }
    ]
  }
}
```

### Exercise 46: Configure Auto-Approval

**Task**: Set up smart auto-approval

1. Auto-approve all Read operations
2. Auto-approve npm test and lint
3. Log all permission decisions
4. Test the configuration

**Verification**:
- [ ] Read operations auto-approved
- [ ] Specified commands auto-approved
- [ ] Other operations prompt normally

---

## Lesson 47: Notification & Event Hooks

### Objectives
- Handle system notifications
- Track session events (start/end)
- Monitor agent completion
- Send external notifications

### Notification Hook

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": ".*error.*",
        "command": "osascript -e 'display notification \"$MESSAGE\" with title \"Claude Error\"'"
      }
    ]
  }
}
```

### Session Hooks

```json
{
  "hooks": {
    "SessionStart": [
      {
        "command": "echo \"Session started: $(date)\" >> ~/.claude/sessions.log"
      }
    ],
    "SessionEnd": [
      {
        "command": "echo \"Session ended: $(date)\" >> ~/.claude/sessions.log"
      }
    ]
  }
}
```

### Agent Completion Hook

```json
{
  "hooks": {
    "AgentComplete": [
      {
        "command": "terminal-notifier -message 'Agent task completed' -title 'Claude'"
      }
    ]
  }
}
```

### External Notifications

Send to Slack:
```json
{
  "hooks": {
    "AgentComplete": [
      {
        "command": "curl -X POST -H 'Content-type: application/json' --data '{\"text\":\"Claude completed: $TASK\"}' $SLACK_WEBHOOK"
      }
    ]
  }
}
```

### Exercise 47: Session Logging

**Task**: Track session activity

1. Create hooks for session start/end
2. Log to a file with timestamps
3. Start and end a session
4. Review the log

**Verification**:
- [ ] Session start logged
- [ ] Session end logged
- [ ] Timestamps accurate

---

## Lesson 48: Hook Matchers

### Objectives
- Match specific tools precisely
- Use regex patterns effectively
- Match by file paths
- Combine multiple conditions

### Matcher Syntax

```
"matcher": "ToolName:PathPattern"
```

### Tool Matching

```json
"matcher": "Write"           // Any Write
"matcher": "Write|Edit"      // Write or Edit
"matcher": "Bash"            // Any Bash command
```

### Path Matching (Regex)

```json
"matcher": ".*\\.js$"        // Any .js file
"matcher": ".*\\.test\\.ts$" // Test files
"matcher": "src/.*"          // Files in src/
"matcher": ".*\\.(ts|tsx)$"  // TypeScript files
```

### Combined Matching

```json
"matcher": "Write|Edit:src/.*\\.(ts|tsx)$"  // Write/Edit TS in src/
"matcher": "Bash:npm (test|run)"            // npm test or run commands
```

### Negative Matching

```json
{
  "matcher": "Write:^(?!.*\\.test\\.).*$",
  "command": "..."
}
```

### Exercise 48: Precise Hook Targeting

**Task**: Create targeted hooks

1. Hook that only runs on TypeScript files in `src/components/`
2. Hook that runs on all test files
3. Hook that runs on package.json only

**Configuration:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:src/components/.*\\.tsx$",
        "command": "echo 'Component modified'"
      },
      {
        "matcher": "Write|Edit:.*\\.test\\.(ts|tsx)$",
        "command": "echo 'Test file modified'"
      },
      {
        "matcher": "Write|Edit:package\\.json$",
        "command": "npm install"
      }
    ]
  }
}
```

**Verification**:
- [ ] Hooks trigger on correct files
- [ ] No false positives
- [ ] Patterns work as expected

---

## Lesson 49: Hook Configuration

### Objectives
- Configure hooks in settings.json
- Understand hook scopes (user, project, enterprise)
- Debug hook execution
- Handle hook failures gracefully

### Configuration Location

**User level** (`~/.claude/settings.json`):
```json
{
  "hooks": {
    "PostToolUse": [...]
  }
}
```

**Project level** (`.claude/settings.json`):
```json
{
  "hooks": {
    "PreToolUse": [...]
  }
}
```

### Hook Precedence

```
1. Enterprise hooks (highest)
2. Project hooks
3. User hooks (lowest)
```

All matching hooks run in order.

### Debugging Hooks

Enable verbose mode:
```bash
claude --verbose
```

Check hook output:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": ".*",
        "command": "echo 'Hook triggered: $TOOL_NAME' >&2"
      }
    ]
  }
}
```

### Handling Failures

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write:.*\\.ts$",
        "command": "npx prettier --write \"$FILE_PATH\" || true",
        "continueOnError": true
      }
    ]
  }
}
```

### Exercise 49: Debug Hook Setup

**Task**: Set up hooks with proper error handling

1. Create a hook that might fail (e.g., linter on syntax error)
2. Add error handling to prevent blocking
3. Add logging to track hook execution
4. Test with both success and failure cases

**Verification**:
- [ ] Hook runs on success
- [ ] Failure doesn't block Claude
- [ ] Execution is logged

---

## Lesson 50: Advanced Hook Patterns

### Objectives
- Chain multiple hooks effectively
- Create reusable hook libraries
- Share hooks with team
- Optimize hook performance

### Hook Chains

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.ts$",
        "command": "npx prettier --write \"$FILE_PATH\"",
        "order": 1
      },
      {
        "matcher": "Write|Edit:.*\\.ts$",
        "command": "npx eslint --fix \"$FILE_PATH\"",
        "order": 2
      },
      {
        "matcher": "Write|Edit:.*\\.ts$",
        "command": "npx tsc --noEmit \"$FILE_PATH\"",
        "order": 3
      }
    ]
  }
}
```

### Reusable Hook Scripts

Create `scripts/format.sh`:
```bash
#!/bin/bash
FILE_PATH=$1
EXT="${FILE_PATH##*.}"

case $EXT in
  js|jsx|ts|tsx) npx prettier --write "$FILE_PATH" ;;
  py) black "$FILE_PATH" ;;
  go) gofmt -w "$FILE_PATH" ;;
  rs) rustfmt "$FILE_PATH" ;;
esac
```

Reference in hooks:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*",
        "command": "./scripts/format.sh \"$FILE_PATH\""
      }
    ]
  }
}
```

### Team Hook Sharing

1. Create `.claude/hooks/` directory
2. Add hook scripts
3. Reference in settings.json
4. Commit to git

### Performance Optimization

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:src/.*\\.ts$",
        "command": "npx prettier --write \"$FILE_PATH\"",
        "async": true  // Don't block Claude
      }
    ]
  }
}
```

### Exercise 50: Complete Hook System

**Task**: Build a production hook system

1. Create formatting hooks for 3 languages
2. Add pre-commit validation hook
3. Add logging for all hooks
4. Implement error handling
5. Test the complete system

**Verification**:
- [ ] Multi-language formatting works
- [ ] Validation catches errors
- [ ] Logging captures events
- [ ] System handles errors gracefully

---

## Section 8 Summary

### Key Takeaways

1. **PreToolUse** hooks validate/block before execution
2. **PostToolUse** hooks automate post-processing
3. **Matchers** target specific tools and files
4. **Hook chains** create comprehensive workflows
5. **Error handling** prevents workflow disruption

### Hook Types
- PreToolUse - Validation & blocking
- PostToolUse - Automation & formatting
- Permission - Auto-approval & logging
- Notification - Alerts & events
- Session - Lifecycle tracking

### Next Steps
Proceed to Advanced Level (Lessons 51-80) to learn MCP, custom subagents, and IDE integration.

---

## Intermediate Level Complete!

You've completed Lessons 26-50. You can now:
- Use subagents for complex tasks
- Create custom skills
- Configure automation hooks
- Manage parallel work
- Build team workflows
