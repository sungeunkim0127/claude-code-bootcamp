---
name: code-review
description: Performs comprehensive code review with security, performance, and quality checks
commands:
  - review
  - review-pr
triggers:
  - pattern: "review (this|the) (code|changes|PR)"
    command: review
model: sonnet
autoInvoke: false
tools:
  - Read
  - Glob
  - Grep
  - Bash(git*)
  - Bash(gh pr*)
---

# Code Review Skill

Performs systematic, comprehensive code reviews checking for code quality, security vulnerabilities, performance issues, and best practices adherence.

## Commands

### review [$ARGUMENTS]
Reviews code in current directory or specified files/patterns.

**Arguments**:
- File patterns (e.g., `src/**/*.js`)
- PR number (e.g., `123`)
- Empty (reviews all changed files)

### review-pr [$ARGUMENTS]
Reviews a specific pull request.

**Arguments**: PR number (required)

---

## Review Process

### Step 1: Determine Scope

**If PR number provided**:
```bash
gh pr view $ARGUMENTS
gh pr diff $ARGUMENTS
```

**If file patterns provided**:
Use Glob with the patterns: `$ARGUMENTS`

**If no arguments**:
```bash
git diff --name-only HEAD
```
Review all modified files.

### Step 2: Read Files

For each file in scope:
1. Read the entire file
2. Focus on:
   - Recently changed sections
   - Security-sensitive code (auth, validation, database queries)
   - Complex logic
   - Public interfaces/APIs

### Step 3: Security Analysis

Check for common vulnerabilities:

**SQL Injection**:
```javascript
// BAD - String concatenation
const query = "SELECT * FROM users WHERE id = '" + userId + "'"

// GOOD - Parameterized query
const query = "SELECT * FROM users WHERE id = ?"
```

**XSS (Cross-Site Scripting)**:
```javascript
// BAD - innerHTML
element.innerHTML = userInput

// GOOD - textContent
element.textContent = userInput
```

**Authentication/Authorization**:
- Check for proper auth middleware
- Verify permission checks
- Look for auth bypass potential

**Sensitive Data Exposure**:
```javascript
// BAD - Logging passwords
console.log('User login:', { email, password })

// GOOD - Redact sensitive data
console.log('User login:', { email })
```

**Dependency Vulnerabilities**:
```bash
npm audit
# or
yarn audit
```

### Step 4: Code Quality Analysis

**Function Complexity**:
- Functions >50 lines → Flag for refactoring
- Nesting depth >3 → Suggest simplification
- Cyclomatic complexity >10 → Recommend breaking down

**Code Duplication**:
- Identical blocks >3 lines → Suggest extraction
- Similar patterns → Recommend abstraction

**Naming Clarity**:
```javascript
// BAD
function p(d) { ... }

// GOOD
function processData(data) { ... }
```

**Error Handling**:
```javascript
// BAD - Silent failures
try {
  await riskyOperation()
} catch (e) {}

// GOOD - Proper error handling
try {
  await riskyOperation()
} catch (error) {
  logger.error('Operation failed', { error })
  throw new ServiceError('Failed to process', { cause: error })
}
```

### Step 5: Performance Analysis

**Database Queries**:
- N+1 query problems
- Missing indexes
- SELECT * instead of specific fields
- Unnecessary queries in loops

**Algorithm Efficiency**:
```javascript
// BAD - O(n²)
for (let i = 0; i < array.length; i++) {
  for (let j = 0; j < array.length; j++) {
    if (array[i] === array[j]) { ... }
  }
}

// GOOD - O(n) with Set
const seen = new Set()
for (const item of array) {
  if (seen.has(item)) { ... }
  seen.add(item)
}
```

**Resource Leaks**:
- Unclosed connections
- Event listeners not removed
- Timers not cleared
- File handles not closed

**Unnecessary Computations**:
- Results that could be cached
- Repeated calculations in loops
- Heavy operations that could be deferred

### Step 6: Best Practices

**Consistency**:
- Follows project style guide
- Matches existing patterns
- Uses project conventions

**Documentation**:
```javascript
// GOOD - Clear JSDoc
/**
 * Validates user email address
 * @param {string} email - Email to validate
 * @returns {boolean} True if valid
 * @throws {ValidationError} If email format is invalid
 */
function validateEmail(email) { ... }
```

**Testing**:
- New code has tests
- Tests cover edge cases
- Tests are maintainable
- No test pollution

**Dependencies**:
- Necessary (not bloat)
- Actively maintained
- No known vulnerabilities
- License compatible

### Step 7: Generate Report

Create a structured markdown report:

```markdown
# Code Review Report

**Date**: [Current date]
**Reviewer**: Claude Code Review Skill
**Scope**: [Files/PR reviewed]

---

## 📊 Summary

- **Files Reviewed**: X
- **Issues Found**: Y
- **Critical**: Z
- **High Priority**: A
- **Medium Priority**: B
- **Suggestions**: C

---

## 🚨 Critical Issues

Issues that **must** be fixed before merge:

### 1. SQL Injection Vulnerability in `src/api/users.js:45`

**Severity**: Critical
**Type**: Security

**Issue**:
```javascript
const query = `SELECT * FROM users WHERE email = '${req.body.email}'`
```

**Problem**: User input directly concatenated into SQL query allows SQL injection attacks.

**Fix**:
```javascript
const query = 'SELECT * FROM users WHERE email = ?'
db.query(query, [req.body.email])
```

**Impact**: Could allow attackers to access/modify database.

---

## ⚠️ High Priority

Important issues that should be addressed:

### 1. Missing Error Handling in `src/services/payment.js:67`

**Severity**: High
**Type**: Reliability

**Issue**: Async function has no error handling...

[Continue for each issue]

---

## 📝 Medium Priority

Issues to consider:

### 1. Function Complexity in `src/utils/parser.js:123`

**Severity**: Medium
**Type**: Maintainability

**Issue**: Function is 75 lines with nesting depth of 5...

---

## 💡 Suggestions

Nice-to-have improvements:

- Extract common validation logic into utils
- Add JSDoc comments to public functions
- Consider using TypeScript for better type safety
- Update outdated dependencies

---

## ✅ Positive Notes

What was done well:

- Excellent test coverage (94%)
- Clear variable naming
- Good separation of concerns
- Well-structured error handling in most places

---

## 🎯 Recommendations

1. **Immediate** (before merge):
   - Fix SQL injection vulnerability
   - Add missing error handling
   - Address authentication bypass

2. **Short-term** (next sprint):
   - Refactor complex functions
   - Improve test coverage for edge cases
   - Update documentation

3. **Long-term** (technical debt):
   - Consider architectural refactoring for service layer
   - Evaluate migration to TypeScript
   - Implement comprehensive logging

---

## 📋 Checklist

Before merging:
- [ ] All critical issues resolved
- [ ] High priority issues addressed or justified
- [ ] Tests passing
- [ ] Security scan clean
- [ ] Performance acceptable

---

**Overall Assessment**: [APPROVE / REQUEST CHANGES / COMMENT]

**Reason**: [Brief explanation of recommendation]
```

---

## Examples

### Example 1: Review Current Changes

```
User: /review

Claude:
  [Runs git diff to find changed files]
  [Reads each changed file]
  [Performs analysis]
  [Generates report]

  "I've reviewed 5 changed files. Found 2 critical issues,
   3 high priority items, and 5 suggestions.

   See full report above."
```

### Example 2: Review Specific Files

```
User: /review src/api/**/*.js

Claude:
  [Globs for src/api/**/*.js]
  [Reads matching files]
  [Performs analysis focused on API security]
  [Generates report]
```

### Example 3: Review Pull Request

```
User: /review-pr 123

Claude:
  [Fetches PR #123 details]
  [Gets diff using gh pr diff]
  [Reads changed files]
  [Performs analysis]
  [Generates report]
  [Optionally posts comment to PR]
```

---

## Configuration

### Customize Severity Thresholds

Edit this skill to adjust what counts as critical/high/medium:

**Critical**:
- Security vulnerabilities
- Data loss potential
- Authentication/authorization bypasses

**High**:
- Missing error handling in critical paths
- Performance issues (>2x slower)
- Breaking changes to public API

**Medium**:
- Code complexity (>50 lines, depth >3)
- Missing tests
- Documentation gaps

**Low/Suggestions**:
- Style inconsistencies
- Minor optimizations
- Refactoring opportunities

### Project-Specific Rules

Add your own checks:

```javascript
// Example: Check for banned functions
if (code.includes('eval(')) {
  issues.push({
    severity: 'critical',
    type: 'security',
    message: 'eval() is banned in this project'
  })
}
```

---

## Tips

### For Best Results

1. **Review Small Changes**: <500 lines is ideal
2. **Provide Context**: Mention what type of review (security-focused, performance, etc.)
3. **Be Specific**: Point to specific files/areas if known
4. **Iterate**: Address critical issues first, then re-review

### Common Use Cases

**Pre-Commit Review**:
```bash
git add .
claude
> /review
```

**PR Review**:
```bash
claude
> /review-pr 123
```

**Security Audit**:
```bash
claude
> /review src/ --focus security
```

**Performance Check**:
```bash
claude
> /review src/services/ --focus performance
```

---

## Integration

### GitHub Actions

```yaml
name: Claude Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Code Review
        run: |
          claude --non-interactive /review-pr ${{ github.event.pull_request.number }}
```

### Pre-commit Hook

```bash
#!/bin/bash
# .git/hooks/pre-commit

claude --non-interactive /review
```

---

**Use this skill to maintain high code quality and catch issues early!**
