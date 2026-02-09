# Project 20: Enterprise Claude Code Deployment

**Level**: Expert
**Time**: 20-40 hours
**Prerequisites**: Completed all previous sections (Projects 1-19, Lessons 1-100)

---

## Overview

Design, implement, and roll out an enterprise-grade Claude Code deployment for an engineering organization of 50+ developers. This capstone project brings together every concept from the bootcamp -- skills, hooks, subagents, MCP servers, CLAUDE.md, managed settings, and CI/CD integration -- into a production-ready system with governance, compliance, training, and measurable outcomes.

By the end of this project you will have a fully operational enterprise deployment that includes a company-wide plugin, CI/CD pipelines, a governance framework, training materials, and a metrics dashboard for continuous improvement.

---

## Architecture Overview

```
+-----------------------------------------------------------------------+
|                     Enterprise Claude Code Platform                    |
+-----------------------------------------------------------------------+
|                                                                        |
|  +------------------+    +-------------------+    +-----------------+  |
|  |  Managed         |    |  Enterprise       |    |  CI/CD          |  |
|  |  Settings        |    |  Plugin           |    |  Integration    |  |
|  |  (settings.json) |    |  (skills, hooks,  |    |  (GitHub        |  |
|  |                  |    |   subagents, MCP)  |    |   Actions)      |  |
|  +--------+---------+    +--------+----------+    +--------+--------+  |
|           |                       |                        |           |
|           v                       v                        v           |
|  +--------+---------+    +--------+----------+    +--------+--------+  |
|  |  Governance       |    |  Developer        |    |  Automation     |  |
|  |  - Permissions    |    |  Workstations     |    |  - Code Review  |  |
|  |  - Compliance     |    |  - Local Claude   |    |  - Testing      |  |
|  |  - Cost Tracking  |    |  - Shared Config  |    |  - Security     |  |
|  |  - Audit Logs     |    |  - Team Skills    |    |  - Deploy Gates |  |
|  +------------------+    +-------------------+    +-----------------+  |
|                                                                        |
|  +------------------+    +-------------------+    +-----------------+  |
|  |  Training &       |    |  Metrics &        |    |  Support &      |  |
|  |  Onboarding       |    |  Analytics        |    |  Maintenance    |  |
|  |  - Bootcamp       |    |  - Adoption       |    |  - Help Desk    |  |
|  |  - Documentation  |    |  - Velocity       |    |  - Updates      |  |
|  |  - Best Practices |    |  - Quality        |    |  - Roadmap      |  |
|  +------------------+    +-------------------+    +-----------------+  |
+-----------------------------------------------------------------------+
```

---

## Phase 1: Assessment & Planning (4 hours)

### Task 1.1: Audit Current Development Workflows

Conduct a thorough audit of your organization's existing development processes. Document every workflow that Claude Code could augment or automate.

Create an assessment document:

```markdown
# Enterprise Claude Code Assessment

## Current Development Workflows

### Code Review Process
- **Current**: Manual PR reviews, average 24h turnaround
- **Pain Points**: Inconsistent standards, reviewer fatigue, missed issues
- **Opportunity**: Automated first-pass review, style enforcement, security scanning

### Testing Practices
- **Current**: Manual test writing, ~60% coverage
- **Pain Points**: Low coverage on legacy code, inconsistent test quality
- **Opportunity**: Automated test generation, coverage gap analysis

### Documentation
- **Current**: Ad-hoc, often outdated
- **Pain Points**: New engineers struggle to onboard, API docs incomplete
- **Opportunity**: Auto-generated docs, enforced documentation on new code

### Deployment
- **Current**: Semi-manual deploys with checklist
- **Pain Points**: Missed steps, configuration drift, rollback complexity
- **Opportunity**: Automated pre-deploy validation, deployment gate checks

### Security
- **Current**: Quarterly security audits, manual review
- **Pain Points**: Vulnerabilities found late, inconsistent scanning
- **Opportunity**: Continuous security scanning in hooks and CI/CD
```

### Task 1.2: Identify Automation Opportunities

Rank each opportunity by impact and effort:

```markdown
## Automation Priority Matrix

| Opportunity              | Impact | Effort | Priority |
|--------------------------|--------|--------|----------|
| Automated code review    | High   | Medium | P0       |
| Pre-commit security scan | High   | Low    | P0       |
| Test generation          | High   | Medium | P1       |
| Documentation generation | Medium | Low    | P1       |
| Deploy gate checks       | High   | Medium | P1       |
| Onboarding automation    | Medium | Medium | P2       |
| Incident response assist | Medium | High   | P2       |
| Architecture review      | Low    | High   | P3       |
```

### Task 1.3: Define Governance Requirements

```markdown
## Governance Requirements

### Security Policies
- No secrets in prompts or CLAUDE.md files
- All Bash commands must be explicitly allowlisted
- Network access restricted to approved domains
- File system access scoped to project directories

### Compliance Requirements
- Audit log of all Claude Code invocations
- Data residency constraints (no code sent to unapproved regions)
- License compliance checking on generated code
- SOC 2 / ISO 27001 alignment

### Cost Controls
- Per-team monthly spend limits
- Per-developer daily token budgets
- Alerting thresholds at 80% and 95% of budget
- Quarterly cost review process

### Access Controls
- Role-based permission tiers (junior, senior, lead, admin)
- Project-level access restrictions
- Approval workflow for production-touching operations
```

### Task 1.4: Create Deployment Plan

```markdown
## Deployment Plan

### Timeline
- Weeks 1-2: Build enterprise plugin and managed settings
- Weeks 3-4: CI/CD integration and governance framework
- Weeks 5-6: Pilot with 10 engineers (2 teams)
- Weeks 7-8: Incorporate feedback, iterate
- Weeks 9-10: Training materials and onboarding program
- Weeks 11-12: Company-wide rollout (phased by team)
- Ongoing: Measurement, optimization, support

### Success Criteria
- 90%+ adoption within 3 months of rollout
- 2x improvement in PR turnaround time
- 50% reduction in bugs reaching production
- Positive ROI within first quarter
- 90%+ developer satisfaction score

### Risk Mitigation
| Risk                        | Mitigation                              |
|-----------------------------|-----------------------------------------|
| Low adoption                | Champions program, executive sponsorship|
| Security incident           | Strict allowlists, audit logging        |
| Cost overrun                | Budget limits, alerting, monthly review |
| Quality degradation         | Mandatory human review, test gates      |
| Vendor lock-in              | Abstract integration points, document   |
```

### Phase 1 Deliverables
- [ ] Completed assessment document with workflow audit
- [ ] Prioritized automation opportunity matrix
- [ ] Governance requirements specification
- [ ] Deployment plan with timeline, risks, and success criteria

---

## Phase 2: Enterprise Plugin Development (8 hours)

### Task 2.1: Plugin Directory Structure

Create the enterprise plugin with a comprehensive structure:

```
company-claude-plugin/
├── CLAUDE.md                          # Organization-wide instructions
├── settings.json                      # Managed settings
├── skills/
│   ├── code-review/
│   │   └── SKILL.md                   # Automated code review
│   ├── deploy/
│   │   └── SKILL.md                   # Deployment workflow
│   ├── test-gen/
│   │   └── SKILL.md                   # Test generation
│   ├── security-scan/
│   │   └── SKILL.md                   # Security scanning
│   ├── doc-gen/
│   │   └── SKILL.md                   # Documentation generation
│   ├── incident-response/
│   │   └── SKILL.md                   # Incident response assistant
│   ├── onboard/
│   │   └── SKILL.md                   # New engineer onboarding
│   ├── architecture-review/
│   │   └── SKILL.md                   # Architecture decision review
│   ├── migrate/
│   │   └── SKILL.md                   # Code migration assistant
│   └── perf-audit/
│       └── SKILL.md                   # Performance audit
├── hooks/
│   ├── pre-commit-security.sh         # Security pre-commit checks
│   ├── pre-commit-lint.sh             # Linting enforcement
│   ├── post-tool-use-audit.sh         # Audit logging
│   ├── pre-tool-use-permission.sh     # Permission enforcement
│   └── notification.sh                # Slack/Teams notifications
├── agents/
│   ├── security-reviewer/
│   │   └── SUBAGENT.md                # Security review subagent
│   ├── test-writer/
│   │   └── SUBAGENT.md                # Test writing subagent
│   ├── doc-writer/
│   │   └── SUBAGENT.md                # Documentation subagent
│   └── code-analyzer/
│       └── SUBAGENT.md                # Code analysis subagent
├── mcp-servers/
│   ├── jira-integration/              # Jira ticket management
│   ├── slack-notifier/                # Slack notifications
│   └── metrics-collector/             # Usage metrics collection
├── templates/
│   ├── CLAUDE.md.team-template        # Per-team CLAUDE.md template
│   ├── CLAUDE.md.project-template     # Per-project CLAUDE.md template
│   └── onboarding-checklist.md        # New engineer checklist
└── scripts/
    ├── install.sh                     # Plugin installation script
    ├── update.sh                      # Plugin update script
    └── verify.sh                      # Installation verification
```

### Task 2.2: Organization-Wide CLAUDE.md

Create the root CLAUDE.md that applies to all projects:

```markdown
# CLAUDE.md - Acme Corp Engineering Standards

## Company Context
Acme Corp is a SaaS platform serving enterprise customers. Our stack is
TypeScript/Node.js (backend), React/TypeScript (frontend), PostgreSQL
(database), and AWS (infrastructure). We practice trunk-based development
with short-lived feature branches.

## Code Standards

### General
- All code must pass ESLint and Prettier before commit
- Maximum function length: 50 lines
- Maximum file length: 400 lines
- Maximum cyclomatic complexity: 10
- All public functions must have JSDoc documentation
- All new code must have corresponding tests (minimum 80% coverage)

### TypeScript
- Strict mode enabled, no `any` types without documented justification
- Use interfaces over type aliases for object shapes
- Prefer immutable patterns (const, readonly, Object.freeze)
- Use barrel exports (index.ts) for module boundaries

### React
- Functional components only (no class components)
- Custom hooks for shared logic (prefix with `use`)
- Component files: one component per file
- Props interfaces exported alongside components
- Use React.memo only with measured performance justification

### API Design
- RESTful conventions for CRUD operations
- GraphQL for complex data fetching
- All endpoints must have OpenAPI documentation
- Rate limiting on all public endpoints
- Input validation using Zod schemas

### Database
- All schema changes via migrations (never manual DDL)
- Indexes required for any column used in WHERE clauses
- No N+1 queries (use DataLoader or eager loading)
- Soft deletes preferred over hard deletes

## Security Requirements

### Mandatory
- Never commit secrets, tokens, or credentials
- All user input must be validated and sanitized
- SQL queries must use parameterized statements
- Authentication required on all non-public endpoints
- HTTPS only, no mixed content

### Prohibited Patterns
- eval() or Function() constructor
- innerHTML with user-supplied content
- Disabling CORS without security review
- Storing passwords in plaintext
- Logging sensitive user data (PII, credentials)

## Git Conventions
- Branch naming: feature/JIRA-123-short-description
- Commit messages: conventional commits format
  - feat: new feature
  - fix: bug fix
  - refactor: code change that neither fixes nor adds
  - test: adding or updating tests
  - docs: documentation changes
  - chore: maintenance tasks
- PRs require at least 1 approval
- Squash merge to main

## Testing Standards
- Unit tests: Jest with React Testing Library
- Integration tests: Supertest for APIs
- E2E tests: Playwright for critical user flows
- Test naming: "should [expected behavior] when [condition]"
- Arrange-Act-Assert structure
- No test interdependence (each test must be independent)

## Error Handling
- Use custom error classes extending BaseError
- All async operations must have error handling
- User-facing errors must have error codes
- Log errors with structured format (JSON)
- Never swallow errors silently

## Performance Guidelines
- API response time target: <200ms (p95)
- Bundle size budget: <250KB gzipped
- Database query time target: <50ms
- Cache frequently accessed data (Redis, 5-minute TTL default)
- Lazy load non-critical components
```

### Task 2.3: Standardized Skills

#### Code Review Skill

Create `skills/code-review/SKILL.md`:

```markdown
---
name: code-review
description: Comprehensive automated code review following Acme Corp standards
commands:
  - review
  - review-pr
  - review-security
tools:
  - Read
  - Glob
  - Grep
  - Bash(git diff)
  - Bash(git log)
  - Bash(gh pr view)
  - Bash(gh pr diff)
---

# Code Review Skill

You are a senior engineer at Acme Corp performing a thorough code review.

## Invocation

- `/review` - Review staged or uncommitted changes
- `/review-pr <number>` - Review a specific pull request
- `/review-security` - Security-focused review only

## Review Process

1. **Gather Context**
   - Identify all changed files
   - Read the PR description or commit messages
   - Understand the intent of the changes

2. **Standards Compliance**
   - Check against CLAUDE.md coding standards
   - Verify TypeScript strict mode compliance
   - Confirm JSDoc on all public functions
   - Validate naming conventions

3. **Security Analysis**
   - Input validation present on all user inputs
   - No hardcoded secrets or credentials
   - SQL injection prevention (parameterized queries)
   - XSS prevention (no innerHTML with user data)
   - Authentication and authorization checks
   - Dependency vulnerabilities (check package.json changes)

4. **Code Quality**
   - Function length (<50 lines)
   - Cyclomatic complexity (<10)
   - DRY violations (duplicated logic)
   - Error handling completeness
   - Edge case coverage

5. **Performance**
   - N+1 query detection
   - Unnecessary re-renders in React components
   - Missing database indexes for new queries
   - Bundle size impact of new dependencies

6. **Testing**
   - New code has corresponding tests
   - Test quality (meaningful assertions, not just coverage)
   - Edge cases tested
   - Error paths tested

## Output Format

\`\`\`markdown
# Code Review Report

## Summary
[1-2 sentence overview of changes and overall assessment]

## Critical Issues (must fix before merge)
- **[CRIT-1]** [file:line] - [description]
  - **Why**: [explanation of risk]
  - **Fix**: [specific recommendation]

## Major Issues (should fix before merge)
- **[MAJ-1]** [file:line] - [description]
  - **Fix**: [recommendation]

## Minor Issues (consider fixing)
- **[MIN-1]** [file:line] - [description]

## Suggestions (optional improvements)
- **[SUG-1]** [description]

## Positive Notes
- [What was done well]

## Verdict
APPROVED / CHANGES REQUESTED / BLOCKED
[Reasoning for verdict]
\`\`\`
```

#### Deployment Skill

Create `skills/deploy/SKILL.md`:

```markdown
---
name: deploy
description: Safe deployment workflow with pre-flight checks
commands:
  - deploy
  - deploy-check
  - rollback
tools:
  - Read
  - Glob
  - Grep
  - Bash(git status)
  - Bash(git log)
  - Bash(git tag)
  - Bash(npm run build)
  - Bash(npm test)
  - Bash(npm audit)
  - Bash(curl)
---

# Deployment Skill

## Commands

### /deploy [environment]
Full deployment workflow with safety checks.

### /deploy-check
Pre-deployment validation only (dry run).

### /rollback [version]
Emergency rollback procedure.

## Pre-Deployment Checklist

Before ANY deployment, verify ALL of the following:

1. **Git State**
   - On correct branch (main for production, develop for staging)
   - No uncommitted changes
   - Branch is up to date with remote
   - All CI checks passing

2. **Code Quality**
   - All tests pass locally
   - Linter passes with no errors
   - No TypeScript compilation errors
   - No known critical vulnerabilities (npm audit)

3. **Database**
   - Migrations are ready and tested
   - Rollback migrations exist
   - No breaking schema changes without migration plan

4. **Configuration**
   - Environment variables documented
   - Feature flags configured
   - Third-party service configurations verified

5. **Approval**
   - PR approved by at least 1 reviewer
   - Product owner sign-off for user-facing changes
   - Security review for auth/data changes

## Deployment Steps

1. Create deployment tag: v{major}.{minor}.{patch}
2. Build production assets
3. Run database migrations
4. Deploy to target environment
5. Health check: GET /api/health (expect 200)
6. Smoke tests: critical user flows
7. Monitor error rates for 10 minutes
8. Update deployment log

## Post-Deployment Verification

- Health endpoint returns 200
- Key API endpoints respond correctly
- Error rate has not increased
- No new error types in logs
- Performance metrics within baseline
```

#### Test Generation Skill

Create `skills/test-gen/SKILL.md`:

```markdown
---
name: test-gen
description: Generate comprehensive test suites following Acme Corp standards
commands:
  - gen-tests
  - gen-tests-coverage
tools:
  - Read
  - Write
  - Glob
  - Grep
  - Bash(npm test)
  - Bash(npx jest --coverage)
---

# Test Generation Skill

## Commands

- `/gen-tests [file]` - Generate tests for a specific file
- `/gen-tests-coverage` - Analyze coverage gaps and generate missing tests

## Process

1. **Analyze Source File**
   - Read the source file
   - Identify all exported functions, classes, and methods
   - Determine dependencies that need mocking
   - Identify edge cases and error conditions

2. **Check Existing Tests**
   - Look for existing test file
   - Analyze current coverage if tests exist
   - Identify untested code paths

3. **Detect Testing Framework**
   - Check package.json for jest, mocha, vitest
   - Check existing test files for patterns
   - Default to Jest if unclear

4. **Generate Tests**
   - Follow Arrange-Act-Assert pattern
   - Use descriptive test names: "should [behavior] when [condition]"
   - Include happy path, error cases, and edge cases
   - Mock external dependencies
   - Test async operations with proper await/resolve patterns

5. **Verify Tests**
   - Run the generated tests
   - Fix any failures
   - Report coverage achieved

## Test Template

\`\`\`typescript
import { describe, it, expect, jest, beforeEach } from '@jest/globals';
import { FunctionUnderTest } from '../src/module';

// Mock dependencies
jest.mock('../src/dependency', () => ({
  externalCall: jest.fn(),
}));

describe('FunctionUnderTest', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  describe('happy path', () => {
    it('should return expected result when given valid input', () => {
      // Arrange
      const input = { key: 'value' };

      // Act
      const result = FunctionUnderTest(input);

      // Assert
      expect(result).toEqual(expectedOutput);
    });
  });

  describe('error handling', () => {
    it('should throw ValidationError when input is invalid', () => {
      expect(() => FunctionUnderTest(null))
        .toThrow(ValidationError);
    });
  });

  describe('edge cases', () => {
    it('should handle empty input gracefully', () => {
      const result = FunctionUnderTest({});
      expect(result).toEqual(defaultOutput);
    });
  });
});
\`\`\`
```

### Task 2.4: Enforcement Hooks

Create hooks that enforce standards automatically.

#### Pre-Commit Security Hook

Create `hooks/pre-commit-security.sh`:

```bash
#!/bin/bash
# Pre-commit security scanning hook for Claude Code
# Blocks commits containing secrets, credentials, or security violations

set -euo pipefail

STAGED_FILES=$(git diff --cached --name-only --diff-filter=ACM)
EXIT_CODE=0

echo "Running security pre-commit checks..."

# Check for hardcoded secrets
SECRET_PATTERNS=(
  'AKIA[0-9A-Z]{16}'                    # AWS Access Key
  'password\s*=\s*["\x27][^"\x27]+'     # Hardcoded passwords
  'api[_-]?key\s*=\s*["\x27][^"\x27]+'  # API keys
  'secret\s*=\s*["\x27][^"\x27]+'       # Generic secrets
  'BEGIN\s+(RSA|DSA|EC)\s+PRIVATE'       # Private keys
  'ghp_[a-zA-Z0-9]{36}'                 # GitHub PAT
  'sk-[a-zA-Z0-9]{48}'                  # OpenAI/Anthropic keys
)

for file in $STAGED_FILES; do
  if [[ -f "$file" ]]; then
    for pattern in "${SECRET_PATTERNS[@]}"; do
      if grep -qEi "$pattern" "$file" 2>/dev/null; then
        echo "BLOCKED: Potential secret found in $file (pattern: $pattern)"
        EXIT_CODE=1
      fi
    done
  fi
done

# Check for prohibited patterns
for file in $STAGED_FILES; do
  if [[ "$file" =~ \.(js|ts|tsx|jsx)$ ]] && [[ -f "$file" ]]; then
    if grep -q 'eval(' "$file" 2>/dev/null; then
      echo "BLOCKED: eval() usage found in $file"
      EXIT_CODE=1
    fi
    if grep -q '\.innerHTML\s*=' "$file" 2>/dev/null; then
      echo "WARNING: innerHTML assignment in $file - verify no user input"
    fi
  fi
done

# Check for .env files being committed
for file in $STAGED_FILES; do
  if [[ "$file" =~ \.env$ ]] || [[ "$file" =~ \.env\. ]]; then
    echo "BLOCKED: Environment file $file should not be committed"
    EXIT_CODE=1
  fi
done

if [[ $EXIT_CODE -eq 0 ]]; then
  echo "Security checks passed."
else
  echo ""
  echo "Security checks FAILED. Fix the issues above before committing."
fi

exit $EXIT_CODE
```

#### Post-Tool-Use Audit Hook

Create `hooks/post-tool-use-audit.sh`:

```bash
#!/bin/bash
# Audit logging hook - records all Claude Code tool invocations
# Writes structured logs for compliance and analytics

AUDIT_LOG="${HOME}/.claude/audit/audit-$(date +%Y-%m-%d).jsonl"
mkdir -p "$(dirname "$AUDIT_LOG")"

# Tool invocation details are passed via environment variables
# CLAUDE_TOOL_NAME, CLAUDE_TOOL_ARGS, CLAUDE_SESSION_ID, CLAUDE_PROJECT

TIMESTAMP=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
USER=$(whoami)
PROJECT=$(basename "$(pwd)")

cat >> "$AUDIT_LOG" << JSONEOF
{"timestamp":"${TIMESTAMP}","user":"${USER}","project":"${PROJECT}","tool":"${CLAUDE_TOOL_NAME:-unknown}","session":"${CLAUDE_SESSION_ID:-unknown}","working_dir":"$(pwd)"}
JSONEOF
```

### Task 2.5: Specialized Subagents

#### Security Reviewer Subagent

Create `agents/security-reviewer/SUBAGENT.md`:

```markdown
---
name: security-reviewer
description: Performs deep security analysis of code changes
model: opus
tools:
  - Read
  - Glob
  - Grep
---

# Security Reviewer Subagent

You are a senior application security engineer performing a thorough
security review.

## Responsibilities

1. **Vulnerability Detection**
   - Injection flaws (SQL, NoSQL, LDAP, OS command)
   - Cross-site scripting (XSS) - reflected, stored, DOM-based
   - Broken authentication and session management
   - Insecure direct object references
   - Security misconfiguration
   - Sensitive data exposure
   - Missing function-level access control
   - Cross-site request forgery (CSRF)
   - Using components with known vulnerabilities
   - Insufficient logging and monitoring

2. **Authentication & Authorization**
   - Verify auth checks on all protected endpoints
   - Check for privilege escalation paths
   - Validate session handling
   - Review token generation and validation

3. **Data Protection**
   - PII handling compliance
   - Encryption at rest and in transit
   - Secure key management
   - Data retention policies

## Output Format

\`\`\`markdown
# Security Review Report

## Risk Level: [CRITICAL / HIGH / MEDIUM / LOW / NONE]

## Findings

### [SEV-1] Critical
- [Finding with file:line reference]
- **Impact**: [description]
- **Remediation**: [specific fix]

### [SEV-2] High
- [Finding with file:line reference]

### [SEV-3] Medium
- [Finding with file:line reference]

## Compliance Notes
- [Relevant compliance observations]

## Recommendations
- [Proactive security improvements]
\`\`\`
```

#### Test Writer Subagent

Create `agents/test-writer/SUBAGENT.md`:

```markdown
---
name: test-writer
description: Generates comprehensive test suites for code changes
model: sonnet
tools:
  - Read
  - Write
  - Glob
  - Grep
  - Bash(npm test)
  - Bash(npx jest)
---

# Test Writer Subagent

You are a QA engineer specializing in automated test development.

## Responsibilities

1. **Analyze Code Under Test**
   - Read source files and understand behavior
   - Identify all code paths and branches
   - Determine external dependencies to mock
   - Find edge cases and boundary conditions

2. **Generate Tests**
   - Unit tests for individual functions
   - Integration tests for module interactions
   - Error handling tests for failure modes
   - Boundary value tests for edge cases

3. **Verify Quality**
   - Run tests and confirm they pass
   - Check coverage meets 80% minimum threshold
   - Ensure tests are deterministic (no flaky tests)
   - Validate mocks accurately represent dependencies

## Test Writing Rules
- One assertion concept per test
- Descriptive test names that document behavior
- No test interdependence
- Clean setup and teardown
- Meaningful assertions (not just "does not throw")
```

### Phase 2 Deliverables
- [ ] Complete plugin directory structure created
- [ ] Organization-wide CLAUDE.md with all coding standards
- [ ] 10 standardized skills (code-review, deploy, test-gen, security-scan, doc-gen, incident-response, onboard, architecture-review, migrate, perf-audit)
- [ ] 5 enforcement hooks (pre-commit security, lint, audit, permissions, notifications)
- [ ] 4 specialized subagents (security-reviewer, test-writer, doc-writer, code-analyzer)
- [ ] Installation and update scripts

---

## Phase 3: CI/CD Integration (6 hours)

### Task 3.1: Automated Code Review Workflow

Create `.github/workflows/claude-code-review.yml`:

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write

jobs:
  claude-review:
    runs-on: ubuntu-latest
    timeout-minutes: 15

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run Claude Code Review
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Review this pull request following our CLAUDE.md standards.

            Focus on:
            1. Code quality and standards compliance
            2. Security vulnerabilities
            3. Performance concerns
            4. Test coverage for new code
            5. Documentation completeness

            For each issue found, provide:
            - Severity (critical, major, minor, suggestion)
            - File and line reference
            - Clear explanation
            - Specific fix recommendation

            End with an overall assessment and verdict.
          allowed_tools: |
            Read
            Glob
            Grep
            Bash(git diff)
            Bash(git log)
          direct_prompt: true

      - name: Post Review Summary
        if: always()
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            // Post review results as PR comment
            const reviewOutput = process.env.CLAUDE_OUTPUT || 'Review completed.';
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: `## Claude Code Review\n\n${reviewOutput}`
            });
```

### Task 3.2: Security Scanning Workflow

Create `.github/workflows/claude-security-scan.yml`:

```yaml
name: Claude Security Scan

on:
  pull_request:
    types: [opened, synchronize]
    paths:
      - 'src/**'
      - 'packages/**'
      - 'package.json'
      - 'package-lock.json'

permissions:
  contents: read
  pull-requests: write
  security-events: write

jobs:
  security-scan:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Run npm audit
        id: npm-audit
        run: |
          npm audit --json > audit-results.json 2>&1 || true
        continue-on-error: true

      - name: Claude Security Analysis
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Perform a security-focused review of the changes in this PR.

            Also review the npm audit results in audit-results.json if present.

            Check for:
            1. OWASP Top 10 vulnerabilities
            2. Hardcoded secrets or credentials
            3. Injection vulnerabilities (SQL, XSS, command injection)
            4. Authentication and authorization issues
            5. Insecure data handling
            6. Dependency vulnerabilities
            7. Configuration security

            Rate overall security risk: CRITICAL / HIGH / MEDIUM / LOW

            If CRITICAL or HIGH, this PR should be blocked until resolved.
          allowed_tools: |
            Read
            Glob
            Grep
            Bash(git diff)
          direct_prompt: true

      - name: Block on Critical Security Issues
        if: contains(env.CLAUDE_OUTPUT, 'CRITICAL')
        run: |
          echo "::error::Critical security issues detected. PR blocked."
          exit 1
```

### Task 3.3: Test Automation Workflow

Create `.github/workflows/claude-test-coverage.yml`:

```yaml
name: Claude Test Coverage

on:
  pull_request:
    types: [opened, synchronize]

permissions:
  contents: read
  pull-requests: write

jobs:
  test-coverage:
    runs-on: ubuntu-latest
    timeout-minutes: 20

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run existing tests with coverage
        id: test-run
        run: |
          npx jest --coverage --coverageReporters=json-summary \
            --coverageReporters=text 2>&1 | tee test-output.txt
        continue-on-error: true

      - name: Claude Test Analysis
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Analyze the test results and coverage for this PR.

            1. Review test-output.txt for test results
            2. Identify changed files that lack test coverage
            3. Check if new functions have corresponding tests
            4. Verify test quality (meaningful assertions, edge cases)

            Report:
            - Files with insufficient coverage (<80%)
            - New code paths without tests
            - Suggestions for additional test cases
            - Overall test health assessment
          allowed_tools: |
            Read
            Glob
            Grep
          direct_prompt: true
```

### Task 3.4: Deployment Gate Workflow

Create `.github/workflows/claude-deploy-gate.yml`:

```yaml
name: Claude Deployment Gate

on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Target environment'
        required: true
        type: choice
        options:
          - staging
          - production
      version:
        description: 'Version to deploy'
        required: true
        type: string

permissions:
  contents: read
  deployments: write

jobs:
  pre-deploy-validation:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    environment: ${{ github.event.inputs.environment }}

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          ref: ${{ github.event.inputs.version }}

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install and build
        run: |
          npm ci
          npm run build

      - name: Run full test suite
        run: npm test

      - name: Claude Pre-Deploy Validation
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Perform pre-deployment validation for deploying version
            ${{ github.event.inputs.version }} to
            ${{ github.event.inputs.environment }}.

            Checks:
            1. All tests pass (review test output)
            2. No critical npm audit findings
            3. Database migrations are safe (no data loss risk)
            4. Environment configuration is complete
            5. No breaking API changes without version bump
            6. Feature flags configured for gradual rollout
            7. Rollback plan exists

            For production deployments, additionally verify:
            - Load testing results acceptable
            - Monitoring and alerting configured
            - Runbook updated
            - On-call team notified

            Provide a GO / NO-GO recommendation with reasoning.
          allowed_tools: |
            Read
            Glob
            Grep
            Bash(npm audit)
            Bash(npm test)
          direct_prompt: true

      - name: Gate Decision
        if: contains(env.CLAUDE_OUTPUT, 'NO-GO')
        run: |
          echo "::error::Deployment blocked by pre-deploy validation."
          exit 1
```

### Phase 3 Deliverables
- [ ] Automated code review GitHub Action workflow
- [ ] Security scanning workflow with PR blocking on critical issues
- [ ] Test coverage analysis workflow
- [ ] Deployment gate workflow with GO/NO-GO decision
- [ ] All workflows tested on a pilot repository

---

## Phase 4: Governance & Compliance (4 hours)

### Task 4.1: Enterprise Managed Settings

Create the organization-level `settings.json` that enforces policies across all teams:

```jsonc
{
  // Enterprise managed settings - deployed via MDM or config management
  // Location: /etc/claude/settings.json (Linux/Mac) or
  //           managed via Group Policy (Windows)

  "permissions": {
    // Tools available to all developers
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Write",
      "Edit",
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(npm run build)",
      "Bash(npx jest*)",
      "Bash(npx eslint*)",
      "Bash(npx prettier*)",
      "Bash(npx tsc*)",
      "Bash(git status)",
      "Bash(git diff*)",
      "Bash(git log*)",
      "Bash(git branch*)",
      "Bash(git add*)",
      "Bash(git commit*)",
      "Bash(git checkout*)",
      "Bash(git switch*)",
      "Bash(git merge*)",
      "Bash(git rebase*)",
      "Bash(git stash*)",
      "Bash(git tag*)",
      "Bash(gh pr*)",
      "Bash(gh issue*)",
      "Bash(curl --max-time 10 https://*.acme-corp.com/*)",
      "Bash(docker compose*)",
      "Bash(docker build*)",
      "WebFetch(*.acme-corp.com/*)"
    ],

    // Explicitly denied operations
    "deny": [
      "Bash(rm -rf /)",
      "Bash(chmod 777*)",
      "Bash(curl * | bash)",
      "Bash(wget * | sh)",
      "Bash(npm publish*)",
      "Bash(git push --force*)",
      "Bash(git reset --hard*)",
      "Bash(docker push*)",
      "Bash(ssh *)",
      "Bash(scp *)",
      "Bash(sudo *)",
      "WebFetch(*.competitor.com/*)"
    ]
  },

  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "node /opt/claude-enterprise/hooks/validate-write.js",
        "description": "Validate file writes against policy"
      }
    ],
    "PostToolUse": [
      {
        "matcher": ".*",
        "command": "/opt/claude-enterprise/hooks/audit-log.sh",
        "description": "Log all tool usage for compliance"
      }
    ],
    "PreCommit": [
      {
        "command": "/opt/claude-enterprise/hooks/pre-commit-security.sh",
        "description": "Security scanning before commit"
      }
    ]
  },

  "env": {
    "CLAUDE_MAX_TOKENS_PER_SESSION": "100000",
    "CLAUDE_AUDIT_LOG_PATH": "/var/log/claude/audit",
    "CLAUDE_ENTERPRISE_MODE": "true"
  }
}
```

### Task 4.2: Role-Based Permission Tiers

Define permission policies for different roles:

```jsonc
// junior-developer-settings.json
// Applied to engineers in their first 6 months
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Write",
      "Edit",
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(git status)",
      "Bash(git diff*)",
      "Bash(git log*)",
      "Bash(git add*)",
      "Bash(git commit*)"
    ],
    "deny": [
      "Bash(git push*)",
      "Bash(npm publish*)",
      "Bash(docker*)",
      "Bash(curl*)",
      "Bash(wget*)"
    ]
  }
}
```

```jsonc
// senior-developer-settings.json
// Applied to senior engineers and above
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Write",
      "Edit",
      "Bash(npm *)",
      "Bash(npx *)",
      "Bash(git *)",
      "Bash(gh *)",
      "Bash(docker *)",
      "Bash(curl --max-time 30 *)",
      "Bash(make *)",
      "WebFetch"
    ],
    "deny": [
      "Bash(rm -rf /)",
      "Bash(sudo *)",
      "Bash(ssh *)",
      "Bash(npm publish*)",
      "Bash(git push --force*)"
    ]
  }
}
```

### Task 4.3: Usage Analytics and Cost Tracking

Create a metrics collection system:

```markdown
## Usage Analytics Architecture

### Data Collection Points

1. **Session Metrics** (collected per session)
   - Session duration
   - Tool invocations (count by tool type)
   - Token usage (input + output)
   - Model used (Sonnet, Opus, Haiku)
   - Project/repository
   - User identity

2. **Quality Metrics** (collected per PR)
   - Issues found by automated review
   - Issues that matched human review findings
   - False positive rate
   - Time saved vs. manual review

3. **Cost Metrics** (aggregated daily)
   - Token cost per user
   - Token cost per team
   - Token cost per project
   - Cost trend (week-over-week)
```

Create `scripts/usage-report.sh`:

```bash
#!/bin/bash
# Generate weekly usage report from audit logs
# Run via cron: 0 9 * * MON /opt/claude-enterprise/scripts/usage-report.sh

AUDIT_DIR="${HOME}/.claude/audit"
REPORT_DATE=$(date +%Y-%m-%d)
WEEK_START=$(date -d "7 days ago" +%Y-%m-%d)
REPORT_FILE="/var/reports/claude/weekly-${REPORT_DATE}.md"

echo "# Claude Code Usage Report" > "$REPORT_FILE"
echo "## Week of ${WEEK_START} to ${REPORT_DATE}" >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

# Count sessions per user
echo "### Sessions by User" >> "$REPORT_FILE"
echo "| User | Sessions | Tool Calls |" >> "$REPORT_FILE"
echo "|------|----------|------------|" >> "$REPORT_FILE"

for log_file in "${AUDIT_DIR}"/audit-*.jsonl; do
  if [[ -f "$log_file" ]]; then
    # Parse JSONL and aggregate by user
    # (In production, use jq or a proper analytics pipeline)
    while IFS= read -r line; do
      user=$(echo "$line" | python3 -c "import sys,json; print(json.load(sys.stdin).get('user','unknown'))" 2>/dev/null)
      echo "$user"
    done < "$log_file"
  fi
done | sort | uniq -c | sort -rn | while read count user; do
  echo "| ${user} | ${count} | - |" >> "$REPORT_FILE"
done

echo "" >> "$REPORT_FILE"
echo "### Cost Summary" >> "$REPORT_FILE"
echo "*(Requires API usage data from Anthropic dashboard)*" >> "$REPORT_FILE"

echo "Report generated: ${REPORT_FILE}"
```

### Task 4.4: Compliance Checks

Create `scripts/compliance-check.sh`:

```bash
#!/bin/bash
# Compliance verification script
# Validates that enterprise Claude Code deployment meets policy requirements

set -euo pipefail

PASS=0
FAIL=0
WARN=0

check() {
  local description="$1"
  local result="$2"
  if [[ "$result" == "pass" ]]; then
    echo "  PASS: $description"
    PASS=$((PASS + 1))
  elif [[ "$result" == "warn" ]]; then
    echo "  WARN: $description"
    WARN=$((WARN + 1))
  else
    echo "  FAIL: $description"
    FAIL=$((FAIL + 1))
  fi
}

echo "============================================"
echo "Enterprise Claude Code Compliance Check"
echo "============================================"
echo ""

# 1. Managed settings deployed
echo "## Settings & Configuration"
if [[ -f "/etc/claude/settings.json" ]]; then
  check "Managed settings file exists" "pass"
else
  check "Managed settings file exists" "fail"
fi

# 2. Audit logging enabled
echo ""
echo "## Audit & Logging"
AUDIT_DIR="${HOME}/.claude/audit"
if [[ -d "$AUDIT_DIR" ]]; then
  check "Audit log directory exists" "pass"
  RECENT_LOGS=$(find "$AUDIT_DIR" -name "audit-*.jsonl" -mtime -1 | wc -l)
  if [[ "$RECENT_LOGS" -gt 0 ]]; then
    check "Audit logs being written (last 24h)" "pass"
  else
    check "Audit logs being written (last 24h)" "warn"
  fi
else
  check "Audit log directory exists" "fail"
fi

# 3. Security hooks configured
echo ""
echo "## Security Controls"
if grep -q "pre-commit-security" "/etc/claude/settings.json" 2>/dev/null; then
  check "Pre-commit security hook configured" "pass"
else
  check "Pre-commit security hook configured" "fail"
fi

# 4. Denied commands enforced
echo ""
echo "## Permission Enforcement"
if grep -q '"deny"' "/etc/claude/settings.json" 2>/dev/null; then
  check "Deny list configured" "pass"
else
  check "Deny list configured" "fail"
fi

echo ""
echo "============================================"
echo "Results: ${PASS} passed, ${FAIL} failed, ${WARN} warnings"
echo "============================================"

if [[ "$FAIL" -gt 0 ]]; then
  echo "COMPLIANCE STATUS: NOT COMPLIANT"
  exit 1
else
  echo "COMPLIANCE STATUS: COMPLIANT"
  exit 0
fi
```

### Phase 4 Deliverables
- [ ] Enterprise managed settings.json with allowlists and denylists
- [ ] Role-based permission tier configurations (junior, senior, admin)
- [ ] Usage analytics collection and weekly reporting scripts
- [ ] Cost tracking framework with budget alerting
- [ ] Compliance verification script
- [ ] Audit logging pipeline

---

## Phase 5: Training & Onboarding (4 hours)

### Task 5.1: Create Training Curriculum

Design a structured training program:

```markdown
# Claude Code Enterprise Training Program

## Track 1: Getting Started (2 hours)
Target audience: All engineers

### Module 1.1: Introduction (30 min)
- What is Claude Code and why we use it
- How it fits into our development workflow
- Company policies and guidelines
- Quick demo of common workflows

### Module 1.2: Core Skills (30 min)
- Using /review for code review
- Using /gen-tests for test generation
- Using /deploy-check for deployment validation
- Understanding CLAUDE.md and how it guides behavior

### Module 1.3: Hands-On Lab (60 min)
- Exercise 1: Use Claude Code to review a sample PR
- Exercise 2: Generate tests for an untested module
- Exercise 3: Run a deploy-check on staging
- Exercise 4: Create a CLAUDE.md for your project

## Track 2: Advanced Usage (2 hours)
Target audience: Senior engineers and tech leads

### Module 2.1: Custom Skills (45 min)
- Creating project-specific skills
- Skill frontmatter and tool restrictions
- Sharing skills across the team

### Module 2.2: Hooks and Automation (45 min)
- Understanding the hook lifecycle
- Creating custom enforcement hooks
- CI/CD integration patterns

### Module 2.3: Subagents and Orchestration (30 min)
- When to use subagents
- Creating specialized agents
- Multi-agent workflows

## Track 3: Administration (1 hour)
Target audience: Platform team and engineering managers

### Module 3.1: Governance (30 min)
- Managed settings configuration
- Permission management
- Compliance monitoring

### Module 3.2: Metrics and Optimization (30 min)
- Reading usage reports
- Cost optimization strategies
- Measuring ROI
```

### Task 5.2: Create Internal Documentation

Create a CLAUDE.md template for teams to customize:

```markdown
# CLAUDE.md Template for [Team Name]

## Team Context
[Describe your team, domain, and primary responsibilities]

## Project Overview
[Brief description of the project, its purpose, and users]

## Architecture
[High-level architecture description and key components]

## Technology Stack
- **Language**: [e.g., TypeScript 5.x]
- **Framework**: [e.g., NestJS 10.x]
- **Database**: [e.g., PostgreSQL 15]
- **Cache**: [e.g., Redis 7]
- **Cloud**: [e.g., AWS (ECS, RDS, ElastiCache)]

## Code Organization
\`\`\`
src/
  modules/         # Feature modules
  common/          # Shared utilities
  config/          # Configuration
  database/        # Database entities and migrations
  tests/           # Test files mirror src/ structure
\`\`\`

## Team Conventions
[List team-specific conventions beyond the org-wide standards]

### Naming Conventions
- [Describe team-specific naming patterns]

### File Organization
- [Describe where new files should go]

### Common Patterns
- [List patterns used in this codebase, e.g., Repository Pattern, CQRS]

## Key Dependencies
- [Important internal libraries and how to use them]
- [Important external libraries and any wrappers]

## Development Workflow
1. [Step-by-step workflow for this team]
2. [Include branch strategy, review process, etc.]

## Known Gotchas
- [List things that commonly trip people up]
- [Include workarounds and explanations]

## Contacts
- **Tech Lead**: [name]
- **Product Owner**: [name]
- **On-Call**: [rotation link]
```

### Task 5.3: Best Practices Guide

```markdown
# Claude Code Best Practices at Acme Corp

## Effective Prompting

### Be Specific
Instead of:
> "Fix the bug"

Say:
> "The /api/users endpoint returns 500 when the email field contains
> special characters. The error originates in src/validators/email.ts.
> Fix the validation logic to handle special characters per RFC 5322."

### Provide Context
Instead of:
> "Add authentication"

Say:
> "Add JWT authentication to the /api/orders endpoints using our
> existing AuthService in src/common/auth.service.ts. Follow the
> same pattern used in src/modules/users/users.controller.ts."

### Iterate Incrementally
1. Start with a clear, scoped request
2. Review the output
3. Ask for specific adjustments
4. Verify with tests before moving on

## Security Guidelines

### Always Do
- Review generated code before committing
- Run security scans on all changes
- Keep secrets in environment variables
- Use parameterized queries for database access

### Never Do
- Paste secrets or credentials into prompts
- Accept generated code without review
- Bypass security hooks
- Use Claude Code for production database access

## Cost Optimization

### Use the Right Model
- **Haiku**: Simple questions, quick lookups, formatting
- **Sonnet**: Most development tasks, code generation, review
- **Opus**: Complex architecture decisions, security audits

### Efficient Sessions
- Provide relevant context upfront to reduce back-and-forth
- Use CLAUDE.md to avoid repeating project context
- Use skills for repetitive workflows
- Close sessions when done (do not leave idle)

## Quality Assurance

### Human Review Required
All Claude Code output must be reviewed by a human before:
- Merging to main branch
- Deploying to production
- Modifying authentication or authorization logic
- Changing database schemas
- Updating infrastructure configuration

### Testing Standards
- All generated code must have corresponding tests
- Run the full test suite after changes
- Verify edge cases manually for critical paths
```

### Task 5.4: Support Process

```markdown
# Claude Code Support Process

## Self-Service (Tier 0)
- Internal documentation wiki
- Best practices guide
- FAQ document
- Recorded training sessions

## Team Champions (Tier 1)
Each team has a designated Claude Code champion who:
- Answers day-to-day questions
- Helps with CLAUDE.md customization
- Shares tips and tricks
- Escalates issues to platform team

### Champions Roster
| Team          | Champion       | Slack Channel          |
|---------------|----------------|------------------------|
| Platform      | [Name]         | #claude-platform       |
| Payments      | [Name]         | #claude-payments       |
| Growth        | [Name]         | #claude-growth         |
| Infrastructure| [Name]         | #claude-infra          |

## Platform Team (Tier 2)
For issues that champions cannot resolve:
- Plugin bugs or feature requests
- Settings and permission changes
- CI/CD workflow issues
- Performance problems

**Contact**: #claude-code-support on Slack
**SLA**: 4-hour response during business hours

## Vendor Support (Tier 3)
For issues requiring Anthropic support:
- API errors or outages
- Model behavior concerns
- Billing questions
- Feature requests

**Contact**: Platform team escalates via enterprise support portal
```

### Phase 5 Deliverables
- [ ] Structured training curriculum (3 tracks, 5 hours total)
- [ ] CLAUDE.md template for team customization
- [ ] Best practices guide with prompting, security, and cost optimization
- [ ] Support process documentation with tiered escalation
- [ ] Champions program definition and roster template
- [ ] Training lab exercises with sample projects

---

## Phase 6: Measurement & Optimization (4 hours)

### Task 6.1: Define Adoption Metrics

```markdown
## Adoption Metrics Dashboard

### Primary Metrics

| Metric                    | Target     | Measurement Method               |
|---------------------------|------------|----------------------------------|
| Active users (weekly)     | 90% of eng | Audit log unique users           |
| Sessions per user (daily) | 3+         | Audit log session count          |
| Skill usage rate          | 5+ per day | Audit log skill invocations      |
| CLAUDE.md coverage        | 100% repos | Git scan for CLAUDE.md files     |

### Engagement Metrics

| Metric                    | Target     | Measurement Method               |
|---------------------------|------------|----------------------------------|
| Training completion       | 100%       | LMS completion records           |
| Support tickets (weekly)  | Declining  | Ticketing system                 |
| Champion consultations    | Stable     | Champion activity log            |
| Skill contributions       | Growing    | Plugin repository PRs            |
```

### Task 6.2: Velocity Improvement Tracking

```markdown
## Velocity Metrics

### PR Cycle Time
Measure time from PR open to merge, comparing before and after deployment.

**Baseline** (pre-Claude Code):
- Mean PR cycle time: [X hours]
- Median PR cycle time: [X hours]
- P95 PR cycle time: [X hours]

**Current** (with Claude Code):
- Mean PR cycle time: [X hours] ([X%] change)
- Median PR cycle time: [X hours] ([X%] change)
- P95 PR cycle time: [X hours] ([X%] change)

### Code Review Turnaround
- Time to first review comment (automated)
- Time to human review after automated review
- Number of review cycles before approval

### Development Velocity
- Story points completed per sprint (team average)
- PRs merged per developer per week
- Time from ticket creation to PR submission
```

### Task 6.3: Quality Metrics

```markdown
## Quality Metrics

### Bug Rates
| Metric                         | Baseline | Target  | Current |
|--------------------------------|----------|---------|---------|
| Bugs per 1000 lines of code   | [X]      | [X/2]   | [X]     |
| Bugs found in code review     | [X%]     | [+20%]  | [X%]    |
| Bugs reaching production      | [X/mo]   | [X/2]   | [X/mo]  |
| Security vulnerabilities/mo   | [X]      | [X/4]   | [X]     |

### Test Coverage
| Metric                         | Baseline | Target  | Current |
|--------------------------------|----------|---------|---------|
| Overall test coverage          | [X%]     | 80%+    | [X%]    |
| New code coverage              | [X%]     | 90%+    | [X%]    |
| Critical path coverage         | [X%]     | 95%+    | [X%]    |

### Code Quality
| Metric                         | Baseline | Target  | Current |
|--------------------------------|----------|---------|---------|
| Avg cyclomatic complexity      | [X]      | <10     | [X]     |
| Linting violations per PR      | [X]      | 0       | [X]     |
| Documentation coverage         | [X%]     | 90%+    | [X%]    |
```

### Task 6.4: ROI Calculation

```markdown
## ROI Calculation Framework

### Costs

| Cost Category              | Monthly Amount |
|----------------------------|----------------|
| Anthropic API usage        | $[X]           |
| Platform team maintenance  | $[X] (FTE%)    |
| Training time (amortized)  | $[X]           |
| Infrastructure (CI/CD)     | $[X]           |
| **Total Monthly Cost**     | **$[X]**       |

### Savings

| Savings Category                      | Monthly Amount |
|---------------------------------------|----------------|
| Developer time saved (code review)    | $[X]           |
| Developer time saved (test writing)   | $[X]           |
| Developer time saved (documentation)  | $[X]           |
| Reduced bug fix costs                 | $[X]           |
| Reduced security incident costs       | $[X]           |
| Faster onboarding (reduced ramp time) | $[X]           |
| **Total Monthly Savings**             | **$[X]**       |

### ROI Formula

ROI = ((Total Monthly Savings - Total Monthly Cost) / Total Monthly Cost) x 100

### Time Savings Calculation

Average developer cost: $[X]/hour

| Activity            | Before (hrs/wk) | After (hrs/wk) | Savings/wk | Monthly $ |
|---------------------|------------------|-----------------|------------|-----------|
| Code review         | [X]              | [X]             | [X]        | $[X]      |
| Test writing        | [X]              | [X]             | [X]        | $[X]      |
| Documentation       | [X]              | [X]             | [X]        | $[X]      |
| Bug investigation   | [X]              | [X]             | [X]        | $[X]      |
| Onboarding support  | [X]              | [X]             | [X]        | $[X]      |
| **Total per dev**   | **[X]**          | **[X]**         | **[X]**    | **$[X]**  |

Total monthly savings = savings per dev x number of developers
```

### Task 6.5: Continuous Improvement Process

```markdown
## Continuous Improvement Cycle

### Monthly Review
1. **Metrics Review** (30 min)
   - Review adoption dashboard
   - Analyze velocity trends
   - Check quality metrics
   - Review cost data

2. **Feedback Collection** (ongoing)
   - Developer satisfaction survey (quarterly)
   - Champion feedback sessions (monthly)
   - Support ticket analysis (weekly)
   - Skill usage analytics (weekly)

3. **Improvement Actions**
   - Update CLAUDE.md based on common issues
   - Add new skills based on repeated requests
   - Refine hooks based on false positive rates
   - Adjust permissions based on security incidents
   - Update training based on support patterns

### Quarterly Planning
1. Review ROI and present to leadership
2. Identify top 3 improvement opportunities
3. Plan skill/hook development for next quarter
4. Update training materials
5. Adjust budget and cost controls
6. Update 6-month roadmap

### 6-Month Roadmap Template

| Quarter | Focus Area              | Key Initiatives                    |
|---------|-------------------------|------------------------------------|
| Q1      | Foundation              | Plugin, CI/CD, pilot rollout       |
| Q2      | Scale                   | Company-wide rollout, training     |
| Q3      | Optimization            | Custom skills, advanced workflows  |
| Q4      | Innovation              | MCP servers, multi-agent pipelines |
```

### Phase 6 Deliverables
- [ ] Adoption metrics dashboard definition
- [ ] Velocity improvement tracking with baselines
- [ ] Quality metrics framework (bugs, coverage, code quality)
- [ ] ROI calculation spreadsheet with formulas
- [ ] Continuous improvement process documentation
- [ ] 6-month roadmap

---

## Final Deliverables Checklist

### 1. Enterprise Plugin (production-ready)
- [ ] Organization-wide CLAUDE.md with complete coding standards
- [ ] 10+ standardized skills covering review, deploy, test, security, docs, and more
- [ ] 5+ enforcement hooks for security, linting, audit, permissions, notifications
- [ ] 4+ specialized subagents for security, testing, documentation, analysis
- [ ] Installation and update scripts
- [ ] Plugin versioning and changelog

### 2. CI/CD Pipelines (ready for all repositories)
- [ ] Automated code review workflow (GitHub Actions)
- [ ] Security scanning workflow with blocking on critical issues
- [ ] Test coverage analysis workflow
- [ ] Deployment gate workflow with GO/NO-GO decision
- [ ] Reusable workflow templates for easy adoption

### 3. Governance Framework (policies and monitoring)
- [ ] Enterprise managed settings.json
- [ ] Role-based permission tiers (junior, senior, admin)
- [ ] Audit logging pipeline
- [ ] Cost tracking and budget alerting
- [ ] Compliance verification script

### 4. Training Materials (complete onboarding program)
- [ ] 3-track training curriculum (5 hours total)
- [ ] Hands-on lab exercises
- [ ] CLAUDE.md templates for team customization
- [ ] Best practices guide
- [ ] Support process with tiered escalation
- [ ] Champions program definition

### 5. Metrics Dashboard (measurement and tracking)
- [ ] Adoption metrics with targets
- [ ] Velocity improvement tracking
- [ ] Quality metrics framework
- [ ] ROI calculation model
- [ ] Weekly/monthly reporting scripts

### 6. Roadmap (continuous improvement plan)
- [ ] 6-month roadmap with quarterly milestones
- [ ] Continuous improvement process
- [ ] Feedback collection mechanisms
- [ ] Quarterly review cadence

---

## Success Criteria

### Quantitative Targets
- [ ] 90%+ team adoption within 3 months of rollout
- [ ] 2x+ improvement in PR cycle time
- [ ] 50%+ reduction in bugs reaching production
- [ ] 80%+ test coverage on all active projects
- [ ] Positive ROI within first quarter
- [ ] Zero security incidents caused by Claude Code

### Qualitative Targets
- [ ] 90%+ developer satisfaction score (quarterly survey)
- [ ] All teams have customized CLAUDE.md files
- [ ] Champions program active and effective
- [ ] Support tickets trending downward
- [ ] Leadership endorsement for continued investment

---

## Verification Checklist

### Plugin Verification
- [ ] Plugin installs cleanly on a fresh developer machine
- [ ] All skills invoke correctly and produce expected output
- [ ] Hooks fire at the correct lifecycle points
- [ ] Subagents delegate and return results properly
- [ ] Settings enforcement prevents disallowed operations
- [ ] Audit logs capture all tool invocations

### CI/CD Verification
- [ ] Code review workflow triggers on PR open/update
- [ ] Security scan blocks PRs with critical findings
- [ ] Test coverage workflow reports gaps accurately
- [ ] Deploy gate produces correct GO/NO-GO decisions
- [ ] All workflows complete within timeout limits

### Governance Verification
- [ ] Managed settings apply to all developer machines
- [ ] Role-based permissions restrict operations correctly
- [ ] Audit logs are complete and parseable
- [ ] Cost reports generate accurately
- [ ] Compliance script passes on all machines

### Training Verification
- [ ] All training modules have been reviewed by stakeholders
- [ ] Lab exercises work correctly end-to-end
- [ ] CLAUDE.md template is clear and complete
- [ ] Support process is documented and communicated
- [ ] Champions have been identified for all teams

### Metrics Verification
- [ ] Baseline measurements recorded before rollout
- [ ] Dashboards display real-time data accurately
- [ ] ROI calculation uses verified inputs
- [ ] Reporting scripts run on schedule
- [ ] Improvement actions are tracked and followed up

---

## Rollout Timeline

| Week    | Phase              | Activities                                    |
|---------|--------------------|-----------------------------------------------|
| 1-2     | Build              | Plugin development, managed settings          |
| 3-4     | Integrate          | CI/CD workflows, governance framework         |
| 5-6     | Pilot              | 10 engineers (2 teams), collect feedback       |
| 7-8     | Iterate            | Fix issues, refine based on pilot feedback    |
| 9-10    | Train              | Training materials, champion onboarding       |
| 11-12   | Rollout            | Company-wide deployment (phased by team)      |
| 13+     | Optimize           | Metrics tracking, continuous improvement      |

---

## Tips for Success

### Executive Sponsorship
Secure buy-in from engineering leadership early. Present the business case
with projected ROI and risk mitigation. Regular updates to leadership keep
the program funded and supported.

### Start Small, Scale Fast
Begin with a pilot of 2 teams (10 engineers) who are enthusiastic early
adopters. Their success stories and feedback will drive adoption across the
organization far more effectively than a top-down mandate.

### Measure Everything
Establish baselines before rollout. Without pre-deployment metrics, you
cannot demonstrate improvement. Track PR cycle time, bug rates, test
coverage, and developer satisfaction before day one.

### Invest in Champions
Your champions program is the most important lever for adoption. Champions
provide peer support, share context-specific tips, and serve as the first
line of defense for questions and issues.

### Iterate on CLAUDE.md
Your organization-wide CLAUDE.md is a living document. Review it monthly.
When you see recurring issues in code reviews or support tickets, update the
CLAUDE.md to prevent them. The more specific and accurate your CLAUDE.md,
the better Claude Code performs.

### Balance Automation with Autonomy
Enforce critical standards (security, testing) but avoid over-constraining
developers. Too many restrictions create friction and drive people away from
the tool. Focus hooks and gates on high-impact checks.

---

**Estimated time**: 20-40 hours
**Difficulty**: Expert
**Skills practiced**: Enterprise architecture, plugin development, CI/CD, governance, training design, metrics, change management
**Outcome**: Production-ready enterprise Claude Code deployment serving 50+ engineers with measurable improvements in velocity, quality, and developer satisfaction
