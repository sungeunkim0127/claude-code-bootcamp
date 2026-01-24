# Skill Template

Use this template to create custom skills for Claude Code.

## Basic Skill Structure

```markdown
---
name: skill-name
description: Brief description of what this skill does
commands:
  - command-name
  - another-command
triggers:
  - pattern: "trigger phrase"
    command: command-name
model: sonnet  # or opus, haiku
autoInvoke: false
tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Skill Name

## Purpose
[Explain what this skill does and when to use it]

## Commands

### command-name [$ARGUMENTS]
[Description of what this command does]

## Instructions

[Detailed instructions for Claude when executing this skill]

### Step 1: [First Step]
[Instructions]

### Step 2: [Second Step]
[Instructions]

## Examples

### Example 1: [Use Case]
```
Input: /command-name arg1 arg2
Expected behavior: [description]
```

## Supporting Files

[If this skill uses templates or reference files, mention them here]

## Notes

[Any additional context, tips, or warnings]
```

---

## Example Skills

### Example 1: Code Review Skill

```markdown
---
name: code-review
description: Performs comprehensive code review
commands:
  - review
  - review-pr
triggers:
  - pattern: "review (this|the) (code|PR|pull request)"
    command: review
model: sonnet
autoInvoke: false
tools:
  - Read
  - Glob
  - Grep
  - Bash(git*)
---

# Code Review Skill

## Purpose
Performs systematic code review checking for:
- Code quality and maintainability
- Security vulnerabilities
- Performance issues
- Best practices adherence
- Test coverage
- Documentation completeness

## Commands

### review [$ARGUMENTS]
Reviews code in current directory or specified files.
Arguments: Optional file patterns or PR number

### review-pr [$ARGUMENTS]
Reviews a specific pull request.
Arguments: PR number

## Instructions

### Step 1: Determine Scope
- If PR number provided: Use `gh pr diff $ARGUMENTS`
- If file patterns provided: Use Glob with patterns
- If no arguments: Review all modified files (`git diff --name-only`)

### Step 2: Read Files
Read each file in scope, focusing on:
- Recently changed functions
- Security-sensitive code (auth, validation, database queries)
- Complex logic

### Step 3: Check Security
Look for:
- SQL injection vulnerabilities (string concatenation in queries)
- XSS vulnerabilities (innerHTML, dangerouslySetInnerHTML)
- Authentication/authorization bypasses
- Sensitive data exposure
- Dependency vulnerabilities

### Step 4: Check Quality
Analyze:
- Code complexity (functions >50 lines, deep nesting)
- Code duplication
- Naming clarity
- Error handling completeness
- Edge case coverage

### Step 5: Check Performance
Identify:
- N+1 query problems
- Inefficient algorithms
- Missing indexes
- Unnecessary computations
- Memory leaks

### Step 6: Check Tests
Verify:
- Tests exist for new code
- Tests cover edge cases
- Tests are maintainable
- Test quality (not just coverage)

### Step 7: Generate Report
Create structured report:

```markdown
# Code Review Report

## Summary
[Brief overview]

## Critical Issues
[Issues that must be fixed before merge]

## High Priority
[Important issues that should be addressed]

## Medium Priority
[Issues to consider]

## Low Priority / Suggestions
[Nice-to-have improvements]

## Positive Notes
[What was done well]

## Recommendations
[Specific action items]
```

## Examples

### Example 1: Review Current Changes
```
Input: /review
Expected: Reviews all git-modified files
```

### Example 2: Review PR
```
Input: /review-pr 123
Expected: Reviews PR #123 with comprehensive analysis
```

### Example 3: Review Specific Files
```
Input: /review src/auth/*.js
Expected: Reviews all auth files
```
```

---

### Example 2: Refactoring Skill

```markdown
---
name: refactor
description: Safely refactors code with tests
commands:
  - refactor
  - extract-function
  - rename
model: sonnet
tools:
  - Read
  - Edit
  - Bash(npm test)
  - Bash(pytest)
---

# Refactoring Skill

## Purpose
Performs safe refactoring with test verification at each step.

## Commands

### refactor [$ARGUMENTS]
General refactoring command.
Arguments: Description of refactoring goal

### extract-function [$ARGUMENTS]
Extracts code into a separate function.
Arguments: File and function name

### rename [$ARGUMENTS]
Renames variable, function, or class.
Arguments: old-name new-name

## Instructions

### Step 1: Read Current Code
Read the files to be refactored.

### Step 2: Run Tests (Baseline)
```bash
# Determine test command based on project
npm test || pytest || cargo test || go test
```

Ensure all tests pass before refactoring.

### Step 3: Perform Refactoring
Make the changes incrementally.

### Step 4: Run Tests (Verification)
Run tests after each logical change.
If tests fail: Revert and try different approach.

### Step 5: Final Verification
- Run full test suite
- Check for unintended changes
- Verify functionality unchanged

### Step 6: Report
Summarize:
- What was refactored
- Why it's better
- Test results
- Any remaining opportunities

## Example

### Example 1: Extract Function
```
Input: /extract-function src/utils.js calculateTotal
Expected:
1. Reads utils.js
2. Identifies code to extract
3. Creates new function
4. Replaces inline code with function call
5. Runs tests
6. Reports success
```
```

---

### Example 3: Documentation Skill

```markdown
---
name: document
description: Generates comprehensive documentation
commands:
  - doc
  - doc-api
  - doc-readme
model: sonnet
tools:
  - Read
  - Write
  - Glob
  - Grep
---

# Documentation Skill

## Purpose
Generates and updates project documentation.

## Commands

### doc [$ARGUMENTS]
Generate documentation for specific files or patterns.

### doc-api
Generate API documentation from code.

### doc-readme
Create or update README.md.

## Instructions

### For API Documentation

1. **Find API Endpoints**:
```bash
grep -r "app\.(get|post|put|delete)" src/
```

2. **Read Route Files**:
Read each file containing routes.

3. **Extract API Info**:
For each endpoint:
- Method (GET, POST, etc.)
- Path
- Parameters
- Request body
- Response format
- Error codes
- Authentication requirements

4. **Generate OpenAPI Spec**:
Create OpenAPI 3.0 specification.

5. **Generate Markdown Docs**:
Create human-readable API.md.

### For README

1. **Analyze Project**:
- Read package.json / Cargo.toml / requirements.txt
- Identify tech stack
- Find entry points

2. **Generate Sections**:
- Project title and description
- Features
- Installation
- Usage examples
- API reference (link)
- Configuration
- Contributing
- License

3. **Write README.md**:
Create comprehensive, well-formatted README.

## Examples

### Example 1: Generate API Docs
```
Input: /doc-api
Expected: Creates API.md with all endpoints documented
```

### Example 2: Update README
```
Input: /doc-readme
Expected: Creates/updates README.md with current project state
```
```

---

## Skill Development Tips

### 1. Start Simple
Begin with basic functionality, add complexity incrementally.

### 2. Test Thoroughly
Test your skill on multiple projects before sharing.

### 3. Handle Errors
Include error handling for common failure cases.

### 4. Be Specific
Give Claude specific, actionable instructions.

### 5. Use Examples
Include examples to guide Claude's behavior.

### 6. Document Well
Clear documentation helps users and Claude.

### 7. Version Control
Track changes to your skills.

### 8. Share & Iterate
Get feedback, improve based on usage.

---

## Advanced Patterns

### Dynamic Context Injection

```markdown
---
name: dynamic-example
---

Current branch: `$(git branch --show-current)`
Recent commits:
```
$(git log -5 --oneline)
```

Use this context when making decisions.
```

### Conditional Logic

```markdown
If the project uses TypeScript (check for tsconfig.json):
- Use TypeScript syntax
- Check types with tsc --noEmit

If the project uses Python (check for requirements.txt):
- Use Python syntax
- Check with flake8

If the project uses Rust (check for Cargo.toml):
- Use Rust syntax
- Check with cargo check
```

### Multi-Step Workflows

```markdown
1. **Discovery Phase** (read-only):
   - Glob for relevant files
   - Grep for patterns
   - Read key files

2. **Analysis Phase**:
   - Identify issues
   - Determine approach
   - Present plan to user

3. **Execution Phase** (wait for approval):
   - Make changes
   - Run tests after each change
   - Verify results

4. **Reporting Phase**:
   - Summarize changes
   - Show metrics
   - Suggest next steps
```

---

## Skill Library Ideas

Create skills for:
- [ ] Automated testing
- [ ] Database migrations
- [ ] Deployment automation
- [ ] Security scanning
- [ ] Performance profiling
- [ ] Dependency updates
- [ ] Code formatting
- [ ] Changelog generation
- [ ] Release preparation
- [ ] Bug triage
- [ ] Onboarding new developers
- [ ] Architecture documentation
- [ ] API client generation
- [ ] Database seeding
- [ ] Environment setup

---

## Testing Your Skill

1. **Create Test Project**:
```bash
mkdir ~/skill-test
cd ~/skill-test
# Set up test scenario
```

2. **Invoke Skill**:
```bash
claude
/your-skill-name test-args
```

3. **Verify Results**:
- Check output matches expectations
- Verify files are correct
- Ensure no unintended changes

4. **Iterate**:
- Refine instructions
- Add error handling
- Improve clarity

---

## Publishing Your Skill

1. **Package**:
```
my-skill/
├── SKILL.md
├── README.md
├── examples/
└── templates/
```

2. **Document**:
- Clear README
- Usage examples
- Screenshots/demos

3. **Share**:
- GitHub repository
- Claude Code community
- Blog post / tutorial

---

**Start building your skills library today!**
