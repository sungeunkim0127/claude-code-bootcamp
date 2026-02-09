# Section 7: Skills System (Lessons 33-42)

## Overview
Master the Skills system to create reusable, shareable expertise. Skills are one of Claude Code's most powerful features for customization and automation.

---

## Lesson 33: What Are Skills?

### Objectives
- Understand skills as reusable expertise
- Learn skill scopes (enterprise, personal, project, plugin)
- Explore built-in skills
- Know when to create skills

### Content

#### What Are Skills?

**Skills** are reusable expertise packages that extend Claude's capabilities with specialized knowledge and workflows.

**Think of Skills As**:
- Specialized assistants for specific tasks
- Reusable templates for common workflows
- Codified best practices
- Shared team knowledge

#### How Skills Work

```
User: /code-review

Claude:
  [Loads code-review skill]
  [Follows skill instructions]
  [Uses skill-specific tools]
  [Produces skill-defined output]
```

**Behind the Scenes**:
1. Skill SKILL.md loaded into context
2. Claude follows skill instructions
3. Tools restricted to skill's allowed list
4. Output formatted per skill requirements

#### Skill Components

**SKILL.md File**:
```markdown
---
name: skill-name
description: What this skill does
commands:
  - command-name
triggers:
  - pattern: "trigger phrase"
    command: command-name
model: sonnet
autoInvoke: false
tools:
  - Read
  - Write
---

# Skill Instructions

[Detailed instructions for Claude]
```

**Key Parts**:
- **Frontmatter** (YAML): Configuration
- **Instructions**: What Claude should do
- **Examples**: How to use the skill
- **Supporting Files**: Templates, examples

#### Skill Scopes

**1. Enterprise Skills** (`/enterprise/skills/`)
- Managed by organization
- Available to all team members
- Enforced standards
- Examples: Security review, deployment checklist

**2. Personal Skills** (`~/.claude/skills/`)
- Your personal skills
- Available in all projects
- Examples: Personal code style, common tasks

**3. Project Skills** (`.claude/skills/`)
- Specific to one project
- Shared via git
- Examples: Project-specific workflows

**4. Plugin Skills** (via plugins)
- Bundled with plugins
- Installed from marketplace
- Examples: Community-contributed skills

**Precedence**: Enterprise → Personal → Project → Plugin

#### Built-in vs Custom Skills

**Built-in Skills** (if available):
- Pre-installed with Claude Code
- Common workflows
- Maintained by Anthropic
- Examples: Basic git, testing, documentation

**Custom Skills**:
- Created by you or your team
- Specialized to your needs
- Shareable with others
- Examples: Company-specific workflows

#### When to Create a Skill

**Create a Skill When**:
✅ You do the same task repeatedly
✅ Task has clear, reusable steps
✅ Others could benefit from the workflow
✅ You want consistent results
✅ Task requires specific domain knowledge

**Examples**:
- Code review with company standards
- Creating new components following patterns
- Deployment checklists
- Database migration workflows
- API endpoint generation

**Don't Create a Skill When**:
❌ Task is one-off
❌ Steps vary significantly each time
❌ Better handled by hooks (automatic actions)
❌ Too simple (just ask Claude directly)

#### Skill Discovery

**List Available Skills**:
```bash
claude skills list
# or in session:
/skills
```

**Skill Information**:
```bash
claude skills info code-review
```

**Search Skills**:
```bash
claude skills search "review"
```

#### Invoking Skills

**1. Command Syntax**:
```
/skill-name
/skill-name arg1 arg2
```

**2. Auto-Invocation**:
```
User: "review this code"
→ Triggers /code-review skill automatically
```

**3. Skill Tool**:
```
Use Skill tool with skill name
```

#### Skill Benefits

**Consistency**:
- Same process every time
- Standardized output
- Reduced errors

**Efficiency**:
- Don't repeat instructions
- Faster execution
- One command vs multiple prompts

**Knowledge Sharing**:
- Codify best practices
- Onboard new team members
- Share expertise

**Quality**:
- Comprehensive checklists
- Don't forget steps
- Proven workflows

### Exercise

**Task**: Explore and understand skills

**Setup**:
```bash
# Check if you have any skills
ls ~/.claude/skills/

# If not, create directory
mkdir -p ~/.claude/skills/
```

**Tasks**:

1. **Conceptual Understanding**:
```
Think of 3 tasks you do repeatedly in your work.
For each, answer:
- What are the steps?
- Could this be a skill?
- What would the skill do?
```

2. **Explore Skill Structure**:
```
Look at the skill template:
cat ~/claude-code-bootcamp/templates/SKILL-TEMPLATE.md

Identify:
- Where is the configuration?
- Where are the instructions?
- Where are the examples?
```

3. **Read Example Skill**:
```
Read the code review skill:
cat ~/claude-code-bootcamp/resources/example-skills/code-review-skill/SKILL.md

Understand:
- What does it do?
- What tools does it use?
- How is it structured?
```

4. **Plan Your First Skill**:
```
Write down:
- Skill name: ___________
- What it does: ___________
- Steps it follows: ___________
- Tools it needs: ___________
```

### Verification

Before moving on, ensure you understand:
- [ ] What skills are and how they work
- [ ] The four skill scopes
- [ ] When to create a skill vs not
- [ ] How to invoke skills
- [ ] Basic skill structure

### Key Concepts

**Skill vs Prompt**:
- Prompt: One-time instruction
- Skill: Reusable workflow

**Skill vs Hook**:
- Skill: Manually invoked
- Hook: Automatically triggered

**Skill vs Subagent**:
- Skill: Instructions + limited tools
- Subagent: Full agent with custom behavior

---

## Lesson 34: Using Existing Skills

### Objectives
- Invoke skills with /command syntax
- Pass arguments to skills
- Understand auto-invocation
- Use Skill tool

### Content

#### Basic Skill Invocation

**Command Syntax**:
```
/skill-name
```

**Example**:
```
You: /code-review

Claude:
  [Loads code-review skill]
  "I'll perform a comprehensive code review..."
  [Executes review workflow]
```

#### Passing Arguments

**With Arguments**:
```
/skill-name arg1 arg2 arg3
```

**Example**:
```
You: /code-review src/api/users.js

Claude:
  "I'll review src/api/users.js..."
  [Reviews specified file]
```

**Multiple Arguments**:
```
You: /deploy production --skip-tests

Claude:
  "Deploying to production, skipping tests..."
```

#### Auto-Invocation

**Trigger Patterns**:
Skills can auto-trigger based on patterns:

```yaml
triggers:
  - pattern: "review (this|the) code"
    command: review
```

**Example**:
```
You: "Can you review this code?"

Claude:
  [Auto-invokes code-review skill]
  "I'll perform a code review..."
```

**Benefits**:
- Natural language interaction
- No need to remember exact commands
- Seamless experience

**Disable Auto-Invoke**:
```yaml
autoInvoke: false
```

#### Using the Skill Tool

**Explicit Skill Tool**:
```
"Use the code-review skill on these files"
```

Claude will:
1. Recognize skill reference
2. Load skill
3. Execute workflow

**With Fully Qualified Name**:
```
"Use enterprise:code-review skill"
"Use plugin:awesome-plugin:formatter skill"
```

#### Skill Context

**What Skills See**:
- Current conversation history
- Files you've mentioned
- Current working directory
- Skill instructions
- Supporting files

**What Skills Don't See**:
- Other skills' internals
- Hook configurations (unless specified)
- Your full chat history (only current session)

#### Common Built-in Skills (if available)

**Git Skills**:
```
/commit              # Create git commit
/commit-fix          # Fix commit message
/branch              # Create/switch branch
```

**Testing Skills**:
```
/test                # Run tests
/test-gen            # Generate tests
/coverage            # Check coverage
```

**Documentation Skills**:
```
/doc                 # Generate docs
/readme              # Create/update README
/api-doc             # API documentation
```

**Deployment Skills**:
```
/deploy              # Deploy application
/rollback            # Rollback deployment
```

#### Skill Best Practices

**When Using Skills**:

1. **Read Skill Description First**:
```
/skills info code-review
```

2. **Provide Required Context**:
```
# Bad
/deploy

# Good
/deploy production --version v2.1.0
```

3. **Check Output**:
- Review what skill did
- Verify results
- Ask for clarification if needed

4. **Combine Skills**:
```
/test
# If tests pass:
/deploy staging
```

### Exercise

**Task**: Use skills effectively

**Setup**:
```bash
cd ~/claude-practice
claude
```

**Tasks**:

1. **List Available Skills** (if supported):
```
/skills
# or
"What skills are available?"
```

2. **Get Skill Info** (if supported):
```
/skills info code-review
# or
"Tell me about the code-review skill"
```

3. **Invoke a Skill**:
```
# If you have code-review skill installed:
/code-review

# Otherwise, simulate:
"Perform a code review following these steps:
1. Check for security issues
2. Check code quality
3. Check performance
4. Generate report"
```

4. **Pass Arguments**:
```
/code-review src/app.js
# or
"Review src/app.js for security and performance"
```

5. **Chain Skills**:
```
"First run tests, then if they pass, create a commit"
```

### Verification

Before moving on, ensure you can:
- [ ] Invoke skills with /command syntax
- [ ] Pass arguments to skills
- [ ] Understand auto-invocation
- [ ] Get skill information
- [ ] Chain multiple skills

---

## Lesson 35: Creating Your First Skill

### Objectives
- Create SKILL.md file
- Write skill instructions
- Set up skill directory
- Test your skill

### Content

#### Skill File Structure

**Basic Structure**:
```
~/.claude/skills/my-skill/
├── SKILL.md          # Main skill file
├── templates/        # Optional templates
└── examples/         # Optional examples
```

**Single-File Skill**:
```
~/.claude/skills/my-skill.md
```

#### Creating SKILL.md

**Minimal Skill**:
```markdown
---
name: my-skill
description: Brief description
commands:
  - my-command
---

# My Skill

Instructions for Claude to follow.
```

**Step-by-Step**:

1. **Create Directory**:
```bash
mkdir -p ~/.claude/skills/hello-world
cd ~/.claude/skills/hello-world
```

2. **Create SKILL.md**:
```bash
cat > SKILL.md << 'EOF'
---
name: hello-world
description: A simple hello world skill
commands:
  - hello
  - greet
---

# Hello World Skill

When invoked, greet the user enthusiastically.

## Instructions

1. Generate a friendly greeting
2. Include a random fun fact
3. Wish them a great day
EOF
```

3. **Test the Skill**:
```bash
claude
/hello
```

#### Writing Effective Instructions

**Structure Your Instructions**:

```markdown
# Skill Name

Brief overview of what this skill does.

## Purpose
[Why this skill exists]

## Instructions

### Step 1: [First Step]
[Detailed instructions]

### Step 2: [Second Step]
[Detailed instructions]

## Output Format
[How to format results]

## Examples
[Show examples]
```

**Be Specific**:
```markdown
# Bad (vague)
Check the code for issues.

# Good (specific)
Analyze the code for:
1. Security vulnerabilities (SQL injection, XSS, auth bypasses)
2. Performance issues (N+1 queries, inefficient algorithms)
3. Code quality (complexity >10, duplication, naming)
4. Best practices violations

For each issue found:
- Severity: Critical/High/Medium/Low
- Location: file:line
- Problem: What's wrong
- Fix: How to fix it
```

**Use Numbered Steps**:
```markdown
1. Read all files in src/
2. Identify functions >50 lines
3. For each function:
   a. Calculate complexity
   b. Check for duplication
   c. Suggest refactoring if needed
4. Generate report with:
   - Total functions analyzed
   - Functions needing refactoring
   - Specific recommendations
```

#### Example: Documentation Skill

**Complete Example**:
```markdown
---
name: doc-function
description: Generates JSDoc documentation for functions
commands:
  - doc-function
  - document
model: sonnet
tools:
  - Read
  - Edit
---

# Function Documentation Skill

Generates comprehensive JSDoc documentation for JavaScript/TypeScript functions.

## Purpose
Ensure all functions have proper documentation following JSDoc standards.

## Instructions

### Step 1: Identify Function
If file and function name provided, read that file.
If not provided, ask which function to document.

### Step 2: Analyze Function
Analyze the function:
- Parameters and their types
- Return value and type
- What the function does
- Potential errors/exceptions
- Examples of usage

### Step 3: Generate JSDoc
Create JSDoc comment with:
```javascript
/**
 * [Brief description]
 *
 * [Detailed description if complex]
 *
 * @param {type} paramName - Description
 * @param {type} [optionalParam] - Description (optional)
 * @returns {type} Description of return value
 * @throws {ErrorType} When/why it throws
 * @example
 * // Example usage
 * functionName(arg1, arg2)
 */
```

### Step 4: Add Documentation
Use Edit tool to add JSDoc above the function.

### Step 5: Verify
Show the documented function for review.

## Example Usage

```
User: /doc-function src/utils.js calculateTotal

Output: [Adds JSDoc to calculateTotal function]
```

## Notes
- Follow existing project JSDoc style if present
- Include @example for non-trivial functions
- Use TypeScript types if .ts file
```

### Exercise

**Task**: Create your first useful skill

**Skill to Build**: Code formatter checker

**Requirements**:
```markdown
Name: format-check
Description: Checks if code follows formatting rules
Commands: format-check

Should check:
- Consistent indentation (2 spaces)
- No trailing whitespace
- Semicolons present (for JS)
- Proper line breaks
- Max line length (80 chars)

Output: List of formatting issues found
```

**Steps**:

1. **Create Directory**:
```bash
mkdir -p ~/.claude/skills/format-check
```

2. **Write SKILL.md**:
```bash
# Use your editor or ask Claude:
"Create SKILL.md for a format-check skill that:
[paste requirements above]

Follow the structure from the documentation skill example."
```

3. **Test It**:
```bash
# Create a test file with bad formatting
cat > ~/claude-practice/messy.js << 'EOF'
function test(){
var x=1;
  return x
}
EOF

claude
/format-check messy.js
```

4. **Refine**:
```
Based on the results, refine your skill:
- Add more checks
- Improve output format
- Add color coding (if wanted)
```

### Verification

Before moving on, ensure you can:
- [ ] Create SKILL.md file with frontmatter
- [ ] Write clear, specific instructions
- [ ] Organize instructions in steps
- [ ] Test your skill
- [ ] Refine based on results

### Common Mistakes

1. **Too Vague**:
```markdown
# Bad
Check the code.

# Good
Check the code for:
1. [Specific thing]
2. [Specific thing]
```

2. **No Structure**:
```markdown
# Bad
Just a wall of text instructions...

# Good
## Step 1: ...
## Step 2: ...
```

3. **Missing Output Format**:
```markdown
# Add this section:
## Output Format
Present results as:
- Summary
- Details
- Recommendations
```

---

## Lesson 36: Advanced Skill Configuration

### Objectives
- Configure skill frontmatter options
- Control tool access for skills
- Set up model selection
- Use triggers and auto-invocation

### Content

#### Frontmatter Deep Dive

The YAML frontmatter controls how your skill behaves:

```yaml
---
name: code-review
description: Comprehensive code review with security focus
commands:
  - review
  - code-review
  - cr
triggers:
  - pattern: "review (this|the|my) code"
    command: review
  - pattern: "check.*for (bugs|issues|problems)"
    command: review
model: sonnet
autoInvoke: true
tools:
  - Read
  - Glob
  - Grep
  - Bash
---
```

#### Tool Restrictions

Limiting which tools a skill can use improves safety and focus:

**Read-Only Skill** (safe for review tasks):
```yaml
tools:
  - Read
  - Glob
  - Grep
```

**Full Access Skill** (for generation/modification):
```yaml
tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
```

**Why Restrict Tools?**:
- Prevents accidental file modifications during review
- Ensures skills stay within their intended scope
- Makes skills safer to share with others
- Reduces risk of unintended side effects

#### Model Selection

Choose the right model for your skill's complexity:

```yaml
# Quick, simple tasks
model: haiku

# General-purpose (default)
model: sonnet

# Complex reasoning
model: opus
```

**Guidelines**:
| Task Type | Recommended Model |
|-----------|------------------|
| Formatting, linting | Haiku |
| Code review, generation | Sonnet |
| Architecture analysis, complex refactoring | Opus |

#### Trigger Patterns

Auto-invoke skills based on natural language:

```yaml
triggers:
  - pattern: "review (this|the) (code|PR|pull request)"
    command: review
  - pattern: "create.*component"
    command: create-component
  - pattern: "generate.*test"
    command: gen-test
```

**Pattern Tips**:
- Use `(a|b)` for alternatives
- Use `.*` for flexible matching
- Keep patterns specific to avoid false triggers
- Test patterns with multiple phrasings

#### Multiple Commands

One skill can respond to multiple commands:

```yaml
commands:
  - review          # Full review
  - quick-review    # Quick review
  - security-check  # Security only
```

Your instructions can check which command was used:
```markdown
## Instructions

If invoked as `/review`: Perform full comprehensive review.
If invoked as `/quick-review`: Check only critical issues.
If invoked as `/security-check`: Focus exclusively on security.
```

### Exercise

**Task**: Create a skill with advanced configuration

**Create `~/.claude/skills/smart-review/SKILL.md`**:
```markdown
---
name: smart-review
description: Context-aware code review
commands:
  - smart-review
  - sr
triggers:
  - pattern: "review (this|the|my) (code|changes|file)"
    command: smart-review
model: sonnet
autoInvoke: false
tools:
  - Read
  - Glob
  - Grep
---

# Smart Review

Context-aware review that adapts to the type of code being reviewed.

## Instructions

1. Read the specified file(s)
2. Detect the language and framework
3. Apply language-specific checks:
   - JavaScript/TypeScript: async/await patterns, null checks, type safety
   - Python: type hints, exception handling, PEP 8
   - Rust: ownership, error handling, unsafe usage
4. Check for security issues appropriate to the language
5. Output findings grouped by severity
```

**Test It**:
```bash
claude
/smart-review src/app.js
```

### Verification

Before moving on, ensure you can:
- [ ] Write complete frontmatter configuration
- [ ] Restrict tool access appropriately
- [ ] Choose the right model for a skill
- [ ] Set up trigger patterns
- [ ] Create multi-command skills

---

## Lesson 37: Skill Templates and Supporting Files

### Objectives
- Use templates within skills
- Include supporting files and examples
- Reference external resources
- Build multi-file skill packages

### Content

#### Skill Directory Structure

Complex skills use multiple files:

```
~/.claude/skills/component-generator/
├── SKILL.md              # Main instructions
├── templates/
│   ├── component.tsx     # Component template
│   ├── component.test.tsx # Test template
│   └── component.css     # Style template
├── examples/
│   ├── simple.tsx        # Simple component example
│   └── complex.tsx       # Complex component example
└── config/
    └── defaults.json     # Default settings
```

#### Using Templates in Instructions

Reference templates for Claude to follow:

```markdown
# Component Generator Skill

## Instructions

### Step 1: Determine Component Type
Ask the user or infer:
- Functional component (default)
- Form component
- List component
- Layout component

### Step 2: Generate Component
Use the template from `templates/component.tsx` as the base.
Replace placeholders:
- `{{COMPONENT_NAME}}` with the provided name
- `{{PROPS_INTERFACE}}` with generated props
- `{{COMPONENT_BODY}}` with appropriate JSX

### Step 3: Generate Test
Use `templates/component.test.tsx` as the base.
Create tests for:
- Rendering without errors
- Props are applied correctly
- User interactions work
- Edge cases

### Step 4: Generate Styles
Use `templates/component.css` as the base.
Follow the project's CSS methodology (BEM, CSS modules, etc.)
```

#### Template Files

**`templates/component.tsx`**:
```tsx
import React from 'react'
import styles from './{{COMPONENT_NAME}}.module.css'

interface {{COMPONENT_NAME}}Props {
  {{PROPS_INTERFACE}}
}

export function {{COMPONENT_NAME}}({ {{DESTRUCTURED_PROPS}} }: {{COMPONENT_NAME}}Props) {
  return (
    <div className={styles.container}>
      {{COMPONENT_BODY}}
    </div>
  )
}
```

**`templates/component.test.tsx`**:
```tsx
import { render, screen } from '@testing-library/react'
import { {{COMPONENT_NAME}} } from './{{COMPONENT_NAME}}'

describe('{{COMPONENT_NAME}}', () => {
  it('renders without crashing', () => {
    render(<{{COMPONENT_NAME}} {{DEFAULT_PROPS}} />)
  })

  {{ADDITIONAL_TESTS}}
})
```

#### Examples as Reference

Include examples so Claude understands the expected output:

```markdown
## Examples

### Simple Component
See `examples/simple.tsx` for a basic button component.

### Complex Component
See `examples/complex.tsx` for a data table with sorting,
filtering, and pagination.

When generating, match the style and patterns shown in
these examples.
```

#### Config Files

Store defaults and settings:

**`config/defaults.json`**:
```json
{
  "cssMethodology": "modules",
  "testFramework": "react-testing-library",
  "stateManagement": "hooks",
  "defaultExport": false,
  "includeStorybook": true
}
```

Reference in instructions:
```markdown
Read `config/defaults.json` for project preferences.
Apply these settings unless the user specifies otherwise.
```

### Exercise

**Task**: Create a multi-file skill

Build a **API endpoint generator** skill:

1. **Create Directory**:
```bash
mkdir -p ~/.claude/skills/api-generator/{templates,examples}
```

2. **Create Template** (`templates/route.js`):
```javascript
const express = require('express')
const router = express.Router()

// GET /{{RESOURCE}}
router.get('/', async (req, res) => {
  // TODO: Implement list
})

// GET /{{RESOURCE}}/:id
router.get('/:id', async (req, res) => {
  // TODO: Implement get by ID
})

// POST /{{RESOURCE}}
router.post('/', async (req, res) => {
  // TODO: Implement create
})

module.exports = router
```

3. **Create SKILL.md** with instructions that reference the template

4. **Test**: `/api-generator users`

### Verification

Before moving on, ensure you can:
- [ ] Create a multi-file skill directory
- [ ] Write template files with placeholders
- [ ] Reference templates in skill instructions
- [ ] Include example files for guidance
- [ ] Use config files for defaults

---

## Lesson 38: Testing and Debugging Skills

### Objectives
- Test skills systematically
- Debug common skill issues
- Iterate on skill instructions
- Validate skill output quality

### Content

#### Testing Your Skill

**Manual Testing Checklist**:

1. **Basic Invocation**:
```bash
claude
/your-skill
# Does it load? Does Claude acknowledge the skill?
```

2. **With Arguments**:
```bash
/your-skill src/app.js
# Does it use the argument correctly?
```

3. **Edge Cases**:
```bash
/your-skill                    # No arguments
/your-skill nonexistent.js     # Invalid file
/your-skill src/ tests/        # Multiple arguments
```

4. **Output Quality**:
- Is the output formatted correctly?
- Does it follow the instructions?
- Is it useful and actionable?

#### Common Skill Issues

**Problem: Skill Not Found**:
```
/my-skill
→ "Unknown command: my-skill"
```
**Fix**: Check file location and name spelling:
```bash
ls ~/.claude/skills/
# Verify SKILL.md exists in the right place
```

**Problem: Claude Ignores Instructions**:
```
Skill instructions say "format as table"
but Claude outputs plain text
```
**Fix**: Make instructions more explicit:
```markdown
# Bad
Format the output nicely.

# Good
Format the output as a markdown table with these columns:
| File | Issue | Severity | Line |
```

**Problem: Wrong Tools Used**:
```
Review skill tries to edit files
```
**Fix**: Restrict tool access in frontmatter:
```yaml
tools:
  - Read
  - Glob
  - Grep
# No Write, Edit, or Bash
```

**Problem: Inconsistent Output**:
```
Sometimes outputs a list, sometimes a paragraph
```
**Fix**: Add an explicit output format section:
```markdown
## Output Format

Always output in this exact format:

### Summary
[1-2 sentence overview]

### Findings
1. **[Severity]**: [Issue description]
   - File: [path:line]
   - Fix: [suggested fix]

### Score
[X/10] with brief justification
```

#### Iterative Improvement

**The Testing Loop**:
```
1. Write skill → 2. Test it → 3. Note issues
                                    ↓
6. Test again ← 5. Refine ← 4. Identify fix
```

**Keep a Testing Log**:
```markdown
## Testing Notes

### v1 (2024-01-15)
- Issue: Misses security vulnerabilities in Python
- Fix: Added Python-specific security checks

### v2 (2024-01-16)
- Issue: Output too verbose for quick reviews
- Fix: Added summary section, moved details to expandable

### v3 (2024-01-17)
- Working well for JS/Python
- TODO: Add Go and Rust support
```

#### Skill Quality Checklist

Before sharing a skill, verify:
- [ ] Works with no arguments
- [ ] Works with valid arguments
- [ ] Handles invalid input gracefully
- [ ] Output is consistently formatted
- [ ] Instructions are clear and specific
- [ ] Tool restrictions are appropriate
- [ ] Model choice is optimal
- [ ] Examples are included

### Exercise

**Task**: Debug and improve a skill

**Start With This Intentionally Flawed Skill**:

Create `~/.claude/skills/bad-review/SKILL.md`:
```markdown
---
name: bad-review
description: review code
commands:
  - bad-review
tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Review

Check the code for problems. Tell the user what you found.
```

**Tasks**:

1. **Test the Flawed Skill**:
```bash
claude
/bad-review src/app.js
```

2. **Identify Issues**:
   - Too many tools (Write/Edit shouldn't be in a review)
   - Vague instructions
   - No output format specified
   - No specific checks defined

3. **Fix Each Issue**:
   - Restrict to read-only tools
   - Add specific review criteria
   - Define output format
   - Add examples

4. **Rename and Retest**:
   - Move to `~/.claude/skills/good-review/`
   - Test again with the same file
   - Compare output quality

### Verification

Before moving on, ensure you can:
- [ ] Systematically test skills
- [ ] Diagnose common skill problems
- [ ] Iteratively improve skill instructions
- [ ] Validate output quality and consistency
- [ ] Use a testing checklist

---

## Lesson 39: Sharing Skills with Your Team

### Objectives
- Share skills via project repository
- Set up team skill standards
- Manage skill versions
- Create skill documentation

### Content

#### Project-Level Skills

The most common way to share skills with a team:

```
your-project/
├── .claude/
│   └── skills/
│       ├── code-review/
│       │   └── SKILL.md
│       ├── deploy/
│       │   └── SKILL.md
│       └── component-gen/
│           ├── SKILL.md
│           └── templates/
├── src/
└── package.json
```

**Benefits**:
- Skills are version-controlled with the project
- Everyone on the team gets the same skills
- Skills evolve with the codebase
- New team members get skills automatically

#### Adding Skills to a Project

**Step 1: Create Skill Directory**:
```bash
mkdir -p .claude/skills/team-review
```

**Step 2: Write the Skill**:
```bash
# Create SKILL.md with your team's standards
```

**Step 3: Commit and Push**:
```bash
git add .claude/skills/
git commit -m "Add team code review skill"
git push
```

**Step 4: Team Uses It**:
```bash
# Every team member who pulls the repo gets the skill
claude
/team-review
```

#### Skill Documentation

Every shared skill should include:

```markdown
---
name: team-review
description: Code review following [Company] standards
commands:
  - team-review
  - tr
---

# Team Code Review

## Purpose
Ensures all code follows our team's quality standards before merge.

## Usage
\`\`\`
/team-review                    # Review staged changes
/team-review src/feature.js     # Review specific file
/team-review --security         # Security-focused review
\`\`\`

## What It Checks
1. **Security**: OWASP Top 10, auth patterns
2. **Performance**: N+1 queries, memory leaks
3. **Style**: ESLint compliance, naming conventions
4. **Testing**: Coverage thresholds, edge cases

## Output Format
Produces a structured report with:
- Summary score (1-10)
- Critical issues (must fix)
- Warnings (should fix)
- Suggestions (nice to have)

## Examples
[Link to example review output]

## Maintainer
@alice - ping in #engineering for issues
```

#### Version Management

**Semantic Versioning for Skills**:
```yaml
---
name: team-review
version: 2.1.0
# Major.Minor.Patch
# Major: Breaking changes to output/behavior
# Minor: New checks or features
# Patch: Bug fixes, wording improvements
---
```

**Changelog** (in SKILL.md or separate file):
```markdown
## Changelog

### v2.1.0
- Added Python type hint checking
- Improved output formatting

### v2.0.0
- Breaking: Changed output format to structured JSON
- Added severity scoring

### v1.0.0
- Initial release
```

#### Skill Standards for Teams

Create a **skill style guide** for your team:

```markdown
# Skill Development Guidelines

## Naming
- Use kebab-case: `code-review`, `deploy-staging`
- Be descriptive: `api-endpoint-generator` not `gen`

## Structure
- Always include Purpose section
- Always include Usage examples
- Always include Output Format
- Include at least one worked example

## Tool Access
- Review skills: Read-only (Read, Glob, Grep)
- Generation skills: Read + Write
- Workflow skills: Full access (with justification)

## Testing
- Test with at least 3 different inputs
- Test edge cases (no args, bad args)
- Document expected outputs
```

### Exercise

**Task**: Create a shareable team skill

1. **Create a Project Skill**:
```bash
cd ~/claude-practice
mkdir -p .claude/skills/pr-description
```

2. **Write SKILL.md** that generates PR descriptions following your team format

3. **Add Documentation** including usage, examples, and output format

4. **Test** with at least 3 different scenarios

5. **Commit** the skill to version control

### Verification

Before moving on, ensure you can:
- [ ] Create project-level skills (.claude/skills/)
- [ ] Write skill documentation
- [ ] Version your skills
- [ ] Share skills via git
- [ ] Establish team skill standards

---

## Lesson 40: Multi-Step Workflow Skills

### Objectives
- Build skills that perform multiple steps
- Implement conditional logic in instructions
- Create interactive skills
- Chain operations together

### Content

#### Multi-Step Skill Design

Complex skills perform a sequence of operations:

```markdown
# Deploy Skill

## Instructions

### Step 1: Pre-flight Checks
1. Read package.json for version
2. Run `npm test` — STOP if tests fail
3. Run `npm run lint` — STOP if lint fails
4. Check git status — STOP if uncommitted changes

### Step 2: Build
1. Run `npm run build`
2. Verify build output exists in dist/

### Step 3: Version Bump
1. Ask user: major, minor, or patch?
2. Update version in package.json
3. Create git tag

### Step 4: Deploy
1. Run deployment command
2. Verify deployment succeeded
3. Report results
```

#### Conditional Logic

Use clear if/then language:

```markdown
## Instructions

### Step 1: Detect Environment
Read the project files to determine:
- If `package.json` exists → Node.js project
- If `requirements.txt` exists → Python project
- If `Cargo.toml` exists → Rust project
- If none match → Ask the user

### Step 2: Run Appropriate Checks
**For Node.js**:
1. Run `npm test`
2. Run `npm run lint`

**For Python**:
1. Run `pytest`
2. Run `flake8`

**For Rust**:
1. Run `cargo test`
2. Run `cargo clippy`
```

#### Stop Conditions

Tell Claude when to stop:

```markdown
## Critical Rules

**STOP and report** if any of these occur:
- Tests fail → Report which tests failed
- Lint errors > 10 → List the top 10 errors
- Security vulnerabilities found → List all critical ones
- Build fails → Show the error output

**Do NOT continue** to the next step until the current step passes.
```

#### Interactive Steps

Skills can ask the user for input:

```markdown
### Step 3: User Decision Point

Present the user with these options:
1. **Deploy to staging** — Safer, test first
2. **Deploy to production** — Direct deploy
3. **Cancel** — Abort deployment

Wait for user response before continuing.
```

#### Example: Full Workflow Skill

```markdown
---
name: feature-complete
description: Complete feature workflow (test, review, commit, PR)
commands:
  - feature-complete
  - fc
model: sonnet
tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
---

# Feature Complete Workflow

Runs the full workflow to finalize a feature.

## Instructions

### Step 1: Verify Changes
1. Run `git status` to see all changes
2. Run `git diff` to review changes
3. Summarize what was changed and why

### Step 2: Run Tests
1. Run the project's test suite
2. If tests fail:
   - Show failing tests
   - Attempt to fix obvious issues
   - Re-run tests
   - If still failing, STOP and report

### Step 3: Code Review
1. Read all changed files
2. Check for:
   - Security issues
   - Missing error handling
   - Code style violations
3. Fix any issues found
4. Re-run tests after fixes

### Step 4: Create Commit
1. Stage all relevant files (exclude .env, node_modules)
2. Write a descriptive commit message
3. Create the commit

### Step 5: Create PR (if requested)
Ask user: "Create a pull request? (y/n)"
If yes:
1. Push branch to remote
2. Create PR with summary of all changes
3. Report PR URL

## Output
At each step, report:
- What was done
- Any issues found and fixed
- Current status (pass/fail)
```

### Exercise

**Task**: Build a multi-step deployment skill

Create `~/.claude/skills/safe-deploy/SKILL.md` that:

1. **Pre-flight**: Checks tests, lint, git status
2. **Build**: Compiles the project
3. **Validate**: Verifies build output
4. **Confirm**: Asks user to proceed
5. **Deploy**: Runs deployment command
6. **Verify**: Checks deployment health
7. **Report**: Summarizes what happened

Test with a simple Node.js project.

### Verification

Before moving on, ensure you can:
- [ ] Design multi-step skill workflows
- [ ] Implement conditional logic
- [ ] Add stop conditions for safety
- [ ] Create interactive decision points
- [ ] Chain operations together

---

## Lesson 41: Skill Composition and Reuse

### Objectives
- Compose skills from smaller skills
- Create skill libraries
- Reuse common patterns
- Build a personal skill collection

### Content

#### Composing Skills

Large workflows can reference other skills:

```markdown
# Release Workflow

## Instructions

### Step 1: Quality Gate
Perform the checks from the `/code-review` skill on all
changed files. If any critical issues are found, STOP.

### Step 2: Test Suite
Run the full test suite. Apply the same standards as
the `/test-coverage` skill (minimum 80% coverage).

### Step 3: Create Release
Follow the commit standards from our project's CLAUDE.md.
```

#### Building a Skill Library

Organize your skills by category:

```
~/.claude/skills/
├── review/
│   ├── code-review/SKILL.md
│   ├── security-review/SKILL.md
│   └── perf-review/SKILL.md
├── generate/
│   ├── component/SKILL.md
│   ├── api-endpoint/SKILL.md
│   └── test-suite/SKILL.md
├── workflow/
│   ├── feature-complete/SKILL.md
│   ├── deploy/SKILL.md
│   └── release/SKILL.md
└── utility/
    ├── format-check/SKILL.md
    ├── dependency-audit/SKILL.md
    └── cleanup/SKILL.md
```

#### Common Skill Patterns

**The Analyzer Pattern**:
```markdown
1. Discover files (Glob)
2. Read and analyze (Read, Grep)
3. Generate report (structured output)
```
Used for: Code reviews, audits, documentation checks

**The Generator Pattern**:
```markdown
1. Gather requirements (from args or asking)
2. Read existing patterns (Read)
3. Generate code (Write)
4. Validate (Bash: run tests)
```
Used for: Component generation, API scaffolding, test creation

**The Workflow Pattern**:
```markdown
1. Pre-checks (validate state)
2. Execute steps (sequential operations)
3. Verify (run tests/checks)
4. Report (summarize results)
```
Used for: Deployments, releases, migrations

#### Reusable Instruction Blocks

Create common instruction snippets:

**Error Handling Block**:
```markdown
## Error Handling
If any step fails:
1. Log the error with context
2. Attempt recovery if possible
3. If recovery fails, STOP and report:
   - Which step failed
   - The error message
   - Suggested manual fix
```

**Output Format Block**:
```markdown
## Output Format
Present results as:

### Summary
- Total items checked: [N]
- Issues found: [N]
- Auto-fixed: [N]

### Details
| # | Severity | File | Issue | Status |
|---|----------|------|-------|--------|
| 1 | Critical | path | desc  | Fixed  |
```

### Exercise

**Task**: Build a personal skill library

Create at least 3 skills from different patterns:

1. **Analyzer Skill**: Code complexity checker
   - Finds functions over 50 lines
   - Reports cyclomatic complexity
   - Suggests refactoring

2. **Generator Skill**: Test file creator
   - Reads a source file
   - Generates comprehensive test file
   - Includes edge cases

3. **Workflow Skill**: Morning standup prep
   - Shows yesterday's commits
   - Lists open PRs
   - Shows current branch status
   - Formats for standup report

### Verification

Before moving on, ensure you can:
- [ ] Compose skills from other skills
- [ ] Organize a skill library
- [ ] Recognize common skill patterns
- [ ] Create reusable instruction blocks
- [ ] Build skills across multiple categories

---

## Lesson 42: Skill Performance and Best Practices

### Objectives
- Optimize skill performance
- Follow best practices for production skills
- Handle edge cases gracefully
- Create maintainable skill instructions

### Content

#### Performance Optimization

**Minimize Tool Calls**:
```markdown
# Bad: Many small reads
1. Read file1.js
2. Read file2.js
3. Read file3.js
4. Read file4.js

# Good: Parallel reads
1. Read all files in src/ in parallel:
   - file1.js, file2.js, file3.js, file4.js
```

**Use Targeted Searches**:
```markdown
# Bad: Read everything
1. Read every file in the project

# Good: Search then read
1. Grep for "TODO" in src/
2. Read only the files with matches
```

**Choose the Right Model**:
```markdown
# Use Haiku for simple, fast tasks
model: haiku  # Formatting, simple checks

# Use Sonnet for balanced tasks
model: sonnet  # Reviews, generation

# Use Opus only when necessary
model: opus  # Complex architecture analysis
```

#### Best Practices

**1. Clear, Specific Instructions**:
```markdown
# Bad
Review the code and find problems.

# Good
Review the code for these specific issues:
1. SQL injection: Look for string concatenation in queries
2. XSS: Look for innerHTML or dangerouslySetInnerHTML
3. Auth bypass: Look for missing authentication checks
4. Secrets: Look for hardcoded passwords or API keys

For each issue found, provide:
- Severity (Critical/High/Medium/Low)
- Exact file and line number
- Code snippet showing the problem
- Recommended fix with code example
```

**2. Graceful Error Handling**:
```markdown
## Error Handling

If a file cannot be read:
→ Skip it and note in the report

If no files match the search:
→ Report "No matching files found" and suggest
  checking the path

If tests fail to run:
→ Report the error and suggest checking dependencies
```

**3. Consistent Output**:
```markdown
## Output Format

ALWAYS use this exact format:

## [Skill Name] Report

**Date**: [current date]
**Scope**: [files analyzed]

### Summary
[1-2 sentences]

### Findings
[Numbered list]

### Recommendations
[Prioritized action items]
```

**4. Version and Document**:
```yaml
---
name: my-skill
version: 1.2.0
description: Clear description of what this skill does
author: your-name
---
```

#### Edge Case Handling

**No Input**:
```markdown
If no file or directory is specified:
→ Use the current working directory
→ Analyze all relevant files (*.js, *.ts, *.py)
```

**Large Projects**:
```markdown
If more than 50 files match:
→ Focus on files modified in the last 30 days
→ Prioritize files in src/ over tests/
→ Report that a subset was analyzed
```

**Empty Results**:
```markdown
If no issues are found:
→ Report "No issues found"
→ Confirm what was checked
→ Suggest areas for manual review
```

#### Skill Maintenance

**When to Update a Skill**:
- Team standards change
- New patterns to check for
- Users report false positives/negatives
- Framework versions update
- New tools become available

**Deprecation**:
```markdown
---
name: old-review
deprecated: true
replacement: new-review
---

⚠️ This skill is deprecated. Use `/new-review` instead.
```

### Exercise

**Task**: Optimize and polish a skill

Take any skill you've created and:

1. **Benchmark**: How many tool calls does it make? Can you reduce them?
2. **Edge Cases**: Test with empty files, missing files, large files
3. **Output Quality**: Is the output consistently formatted?
4. **Documentation**: Is usage clear? Are examples included?
5. **Error Handling**: What happens when things go wrong?

**Optimization Checklist**:
- [ ] Parallel reads where possible
- [ ] Targeted search before broad read
- [ ] Appropriate model selection
- [ ] Clear error messages
- [ ] Consistent output format
- [ ] Version number and changelog
- [ ] Usage documentation

### Verification

Before moving on, ensure you can:
- [ ] Optimize skill performance
- [ ] Handle edge cases gracefully
- [ ] Write production-quality skill instructions
- [ ] Maintain and version skills
- [ ] Follow the skill best practices checklist

---

## Section 7 Summary

You've mastered the Skills system:

| Lesson | Topic | Key Takeaway |
|--------|-------|-------------|
| 33 | What Are Skills? | Reusable expertise packages |
| 34 | Using Existing Skills | /command syntax and auto-invocation |
| 35 | Creating Your First Skill | SKILL.md structure and instructions |
| 36 | Advanced Configuration | Frontmatter, tools, models, triggers |
| 37 | Templates & Supporting Files | Multi-file skills with templates |
| 38 | Testing & Debugging | Systematic testing and iteration |
| 39 | Sharing Skills | Project-level skills and team standards |
| 40 | Multi-Step Workflows | Complex sequential operations |
| 41 | Composition & Reuse | Skill libraries and patterns |
| 42 | Performance & Best Practices | Production-quality skills |

### What's Next?

In **Section 8: Hooks System**, you'll learn how to automate actions with hooks — commands that run automatically in response to events like tool calls, file saves, and commits.