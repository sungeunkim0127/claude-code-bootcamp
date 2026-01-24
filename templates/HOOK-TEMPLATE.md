# Hook Template

Hooks allow you to run custom commands in response to Claude Code events.

## Hook Types

### 1. PreToolUse
Runs before a tool is executed. Can block execution.

### 2. PostToolUse
Runs after a tool completes. Good for formatting, logging, notifications.

### 3. Permission
Customizes permission requests. Can auto-approve or auto-deny.

### 4. Notification
Responds to events like session start/end, agent completion.

---

## Configuration Location

**User-level**: `~/.claude/settings.json`
**Project-level**: `.claude/settings.json`
**Enterprise**: Managed settings

---

## Basic Hook Structure

```json
{
  "hooks": {
    "hookType": [
      {
        "match": {
          "tool": "ToolName",
          "param": "pattern"
        },
        "command": "shell command to execute",
        "description": "What this hook does"
      }
    ]
  }
}
```

---

## Example Hooks

### Auto-Format After Write/Edit

```json
{
  "hooks": {
    "postToolUse": [
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.{js,ts,jsx,tsx}"
        },
        "command": "prettier --write {{file_path}}",
        "description": "Auto-format JavaScript files after writing"
      },
      {
        "match": {
          "tool": "Edit",
          "file_path": "**/*.{js,ts,jsx,tsx}"
        },
        "command": "prettier --write {{file_path}}",
        "description": "Auto-format JavaScript files after editing"
      },
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.py"
        },
        "command": "black {{file_path}}",
        "description": "Auto-format Python files with black"
      }
    ]
  }
}
```

### Auto-Approve Safe Operations

```json
{
  "hooks": {
    "permission": [
      {
        "match": {
          "tool": "Read"
        },
        "action": "approve",
        "description": "Auto-approve all read operations"
      },
      {
        "match": {
          "tool": "Grep"
        },
        "action": "approve",
        "description": "Auto-approve all grep searches"
      },
      {
        "match": {
          "tool": "Glob"
        },
        "action": "approve",
        "description": "Auto-approve all glob searches"
      }
    ]
  }
}
```

### Block Dangerous Operations

```json
{
  "hooks": {
    "permission": [
      {
        "match": {
          "tool": "Bash",
          "command": "rm -rf*"
        },
        "action": "deny",
        "description": "Block recursive delete"
      },
      {
        "match": {
          "tool": "Bash",
          "command": "git push --force*"
        },
        "action": "deny",
        "description": "Block force push"
      },
      {
        "match": {
          "tool": "Write",
          "file_path": ".env*"
        },
        "action": "deny",
        "description": "Prevent writing to .env files"
      }
    ]
  }
}
```

### Validate Before Execution

```json
{
  "hooks": {
    "preToolUse": [
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.json"
        },
        "command": "echo '{{content}}' | jq empty",
        "description": "Validate JSON before writing",
        "stopOnError": true
      },
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.{yaml,yml}"
        },
        "command": "echo '{{content}}' | yamllint -",
        "description": "Validate YAML before writing",
        "stopOnError": true
      }
    ]
  }
}
```

### Logging & Auditing

```json
{
  "hooks": {
    "postToolUse": [
      {
        "match": {
          "tool": "*"
        },
        "command": "echo \"$(date): {{tool}} - {{file_path}}\" >> ~/.claude/audit.log",
        "description": "Log all tool usage"
      },
      {
        "match": {
          "tool": "Bash"
        },
        "command": "echo \"$(date): Bash command: {{command}}\" >> ~/.claude/bash-history.log",
        "description": "Log all bash commands"
      }
    ]
  }
}
```

### Git Integration

```json
{
  "hooks": {
    "postToolUse": [
      {
        "match": {
          "tool": "Write",
          "file_path": "src/**"
        },
        "command": "git add {{file_path}}",
        "description": "Auto-stage new files in src/"
      },
      {
        "match": {
          "tool": "Edit",
          "file_path": "src/**"
        },
        "command": "git add {{file_path}}",
        "description": "Auto-stage edited files in src/"
      }
    ]
  }
}
```

### Notifications

```json
{
  "hooks": {
    "notification": [
      {
        "match": {
          "event": "sessionEnd"
        },
        "command": "osascript -e 'display notification \"Claude session ended\" with title \"Claude Code\"'",
        "description": "Notify when session ends (macOS)"
      },
      {
        "match": {
          "event": "agentComplete"
        },
        "command": "notify-send \"Claude Agent Complete\" \"Background agent finished\"",
        "description": "Notify when agent completes (Linux)"
      }
    ]
  }
}
```

### Testing Hooks

```json
{
  "hooks": {
    "postToolUse": [
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.test.{js,ts}"
        },
        "command": "npm test -- {{file_path}}",
        "description": "Auto-run test after writing test file"
      },
      {
        "match": {
          "tool": "Edit",
          "file_path": "src/**/*.{js,ts}"
        },
        "command": "npm test -- $(echo {{file_path}} | sed 's/src/tests/; s/\\.[jt]s$/.test.js/')",
        "description": "Run corresponding test after editing source"
      }
    ]
  }
}
```

### Code Quality

```json
{
  "hooks": {
    "postToolUse": [
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.{js,ts}"
        },
        "command": "eslint {{file_path}} --fix",
        "description": "Run ESLint and auto-fix"
      },
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.py"
        },
        "command": "flake8 {{file_path}} && black {{file_path}}",
        "description": "Check and format Python"
      }
    ]
  }
}
```

---

## Advanced Patterns

### Conditional Hooks

```json
{
  "hooks": {
    "preToolUse": [
      {
        "match": {
          "tool": "Bash",
          "command": "npm install*"
        },
        "command": "if [ -f package-lock.json ]; then echo 'Lock file exists'; else echo 'No lock file' && exit 1; fi",
        "description": "Ensure package-lock.json exists before install",
        "stopOnError": true
      }
    ]
  }
}
```

### Chained Hooks

```json
{
  "hooks": {
    "postToolUse": [
      {
        "match": {
          "tool": "Write",
          "file_path": "**/*.{js,ts,jsx,tsx}"
        },
        "command": "prettier --write {{file_path}} && eslint {{file_path}} --fix && git add {{file_path}}",
        "description": "Format, lint, and stage"
      }
    ]
  }
}
```

### Environment-Specific Hooks

```json
{
  "hooks": {
    "preToolUse": [
      {
        "match": {
          "tool": "Bash",
          "command": "git push*"
        },
        "command": "if [ \"$ENVIRONMENT\" = \"production\" ]; then echo 'Cannot push to production' && exit 1; fi",
        "description": "Block pushes in production environment",
        "stopOnError": true
      }
    ]
  }
}
```

### Complex Matching

```json
{
  "hooks": {
    "permission": [
      {
        "match": {
          "tool": "Write",
          "file_path": "src/**/*.{ts,tsx}",
          "not": {
            "file_path": "**/*.test.{ts,tsx}"
          }
        },
        "action": "prompt",
        "description": "Prompt for non-test TypeScript files"
      }
    ]
  }
}
```

---

## Hook Variables

Available in command strings:

- `{{tool}}` - Tool name
- `{{file_path}}` - File path (for file operations)
- `{{command}}` - Bash command (for Bash tool)
- `{{content}}` - File content (for Write tool)
- `{{old_string}}` - Old string (for Edit tool)
- `{{new_string}}` - New string (for Edit tool)
- `{{pattern}}` - Search pattern (for Grep tool)
- `{{session_id}}` - Current session ID
- `{{timestamp}}` - Current timestamp

---

## Best Practices

### 1. Start Minimal
Begin with simple hooks, add complexity as needed.

### 2. Test Thoroughly
Test hooks on safe operations before deploying broadly.

### 3. Use Descriptions
Always include clear descriptions for debugging.

### 4. Handle Errors
Consider what happens if hook command fails.

### 5. Be Specific with Matching
Use precise patterns to avoid unintended triggers.

### 6. Log Important Actions
Log operations that modify files or system state.

### 7. Don't Block Everything
Over-restrictive hooks frustrate workflow.

### 8. Document Your Hooks
Keep a README of active hooks and their purposes.

---

## Security Considerations

### Never Auto-Approve
- Bash commands with `sudo`
- Git push to main/master
- File writes to sensitive locations (.env, credentials)
- Destructive operations (rm, delete)

### Always Validate
- JSON/YAML syntax before writing
- Test existence before operations
- Check environment before destructive actions

### Audit Regularly
- Review audit logs
- Check for unexpected tool usage
- Monitor hook performance

---

## Debugging Hooks

### Enable Verbose Mode
```bash
claude --verbose
```

### Check Hook Execution
Look for hook-related output in logs.

### Test Hook Command Manually
```bash
# Test the command outside Claude
prettier --write test-file.js
```

### Simplify to Debug
Remove complex logic, test basic functionality first.

---

## Hook Library Ideas

Create hooks for:
- [ ] Auto-formatting (prettier, black, rustfmt)
- [ ] Linting (eslint, flake8, clippy)
- [ ] Auto-testing after changes
- [ ] Git auto-staging
- [ ] Deployment blocking (prevent prod changes)
- [ ] Notification on completion
- [ ] Audit logging
- [ ] Security scanning
- [ ] Documentation generation
- [ ] Dependency checking
- [ ] License compliance
- [ ] Performance profiling
- [ ] Code metrics collection

---

## Example: Complete Hook Configuration

```json
{
  "hooks": {
    "permission": [
      {
        "match": { "tool": "Read" },
        "action": "approve",
        "description": "Auto-approve reads"
      },
      {
        "match": { "tool": "Bash", "command": "rm -rf*" },
        "action": "deny",
        "description": "Block recursive delete"
      }
    ],
    "preToolUse": [
      {
        "match": { "tool": "Write", "file_path": "**/*.json" },
        "command": "echo '{{content}}' | jq empty",
        "description": "Validate JSON",
        "stopOnError": true
      }
    ],
    "postToolUse": [
      {
        "match": { "tool": "Write", "file_path": "**/*.js" },
        "command": "prettier --write {{file_path}} && eslint {{file_path}} --fix && git add {{file_path}}",
        "description": "Format, lint, and stage"
      },
      {
        "match": { "tool": "*" },
        "command": "echo \"$(date): {{tool}}\" >> ~/.claude/audit.log",
        "description": "Audit all operations"
      }
    ],
    "notification": [
      {
        "match": { "event": "sessionEnd" },
        "command": "echo \"Session ended at $(date)\" >> ~/.claude/session.log",
        "description": "Log session end"
      }
    ]
  }
}
```

---

**Master hooks to automate your workflow and enforce standards!**
