# Section 12: IDE Integration (Lessons 75-80)

## Overview
Integrate Claude Code seamlessly into your development environment for maximum productivity.

---

## Lesson 75: VS Code Extension Setup

### Objectives
- Install and configure VS Code extension
- Learn default keyboard shortcuts
- Customize extension settings
- Optimize panel layout for workflow

### Installation

1. Open VS Code
2. Extensions panel (Ctrl+Shift+X)
3. Search "Claude Code"
4. Install official Anthropic extension

### Initial Configuration

Open settings (Ctrl+,) and search for "Claude":

```json
{
  "claude.apiKey": "stored-securely",
  "claude.defaultModel": "sonnet",
  "claude.showInlineHints": true,
  "claude.autoSuggest": false
}
```

### Default Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Shift+C` | Open Claude panel |
| `Ctrl+L` | Add selection to Claude |
| `Ctrl+K Ctrl+C` | New Claude conversation |
| `Ctrl+Enter` | Send message |
| `Escape` | Cancel/close |

### Panel Layout

Recommended setup:
```
┌─────────────────────────────────────────────────────┐
│                    Editor                           │
│  ┌─────────────────────────┬──────────────────────┐ │
│  │                         │                      │ │
│  │      Code Editor        │   Claude Panel       │ │
│  │                         │   (25% width)        │ │
│  │                         │                      │ │
│  └─────────────────────────┴──────────────────────┘ │
│  ┌─────────────────────────────────────────────────┐ │
│  │              Terminal (optional)                 │ │
│  └─────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

### Exercise 75: Complete VS Code Setup

**Task**: Configure optimal VS Code environment

1. Install Claude Code extension
2. Configure API key and settings
3. Set up keyboard shortcuts
4. Arrange panels optimally
5. Test basic interaction

**Verification**:
- [ ] Extension installed
- [ ] Settings configured
- [ ] Shortcuts working
- [ ] Panel layout comfortable

---

## Lesson 76: VS Code Workflows

### Objectives
- Use inline diffs for reviewing changes
- Reference files with @-mentions
- Use line range references precisely
- Review and accept changes efficiently

### Inline Diff Review

When Claude makes changes:
```
┌────────────────────────────────────┐
│ - const oldCode = "previous";      │  ← Red: removed
│ + const newCode = "updated";       │  ← Green: added
│                                    │
│ [Accept] [Reject] [Edit]          │
└────────────────────────────────────┘
```

### @-Mentions in VS Code

**File references:**
```
"Review @src/components/Header.tsx"
```

**Multiple files:**
```
"Compare @utils/old.js with @utils/new.js"
```

**With line numbers:**
```
"Explain @src/api.ts:45-60"
```

### Line Range References

Select code in editor, then:
```
Ctrl+L → "Explain this selected code"
```

Or manually:
```
"Fix the bug at @src/auth.ts:123-145"
```

### Change Review Workflow

1. Request change in Claude panel
2. Review inline diff
3. Accept, reject, or modify
4. Continue with next task

### Exercise 76: IDE-Integrated Development

**Task**: Complete a feature entirely in VS Code

1. Open a project in VS Code
2. Use @-mention to reference a file
3. Request a small change
4. Review inline diff
5. Accept the change
6. Verify in editor

**Verification**:
- [ ] @-mention worked
- [ ] Diff appeared inline
- [ ] Change applied correctly
- [ ] Workflow felt natural

---

## Lesson 77: Multiple Conversations

### Objectives
- Manage multiple conversation tabs
- Switch between conversations efficiently
- Organize conversations by task
- Archive completed conversations

### Opening Multiple Conversations

```
Ctrl+K Ctrl+C → New conversation tab
```

Or click "+" in Claude panel.

### Naming Conversations

Right-click tab → Rename:
```
"Feature: User Auth"
"Bug: Login Error"
"Research: API Design"
```

### Switching Contexts

Each conversation maintains its own:
- Chat history
- Context files
- Session state

Switch tabs to change context instantly.

### Conversation Organization

**By task type:**
```
Tab 1: Current feature development
Tab 2: Bug fixing
Tab 3: Code review
Tab 4: Learning/research
```

**By project area:**
```
Tab 1: Frontend work
Tab 2: Backend work
Tab 3: Database queries
```

### Archiving

Right-click → Archive when done:
- Saves to history
- Frees up tab space
- Searchable later

### Exercise 77: Multi-Conversation Workflow

**Task**: Work with parallel conversations

1. Open 3 conversation tabs
2. Name them appropriately
3. Use different context in each
4. Switch between them
5. Archive one when complete

**Verification**:
- [ ] Multiple tabs open
- [ ] Each has distinct context
- [ ] Switching is smooth
- [ ] Archiving works

---

## Lesson 78: VS Code Command Palette

### Objectives
- Access Claude from command palette
- Learn essential keyboard shortcuts
- Integrate with VS Code tasks
- Create custom keybindings

### Command Palette Access

Press `Ctrl+Shift+P`, then type:
```
Claude: Send to Claude
Claude: New Conversation
Claude: Toggle Panel
Claude: Explain Selection
```

### Essential Commands

| Command | Description |
|---------|-------------|
| `Claude: Explain` | Explain selected code |
| `Claude: Fix` | Fix issues in selection |
| `Claude: Optimize` | Suggest optimizations |
| `Claude: Document` | Generate documentation |
| `Claude: Test` | Generate tests |

### Custom Keybindings

Open keybindings.json:
```json
[
  {
    "key": "ctrl+alt+e",
    "command": "claude.explainSelection"
  },
  {
    "key": "ctrl+alt+f",
    "command": "claude.fixSelection"
  },
  {
    "key": "ctrl+alt+t",
    "command": "claude.generateTests"
  }
]
```

### VS Code Task Integration

tasks.json:
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Claude Review",
      "type": "shell",
      "command": "claude",
      "args": ["--print", "Review the current file for issues"],
      "problemMatcher": []
    }
  ]
}
```

### Exercise 78: Keyboard-Driven Workflow

**Task**: Work primarily with keyboard

1. Set up 3 custom keybindings
2. Use only keyboard for 10 minutes
3. Access Claude via command palette
4. Review code without using mouse

**Verification**:
- [ ] Custom keybindings work
- [ ] Command palette efficient
- [ ] Keyboard-first workflow possible

---

## Lesson 79: Remote Development

### Objectives
- Use Claude with Remote SSH
- Work effectively in containers
- Access remote codebases seamlessly
- Handle network latency gracefully

### Remote SSH Setup

1. Install Remote SSH extension
2. Connect to remote host
3. Claude extension works on remote
4. Files read from remote system

### Container Development

With Dev Containers:
```json
// devcontainer.json
{
  "customizations": {
    "vscode": {
      "extensions": [
        "anthropic.claude-code"
      ]
    }
  }
}
```

### Remote Codebase Access

Claude accesses files where VS Code is connected:
- Local: Local filesystem
- SSH: Remote filesystem
- Container: Container filesystem

### Handling Latency

**Strategies:**
- Use /compact more frequently
- Batch requests
- Request specific files
- Avoid large glob operations

**Settings for slow connections:**
```json
{
  "claude.timeout": 60000,
  "claude.retryOnTimeout": true
}
```

### Exercise 79: Remote Workflow

**Task**: Work with remote environment

1. Connect to remote (SSH or container)
2. Open Claude panel
3. Analyze remote code
4. Make changes remotely
5. Verify changes applied

**Verification**:
- [ ] Remote connection established
- [ ] Claude reads remote files
- [ ] Changes apply correctly
- [ ] Latency managed

---

## Lesson 80: JetBrains Integration

### Objectives
- Set up Claude in JetBrains IDEs
- Compare with VS Code experience
- Use IDE-specific features
- Optimize for your IDE preference

### Installation

1. Open JetBrains IDE (IntelliJ, PyCharm, WebStorm, etc.)
2. Settings → Plugins → Marketplace
3. Search "Claude"
4. Install and restart

### Configuration

Settings → Tools → Claude:
```
API Key: [your-key]
Default Model: sonnet
Panel Location: Right
```

### JetBrains-Specific Features

**Inline completions:**
- Tab to accept
- Escape to dismiss
- Arrow keys to navigate

**Intention actions:**
- Alt+Enter on code
- "Ask Claude about this"

**Tool windows:**
- Claude panel in tool window
- Dockable and movable

### VS Code vs JetBrains

| Feature | VS Code | JetBrains |
|---------|---------|-----------|
| Extension | First-party | Community |
| Inline diffs | Native | Plugin |
| Shortcuts | Customizable | IDE conventions |
| Performance | Lighter | More features |

### Choosing Your IDE

**VS Code better for:**
- Multi-language work
- Lightweight needs
- Extension ecosystem
- Remote development

**JetBrains better for:**
- Single-language focus
- Advanced refactoring
- Integrated tools
- Team settings

### Exercise 80: IDE Comparison

**Task**: Try both IDEs with Claude (if available)

1. Complete a task in VS Code
2. Complete same task in JetBrains (if you use it)
3. Note differences
4. Choose your preference
5. Optimize that environment

**Verification**:
- [ ] Tried both environments
- [ ] Identified preferences
- [ ] Optimized chosen IDE

---

## Section 12 Summary

### Key Takeaways

1. **VS Code** has first-party Claude extension
2. **Inline diffs** make review seamless
3. **@-mentions** reference files precisely
4. **Multiple conversations** separate contexts
5. **Remote development** works transparently

### Essential Shortcuts

| Action | VS Code | JetBrains |
|--------|---------|-----------|
| Open Claude | Ctrl+Shift+C | Alt+C |
| Send selection | Ctrl+L | Alt+Enter |
| New conversation | Ctrl+K Ctrl+C | Alt+N |
| Accept diff | Enter | Enter |

### IDE Features
- Inline diff review
- @-mentions with line numbers
- Multiple conversation tabs
- Command palette integration
- Remote development support

---

## Advanced Level Complete!

You've finished Lessons 51-80. You can now:
- Configure and use MCP servers
- Build custom subagents
- Use plan mode strategically
- Integrate Claude deeply into your IDE

### Next Steps
Proceed to Expert Level (Lessons 81-100+) for plugins, enterprise deployment, and advanced patterns.
