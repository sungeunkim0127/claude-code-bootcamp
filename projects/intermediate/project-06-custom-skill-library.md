# Project 6: Custom Skill Library

**Level**: Intermediate
**Time**: 4-6 hours
**Prerequisites**: Lessons 33-42 (Skills System)

---

## 🎯 Project Goals

Build a library of reusable skills to:
- Automate common development tasks
- Enforce code standards
- Streamline workflows
- Share knowledge with your team

---

## 📋 Requirements

### Core Features
- [ ] At least 5 custom skills
- [ ] Skills cover different use cases
- [ ] Well-documented with examples
- [ ] Tested on real code
- [ ] Shareable with team

### Skills to Build
1. **Code Review Skill** - Automated code review
2. **Test Generator Skill** - Generate test suites
3. **Refactoring Skill** - Safe code refactoring
4. **Documentation Skill** - Generate docs
5. **Deployment Skill** - Deployment checklist

---

## 🏗️ Project Structure

```
~/.claude/skills/
├── code-review/
│   ├── SKILL.md
│   ├── templates/
│   │   └── review-report.md
│   └── examples/
│       └── example-review.md
├── test-generator/
│   ├── SKILL.md
│   └── templates/
│       ├── jest-template.js
│       └── mocha-template.js
├── refactor/
│   ├── SKILL.md
│   └── patterns/
│       ├── extract-function.md
│       └── rename-variable.md
├── document/
│   ├── SKILL.md
│   └── templates/
│       ├── jsdoc-template.md
│       └── api-doc-template.md
└── deploy/
    ├── SKILL.md
    └── checklists/
        ├── staging-checklist.md
        └── production-checklist.md
```

---

## 🚀 Skill 1: Code Review Skill

### Objectives
Create a skill that performs comprehensive code reviews.

### Ask Claude

```
"Create a code-review skill that:

Structure:
- Location: ~/.claude/skills/code-review/SKILL.md
- Commands: review, review-pr
- Tools: Read, Glob, Grep, Bash(git*), Bash(gh pr*)

Review Process:
1. Determine scope (PR number, files, or git changes)
2. Read files in scope
3. Security analysis:
   - SQL injection
   - XSS vulnerabilities
   - Auth/authorization issues
   - Sensitive data exposure
4. Code quality:
   - Function complexity (flag >50 lines)
   - Code duplication
   - Naming clarity
   - Error handling
5. Performance:
   - N+1 queries
   - Inefficient algorithms
   - Missing indexes
6. Best practices:
   - Consistency with project style
   - Documentation
   - Testing
7. Generate comprehensive report with:
   - Critical issues (must fix)
   - High priority
   - Medium priority
   - Suggestions
   - Positive notes

Output Format:
Markdown report with:
- Summary
- Issues by severity
- Specific locations (file:line)
- Fix recommendations
- Overall assessment

Include examples of good and bad code in the skill."
```

### Enhancements

```
"Add to the code-review skill:

1. Security focus mode:
   /review --security
   Deep dive on security only

2. Performance focus mode:
   /review --performance
   Focus on performance issues

3. Quick review mode:
   /review --quick
   High-level overview only

4. PR comment generation:
   Generate formatted comments for GitHub PR

5. Team-specific rules:
   Check project's .reviewrc file for custom rules"
```

---

## 🚀 Skill 2: Test Generator Skill

### Objectives
Create a skill that generates comprehensive test suites.

### Ask Claude

```
"Create a test-generator skill that:

Structure:
- Location: ~/.claude/skills/test-generator/SKILL.md
- Commands: gen-tests, test
- Tools: Read, Write, Bash(npm test)

Process:
1. Read the source file to test
2. Identify:
   - Public functions/methods
   - Edge cases
   - Error conditions
   - Dependencies to mock
3. Detect testing framework:
   - Jest (package.json)
   - Mocha
   - Vitest
   - Default to Jest if unsure
4. Generate tests with:
   - Descriptive test names
   - Arrange-Act-Assert structure
   - Edge case coverage
   - Error case coverage
   - Mock setup for dependencies
5. Follow project patterns:
   - Check existing tests for style
   - Match naming conventions
   - Use same assertion library
6. Aim for >90% coverage

Test Structure:
describe('ModuleName', () => {
  describe('functionName', () => {
    it('should [expected behavior] when [condition]', () => {
      // Arrange
      const input = ...

      // Act
      const result = functionName(input)

      // Assert
      expect(result).toBe(...)
    })

    it('should throw [error] when [invalid condition]', () => {
      expect(() => functionName(invalid))
        .toThrow(ErrorType)
    })
  })
})

Include templates for:
- Jest tests
- Mocha tests
- Integration tests
- Async function tests
- Mock examples"
```

### Enhancements

```
"Add to test-generator skill:

1. Coverage-driven mode:
   Analyze current coverage, generate tests for uncovered code

2. Edge case finder:
   Specifically look for and test edge cases

3. Integration test generator:
   Generate integration tests for APIs

4. Visual regression tests:
   Generate visual tests for UI components

5. Performance tests:
   Generate benchmark tests for critical functions"
```

---

## 🚀 Skill 3: Refactoring Skill

### Objectives
Create a skill that safely refactors code.

### Ask Claude

```
"Create a refactor skill that:

Structure:
- Location: ~/.claude/skills/refactor/SKILL.md
- Commands: refactor, extract-function, rename, simplify
- Tools: Read, Edit, Bash(npm test)

Safety Protocol:
1. ALWAYS run tests before starting
2. Make incremental changes
3. Run tests after each change
4. If tests fail, revert and try different approach
5. Generate final verification report

Refactoring Patterns:

extract-function:
1. Identify code block to extract
2. Determine parameters needed
3. Determine return value
4. Create new function
5. Replace inline code with function call
6. Run tests

rename:
1. Find all occurrences (use Edit with replace_all)
2. Update references
3. Update documentation
4. Run tests

simplify:
1. Identify complex function (>50 lines or complexity >10)
2. Break into smaller functions
3. Reduce nesting (early returns, guard clauses)
4. Extract conditionals into named functions
5. Run tests after each sub-step

remove-duplication:
1. Find duplicated code (>5 similar lines)
2. Extract to shared function
3. Update all call sites
4. Run tests

Common Refactorings:
- Extract function
- Extract variable
- Inline function
- Rename variable/function
- Replace conditional with polymorphism
- Replace magic numbers with constants

Always:
- Preserve behavior (tests must pass)
- Improve readability
- Reduce complexity
- Maintain or improve performance"
```

---

## 🚀 Skill 4: Documentation Skill

### Objectives
Create a skill that generates comprehensive documentation.

### Ask Claude

```
"Create a document skill that:

Structure:
- Location: ~/.claude/skills/document/SKILL.md
- Commands: doc, doc-function, doc-api, doc-readme
- Tools: Read, Write, Glob, Grep

Commands:

doc-function [file] [function]:
Generate JSDoc/docstring for a function
1. Read file and locate function
2. Analyze:
   - Parameters and types
   - Return value
   - Side effects
   - Exceptions
   - Complexity
3. Generate documentation:
   JavaScript/TypeScript: JSDoc
   Python: Google-style docstring
   Rust: rustdoc
   Go: godoc
4. Include:
   - Description
   - Parameters
   - Return value
   - Exceptions
   - Example usage
5. Use Edit to add documentation

doc-api:
Generate API documentation
1. Find all routes (Grep for app.get, app.post, etc.)
2. For each endpoint:
   - Method
   - Path
   - Parameters
   - Request body schema
   - Response schema
   - Error codes
   - Authentication requirements
3. Generate OpenAPI spec
4. Generate markdown documentation
5. Include curl examples

doc-readme:
Generate/update README.md
1. Analyze project:
   - Language and framework
   - Dependencies
   - Entry points
   - Tests
2. Generate sections:
   - Project title and description
   - Features
   - Installation
   - Usage examples
   - API reference (if applicable)
   - Configuration
   - Development setup
   - Testing
   - Contributing
   - License
3. Include badges (build status, coverage, version)

Output Format:
- Clear, concise language
- Code examples
- Proper formatting
- Links to relevant docs"
```

---

## 🚀 Skill 5: Deployment Skill

### Objectives
Create a skill that guides safe deployments.

### Ask Claude

```
"Create a deploy skill that:

Structure:
- Location: ~/.claude/skills/deploy/SKILL.md
- Commands: deploy, rollback, deploy-check
- Tools: Read, Bash(*)

deploy [environment] [version]:
Deployment workflow with comprehensive checks

Pre-deployment Checks:
1. Verify environment (staging, production)
2. Check git status (no uncommitted changes)
3. Verify on correct branch
4. Pull latest changes
5. Run all tests
6. Run linter
7. Check dependencies (npm audit, outdated)
8. Verify environment variables set
9. Check database migrations ready
10. Confirm deployment (require explicit yes)

Deployment Steps:
1. Create deployment tag (v{version})
2. Build production assets
3. Run database migrations (if any)
4. Deploy to environment
5. Health check (ping /health endpoint)
6. Smoke tests (critical paths work)
7. Monitor for errors (first 5 minutes)
8. Update deployment log

Post-deployment:
1. Verify deployment successful
2. Test critical functionality
3. Monitor error rates
4. Update documentation
5. Notify team (Slack/email)

rollback:
Emergency rollback procedure
1. Confirm rollback needed
2. Identify last good deployment
3. Rollback code
4. Rollback database (if needed)
5. Clear caches
6. Health check
7. Notify team
8. Create incident report

deploy-check:
Pre-deployment validation only
1. Run all pre-deployment checks
2. Generate go/no-go report
3. List any blockers
4. Provide recommendations

Safety Features:
- Require explicit confirmation for production
- Automatic rollback on health check failure
- Deployment lock file (prevent concurrent deploys)
- Comprehensive logging
- Post-deployment monitoring

Include checklists:
- Staging deployment checklist
- Production deployment checklist
- Rollback checklist"
```

---

## ✅ Verification Checklist

### Skill Quality
For each skill, verify:
- [ ] Clear, specific instructions
- [ ] Proper YAML frontmatter
- [ ] Appropriate tools listed
- [ ] Examples included
- [ ] Error handling considered
- [ ] Output format specified

### Functionality
- [ ] Each skill works on real code
- [ ] Skills produce expected output
- [ ] Edge cases handled
- [ ] No unintended side effects

### Documentation
- [ ] Each skill has clear description
- [ ] Usage examples provided
- [ ] Supporting files documented
- [ ] README for skill library

### Testing
- [ ] Tested each skill manually
- [ ] Tested with various inputs
- [ ] Tested error conditions
- [ ] Verified output quality

---

## 🎨 Enhancement Ideas

### Advanced Features

1. **Skill Composition**:
```
Create a "full-review" skill that:
- Runs code-review skill
- Runs test-generator for uncovered code
- Runs documentation skill for undocumented functions
- Generates comprehensive report
```

2. **Project-Specific Skills**:
```
Create skills specific to your project:
- Component generator (React/Vue/Angular)
- Database migration generator
- API endpoint generator
- Deployment to your specific infrastructure
```

3. **Team Skills**:
```
Create skills for team collaboration:
- PR review assistant
- Onboarding guide
- Architecture decision record generator
- Meeting notes formatter
```

4. **Workflow Skills**:
```
Create workflow automation skills:
- Feature branch workflow
- Bug fix workflow
- Release workflow
- Hotfix workflow
```

---

## 📚 Sharing Your Skills

### Package for Distribution

1. **Create README**:
```markdown
# My Claude Code Skills

A collection of skills for [purpose].

## Installation

\`\`\`bash
cp -r skills/* ~/.claude/skills/
\`\`\`

## Available Skills

### code-review
Comprehensive code review with security and quality checks.

Usage: `/code-review [files]`

### test-generator
Generate comprehensive test suites.

Usage: `/gen-tests [file]`

[Continue for each skill...]
```

2. **Create LICENSE**:
```
Choose a license (MIT, Apache, etc.)
```

3. **Package Skills**:
```bash
# Create archive
tar -czf my-claude-skills.tar.gz skills/ README.md LICENSE

# Or create GitHub repo
git init
git add .
git commit -m "Initial commit: Claude Code skills library"
git remote add origin [your-repo]
git push
```

4. **Share**:
- GitHub repository
- Company internal registry
- Claude Code community
- Blog post with examples

---

## 💡 Tips

### Skill Development Process

1. **Start Simple**:
```
Build basic version first
Test thoroughly
Add features incrementally
```

2. **Test on Real Code**:
```
Don't just test on examples
Use your actual projects
Find edge cases
```

3. **Iterate Based on Usage**:
```
Use the skills daily
Note pain points
Refine instructions
Add missing checks
```

4. **Document Everything**:
```
Clear descriptions
Usage examples
Expected output
Known limitations
```

---

## 🎯 Success Criteria

By completing this project, you will have:

✅ **Functional Skill Library**:
- 5+ working skills
- Tested on real code
- Well-documented

✅ **Practical Skills**:
- Skills you actually use
- Solve real problems
- Save time daily

✅ **Shareable Package**:
- Properly structured
- Good documentation
- Easy to install

✅ **Advanced Understanding**:
- Skill system mastery
- YAML frontmatter
- Tool restrictions
- Best practices

---

## 🚀 Next Steps

After completing this project:

1. **Daily Use**: Use your skills every day
2. **Refine**: Based on real usage, improve them
3. **Expand**: Add more skills as needs arise
4. **Share**: Contribute to community
5. **Advanced**: Create skill plugins

---

**Estimated time**: 4-6 hours
**Difficulty**: Intermediate
**Skills practiced**: Skill creation, YAML, workflow design, documentation
**Outcome**: Production-ready skill library you'll use daily
