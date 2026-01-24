# Section 5: Basic Configuration (Lessons 21-25)

## Overview
Configure Claude Code for optimal productivity with project context, settings, and session management.

---

## Lesson 21: Introduction to CLAUDE.md

### Objectives
- Understand CLAUDE.md purpose and power
- Learn what to include for maximum effectiveness
- Create your first CLAUDE.md file
- Test that Claude uses your configuration

### What is CLAUDE.md?

CLAUDE.md is a markdown file that gives Claude context about your project. It's like an onboarding document for an AI teammate.

**Location options:**
- `CLAUDE.md` - Project root (recommended)
- `.claude/CLAUDE.md` - Hidden configuration
- `~/.claude/CLAUDE.md` - User-level defaults

### Why CLAUDE.md Matters

Without CLAUDE.md:
```
You: "Add a new API endpoint"
Claude: Uses generic patterns, might not match your style
```

With CLAUDE.md:
```
You: "Add a new API endpoint"
Claude: Follows your patterns, uses your conventions, integrates properly
```

### Essential Sections

```markdown
# Project Name

## Overview
Brief project description and purpose.

## Tech Stack
- Frontend: React 18, TypeScript, Tailwind
- Backend: Node.js, Express, PostgreSQL
- Testing: Jest, React Testing Library

## Project Structure
src/
  components/    # React components
  api/          # API routes
  utils/        # Helper functions
  types/        # TypeScript types

## Coding Conventions
- Use functional components with hooks
- Prefer named exports
- Use async/await over .then()
- Error handling: try/catch with custom Error classes

## Commands
- npm run dev - Start development server
- npm test - Run tests
- npm run build - Production build

## Important Notes
- Never commit .env files
- All API routes need authentication middleware
- Use the logger utility, not console.log
```

### Exercise 21: Create Your First CLAUDE.md

**Task**: Create CLAUDE.md for an existing project

1. Navigate to any project you're working on
2. Create CLAUDE.md with:
   - Project overview
   - Tech stack
   - Key commands
   - One coding convention

3. Test it:
   - Start new Claude session
   - Ask: "What do you know about this project?"
   - Verify Claude references your CLAUDE.md

**Verification**:
- [ ] CLAUDE.md created
- [ ] Claude acknowledges project context
- [ ] Context influences Claude's suggestions

---

## Lesson 22: Settings Overview

### Objectives
- Understand .claude directory structure
- Learn settings hierarchy and precedence
- View and interpret current settings
- Know available configuration scopes

### The .claude Directory

```
~/.claude/                    # User level
├── settings.json            # User settings
├── CLAUDE.md               # User context
├── skills/                 # User skills
└── plugins/                # User plugins

project/
└── .claude/                 # Project level
    ├── settings.json       # Project settings
    ├── settings.local.json # Local overrides (gitignored)
    ├── CLAUDE.md          # Project context
    └── skills/            # Project skills
```

### Settings Hierarchy (Precedence)

```
1. Command line flags (highest)
2. Environment variables
3. Project .claude/settings.local.json
4. Project .claude/settings.json
5. User ~/.claude/settings.json
6. Enterprise managed settings
7. Defaults (lowest)
```

### Viewing Current Settings

```bash
# In Claude session
/settings

# Or ask Claude
"Show me my current settings"
```

### Common Settings

```json
{
  "permissions": {
    "allow": ["Read", "Glob", "Grep"],
    "deny": ["Bash(rm -rf *)"]
  },
  "preferences": {
    "model": "sonnet",
    "autoCompact": true
  },
  "hooks": {
    "PostToolUse": []
  }
}
```

### Exercise 22: Explore Your Settings

**Task**: Understand your current configuration

1. Run: `ls -la ~/.claude/`
2. Run: `ls -la .claude/` (in a project)
3. Ask Claude: "What settings are currently active?"
4. Identify which scope each setting comes from

**Verification**:
- [ ] Can locate settings files
- [ ] Understand settings hierarchy
- [ ] Can view active configuration

---

## Lesson 23: User Settings

### Objectives
- Configure user-level preferences
- Set default permission modes
- Configure model preferences
- Customize keyboard behavior

### Creating User Settings

```bash
# Create user settings directory
mkdir -p ~/.claude
```

Create `~/.claude/settings.json`:

```json
{
  "preferences": {
    "defaultModel": "sonnet",
    "theme": "dark",
    "verboseMode": false
  },
  "permissions": {
    "defaultMode": "normal",
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test*)",
      "Bash(npm run*)",
      "Bash(git status)",
      "Bash(git diff*)",
      "Bash(git log*)"
    ]
  },
  "behavior": {
    "autoCompact": true,
    "compactThreshold": 80
  }
}
```

### Permission Modes

**Normal mode (default):**
- Asks permission for each tool
- Best for learning and careful work

**Auto-accept mode:**
- Approves allowed operations automatically
- Faster for routine work

**Plan mode:**
- Read-only exploration
- Reviews plan before execution

### Model Selection

```json
{
  "preferences": {
    "defaultModel": "sonnet"
  }
}
```

Options:
- `"sonnet"` - Balanced (recommended default)
- `"opus"` - Most capable, more expensive
- `"haiku"` - Fast, lightweight tasks

### Exercise 23: Configure User Preferences

**Task**: Set up your user-level configuration

1. Create `~/.claude/settings.json`
2. Add preferences for:
   - Default model
   - Commonly allowed commands
   - Auto-compact behavior

3. Restart Claude session
4. Verify settings apply

**Verification**:
- [ ] User settings file created
- [ ] Preferences take effect
- [ ] Commonly used commands pre-approved

---

## Lesson 24: Project Settings

### Objectives
- Configure project-level settings
- Share settings with team via git
- Use local settings for personal preferences
- Override user defaults appropriately

### Project Settings Structure

```
project/
└── .claude/
    ├── settings.json       # Shared (committed to git)
    └── settings.local.json # Personal (gitignored)
```

### Shared Settings (settings.json)

Team-wide configuration:

```json
{
  "project": {
    "name": "my-app",
    "type": "typescript-react"
  },
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test*)",
      "Bash(npm run build)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit:.*\\.(ts|tsx)$",
        "command": "npm run lint:fix -- $FILE_PATH"
      }
    ]
  },
  "conventions": {
    "testPattern": "**/*.test.ts",
    "componentPattern": "src/components/**/*.tsx"
  }
}
```

### Local Settings (settings.local.json)

Personal preferences not shared:

```json
{
  "preferences": {
    "model": "opus",
    "verbose": true
  },
  "permissions": {
    "allow": [
      "Bash(docker-compose*)"
    ]
  }
}
```

### Gitignore Setup

Add to `.gitignore`:
```
.claude/settings.local.json
```

### Exercise 24: Set Up Project Configuration

**Task**: Configure project-level settings

1. Create `.claude/settings.json` with:
   - Project-specific allowed commands
   - A PostToolUse hook for formatting

2. Create `.claude/settings.local.json` with:
   - Your model preference
   - Personal allowed commands

3. Update `.gitignore` to exclude local settings
4. Verify both apply correctly

**Verification**:
- [ ] Project settings created
- [ ] Local settings created and gitignored
- [ ] Both layers apply correctly

---

## Lesson 25: Session Management

### Objectives
- Name sessions meaningfully for later reference
- Resume previous sessions with context intact
- Use /history to find old conversations
- Clear and manage sessions effectively

### Naming Sessions

```bash
# Start named session
claude --session "feature-auth-system"

# Or name during session
/name feature-auth-system
```

### Finding Previous Sessions

```bash
# List recent sessions
/history

# Search sessions
/history auth

# Shows:
# 1. feature-auth-system (2 hours ago)
# 2. auth-bug-fix (yesterday)
# 3. authentication-refactor (3 days ago)
```

### Resuming Sessions

```bash
# Resume by name
claude --resume "feature-auth-system"

# Or use /resume in active session
/resume feature-auth-system
```

### Session Benefits

Resuming preserves:
- Conversation history
- Decisions made
- Context about the task
- Files discussed

### Managing Sessions

```bash
# Clear current session (start fresh)
/clear

# End session explicitly
/exit

# View session info
/session
```

### Session Strategy

**Good naming patterns:**
- `feature-[name]` - New features
- `bugfix-[issue]` - Bug fixes
- `refactor-[area]` - Refactoring
- `explore-[topic]` - Research/exploration

**When to start fresh:**
- Switching to unrelated task
- Context becomes cluttered
- After major milestone

**When to resume:**
- Continuing interrupted work
- Adding to previous feature
- Following up on issues

### Exercise 25: Session Workflow

**Task**: Practice session management

1. Start a named session: `claude --session "practice-session"`
2. Have a brief conversation about a task
3. Exit with `/exit`
4. List sessions with `/history`
5. Resume your session
6. Verify context is preserved

**Verification**:
- [ ] Can name sessions
- [ ] Can find sessions in history
- [ ] Can resume with context intact
- [ ] Understand when to resume vs start fresh

---

## Section 5 Summary

### Key Takeaways

1. **CLAUDE.md** provides essential project context
2. **Settings hierarchy** controls configuration precedence
3. **User settings** apply globally
4. **Project settings** can be shared via git
5. **Session management** preserves context across time

### Configuration Files
- `~/.claude/settings.json` - User preferences
- `.claude/settings.json` - Project (shared)
- `.claude/settings.local.json` - Project (personal)
- `CLAUDE.md` - Project context

### Commands Learned
- `/settings` - View settings
- `/name` - Name session
- `/history` - Find sessions
- `/resume` - Continue session
- `/clear` - Fresh start

### Beginner Level Complete!

Congratulations! You've completed the Beginner Level (Lessons 1-25).

**You can now:**
- Use all core file tools effectively
- Work with Git through Claude
- Leverage web tools appropriately
- Configure Claude for your workflow
- Manage sessions productively

### Next Steps
Proceed to Intermediate Level (Lessons 26-50) to learn advanced tools, skills, and hooks.
