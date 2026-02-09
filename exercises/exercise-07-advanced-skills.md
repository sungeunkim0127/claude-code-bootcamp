# Exercise 7: Advanced Skill Development

**Time**: 1 hour
**Prerequisites**: Lessons 36-42

## Objective
Create, configure, and share advanced skills with multi-step workflows, triggers, model selection, and supporting files.

## Setup

```bash
mkdir -p ~/claude-exercises/ex7/src ~/claude-exercises/ex7/tests ~/claude-exercises/ex7/.claude/skills
cd ~/claude-exercises/ex7
git init

cat > src/app.js << 'EOF'
const express = require('express')
const app = express()
app.use(express.json())
app.get('/api/health', (req, res) => res.json({ status: 'ok' }))
app.listen(3000)
module.exports = app
EOF

cat > src/utils.js << 'EOF'
function formatDate(date) { return date.toISOString().split('T')[0] }
function slugify(text) { return text.toLowerCase().replace(/\s+/g, '-') }
function capitalize(str) { return str.charAt(0).toUpperCase() + str.slice(1) }
module.exports = { formatDate, slugify, capitalize }
EOF

cat > tests/utils.test.js << 'EOF'
const { formatDate, slugify, capitalize } = require('../src/utils')
test('slugify converts spaces to hyphens', () => {
  expect(slugify('hello world')).toBe('hello-world')
})
EOF

cat > package.json << 'EOF'
{ "name": "ex7", "scripts": { "test": "jest", "lint": "eslint src/" } }
EOF

mkdir -p ~/.claude/skills
claude
```

## Tasks

### Task 1: Create a Multi-Step Workflow Skill (15 min)

Create a skill that performs a structured API endpoint generation workflow.

Create the file `.claude/skills/generate-endpoint/SKILL.md` with this content:

```markdown
---
name: generate-endpoint
description: Generate a complete REST API endpoint with route, controller, validation, and tests
commands:
  - generate-endpoint
  - gen-api
tools:
  - Read
  - Write
  - Grep
  - Glob
  - Bash
---

# Generate REST API Endpoint

When invoked with an entity name (e.g., `/generate-endpoint products`):

## Step 1: Analyze Existing Patterns
- Use Grep to find existing route patterns in `src/`
- Read existing route files to understand the project's conventions
- Note middleware usage, error handling style, and response format

## Step 2: Generate Route File
Create `src/routes/{entity}.js` with:
- GET / (list all)
- GET /:id (get one)
- POST / (create)
- PUT /:id (update)
- DELETE /:id (delete)
Follow the same patterns found in Step 1.

## Step 3: Generate Validation
Create `src/validation/{entity}.js` with:
- Input validation schemas for create and update
- Use the same validation library as existing code, or use basic checks

## Step 4: Generate Tests
Create `tests/{entity}.test.js` with:
- Tests for each endpoint
- Cover success and error cases
- Use the same test patterns as existing tests

## Step 5: Register Route
- Add the new route to `src/app.js`
- Follow existing import and registration patterns

## Step 6: Summary
Report what was created:
- Files generated
- Endpoints available
- Tests written
```

**Verify**: Run `/generate-endpoint products` and confirm it creates route, validation, and test files

---

### Task 2: Add Advanced Frontmatter (10 min)

Create a skill with triggers, model specification, and tool restrictions.

Create the file `.claude/skills/quick-review/SKILL.md`:

```markdown
---
name: quick-review
description: Fast code review focusing on bugs and security issues
commands:
  - quick-review
  - qr
triggers:
  - pattern: "review this (file|code|change)"
    command: quick-review
model: sonnet
autoInvoke: false
tools:
  - Read
  - Grep
  - Glob
---

# Quick Code Review

Perform a fast, focused code review on the specified file or directory.

## Review Checklist
1. **Security**: SQL injection, XSS, hardcoded secrets, insecure defaults
2. **Bugs**: Null references, off-by-one errors, unhandled promises, race conditions
3. **Performance**: N+1 queries, unnecessary loops, missing indexes
4. **Best Practices**: Error handling, input validation, logging

## Output Format
```text
## Quick Review: {filename}

### Critical Issues
- [SECURITY] Description (line N)
- [BUG] Description (line N)

### Warnings
- [PERF] Description (line N)
- [STYLE] Description (line N)

### Summary
X critical, Y warnings found
```

## Constraints
- Read-only: Do NOT modify any files
- Focus on high-impact issues only
- Keep review under 20 items
```

Test the trigger by asking Claude: "review this file src/app.js"

**Verify**: The skill triggers automatically from the phrase, uses only read-only tools, and outputs in the defined format

---

### Task 3: Create a Skill with Templates and Supporting Files (10 min)

Create a skill that uses supporting template files for code generation.

```bash
mkdir -p .claude/skills/component-generator/templates
```

Create `.claude/skills/component-generator/SKILL.md`:

```markdown
---
name: component-generator
description: Generate a React component from a template with tests and styles
commands:
  - gen-component
tools:
  - Read
  - Write
  - Glob
---

# Component Generator

Generate a React component using the templates in this skill's directory.

## Steps
1. Read the template from `templates/component.template`
2. Replace `{{ComponentName}}` with the provided name (PascalCase)
3. Replace `{{componentName}}` with the camelCase version
4. Create the component file at `src/components/{ComponentName}/{ComponentName}.jsx`
5. Read and apply the test template from `templates/test.template`
6. Create the test at `src/components/{ComponentName}/{ComponentName}.test.jsx`
7. Report created files
```

Create `.claude/skills/component-generator/templates/component.template`:

```text
import React from 'react'

function {{ComponentName}}({ children, className = '' }) {
  return (
    <div className={`{{componentName}} ${className}`}>
      {children}
    </div>
  )
}

export default {{ComponentName}}
```

Create `.claude/skills/component-generator/templates/test.template`:

```text
import { render, screen } from '@testing-library/react'
import {{ComponentName}} from './{{ComponentName}}'

describe('{{ComponentName}}', () => {
  it('renders children', () => {
    render(<{{ComponentName}}>Hello</{{ComponentName}}>)
    expect(screen.getByText('Hello')).toBeInTheDocument()
  })

  it('applies custom className', () => {
    const { container } = render(<{{ComponentName}} className="custom" />)
    expect(container.firstChild).toHaveClass('custom')
  })
})
```

**Verify**: Run `/gen-component UserProfile` and confirm both the component and test file are created with correct substitutions

---

### Task 4: Test and Debug the Skill (10 min)

Systematically test the skills you created:

1. Test the endpoint generator with an edge case:
   ```
   /generate-endpoint order-items
   ```
   Check that hyphenated names are handled correctly.

2. Test the quick-review skill on a file with known issues:
   ```bash
   cat > src/risky.js << 'EOF'
   const password = "admin123"
   function query(input) { return `SELECT * FROM users WHERE name = '${input}'` }
   async function fetchData() { fetch('/api').then(r => r.json()) }
   EOF
   ```
   Ask Claude: "review this file src/risky.js"

3. Test the component generator with unusual input:
   ```
   /gen-component my-widget
   ```
   Verify it converts the name to PascalCase (MyWidget).

**Verify**: All three skills handle edge cases correctly without errors

---

### Task 5: Share Skills at Project Level (10 min)

Set up the skills for team sharing via git:

1. Confirm the skills are in `.claude/skills/` (project level, not `~/.claude/skills/`):
   ```bash
   ls -la .claude/skills/
   ```

2. Add the skills to version control:
   ```
   "Stage and commit all the skills in .claude/skills/ with a message describing what each skill does"
   ```

3. Create a skill index by asking Claude:
   ```
   "Read all SKILL.md files in .claude/skills/ and create a summary table listing each skill's name, commands, and description"
   ```

4. Verify that a teammate could use the skills by checking:
   - All SKILL.md files have complete frontmatter
   - Templates are included alongside the skill
   - No absolute paths or machine-specific references

**Verify**: `git log` shows the commit, and all skills are properly tracked in the repository

---

### Task 6: Skill Interaction and Chaining (5 min)

Test using multiple skills together:

```
"First use /generate-endpoint comments, then use /quick-review on the generated route file"
```

**Expected**:
- First skill generates the endpoint files
- Second skill reviews the generated code
- Any issues from generation are caught by review

**Verify**: The review output references the generated file and provides relevant feedback

---

## Completion Criteria

- [ ] Created a multi-step workflow skill with 5+ steps
- [ ] Used advanced frontmatter: triggers, model, tool restrictions
- [ ] Built a skill with external template files
- [ ] Tested skills with edge cases and unusual input
- [ ] Shared skills at project level via git
- [ ] Successfully chained two skills together
