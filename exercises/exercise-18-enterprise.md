# Exercise 18: Enterprise Deployment

**Time**: 1.5 hours
**Prerequisites**: Lessons 97-100

## Objective
Configure Claude Code for secure, policy-compliant team deployment with managed settings, permissions, and monitoring.

## Setup
```bash
mkdir ~/enterprise-deploy-lab && cd ~/enterprise-deploy-lab && git init
mkdir -p src/{api,auth,db} tests docs .claude
echo '{ "name": "enterprise-app", "version": "2.0.0", "private": true }' > package.json
echo "export function getUser(id) { return db.query('SELECT * FROM users WHERE id = ?', [id]); }" > src/db/users.js
echo "export function authenticate(token) { return jwt.verify(token, process.env.SECRET_KEY); }" > src/auth/auth.js
echo "export function handleRequest(req, res) { res.json({ status: 'ok' }); }" > src/api/handler.js
git add -A && git commit -m "Initial enterprise project"
```

## Tasks
### Task 1: Create Enterprise Managed Settings (20 min)
Create `.claude/settings.json` with organization-wide policies:
```json
{
  "permissions": {
    "allow": ["Read", "Grep", "Glob", "WebSearch"],
    "deny": ["Bash(rm -rf *)", "Bash(curl * | bash)", "Bash(chmod 777 *)"]
  }
}
```
Then create `docs/user-settings-template.json` for team members:
```json
{
  "permissions": {
    "allow": ["Bash(npm test)", "Bash(npm run lint)", "Bash(npm run build)", "Bash(git *)"]
  },
  "preferences": { "notifChannel": "terminal", "verbose": false }
}
```
**Verify**: Both files contain valid JSON. Run `cat .claude/settings.json | claude -p "Is this valid Claude Code settings? List any issues."`
---
### Task 2: Set Up Permission Policies for a Team (15 min)
```bash
mkdir -p docs/policies
```
Ask Claude to create three role-based policy files in `docs/policies/`:
- **junior-developer.json**: Read files, run tests/linters only. Deny arbitrary bash, file deletion, git config changes.
- **senior-developer.json**: Junior permissions plus builds, migrations, push to non-main branches. Deny force-push.
- **tech-lead.json**: Senior permissions plus CI/CD config changes, deploy scripts, production logs. Still deny `rm -rf` and force-push to main.

**Verify**: `ls docs/policies/` shows all three files. Ask Claude: "Read all files in docs/policies/ and verify junior < senior < tech-lead permission hierarchy is consistent."
---
### Task 3: Configure Security Hooks for Compliance (20 min)
Create a pre-commit hook that blocks hardcoded secrets:
```bash
cat > .git/hooks/pre-commit << 'HOOK'
#!/bin/bash
SECRETS=$(grep -rniE '(password|secret|api_key|token)\s*[:=]\s*"[^"]+"' --include="*.js" src/ 2>/dev/null)
if [ -n "$SECRETS" ]; then
  echo "BLOCKED: Hardcoded secrets detected:" && echo "$SECRETS" && exit 1
fi
echo "Compliance checks passed."
HOOK
chmod +x .git/hooks/pre-commit
```
Test it:
```bash
echo 'const API_KEY = "sk-1234567890";' > src/api/config.js
git add src/api/config.js && git commit -m "Add config"   # Should FAIL

echo 'const API_KEY = process.env.API_KEY;' > src/api/config.js
git add src/api/config.js && git commit -m "Add config"   # Should PASS
```
**Verify**: First commit is blocked. Second succeeds. `git log --oneline -1` shows the safe commit.
---
### Task 4: Create an Onboarding CLAUDE.md (15 min)
Ask Claude to generate a `CLAUDE.md` at the project root covering:
1. **Project overview**: Enterprise Node.js API with auth, database, and handler layers
2. **Code conventions**: ESM imports, JSDoc on public functions, custom AppError class, no console.log
3. **Testing**: Every function needs a test, use assert module, tests mirror src/ structure
4. **Security**: No hardcoded secrets, parameterized SQL, validate input, no eval()
5. **Git workflow**: Branch from main, conventional commits, PRs require review, no force-push
6. **Claude instructions**: Always include JSDoc, always add tests, run npm test, flag security concerns

**Verify**: Start a new Claude session in the project. Ask "What are the code conventions?" -- Claude should reference the CLAUDE.md rules.
---
### Task 5: Set Up Usage Monitoring and Cost Tracking (20 min)
```bash
mkdir -p scripts
```
Ask Claude to create two scripts:
- **scripts/track-usage.sh**: Parse Claude session logs, extract date/model/tokens/cost, append to `~/claude-usage-log.csv`, print today's summary.
- **scripts/usage-report.sh**: Read the CSV, calculate weekly cost, daily average, most-used model, total sessions. Flag days exceeding $10.

```bash
chmod +x scripts/track-usage.sh scripts/usage-report.sh
```
**Verify**: Both scripts are executable. `bash scripts/track-usage.sh` runs without errors (or gives a clear "no logs found" message). `bash scripts/usage-report.sh` outputs a formatted report or creates headers in an empty CSV.

## Completion Criteria
- [ ] Managed settings at `.claude/settings.json` with allow/deny lists
- [ ] Three role-based permission policies with consistent hierarchy
- [ ] Pre-commit hook blocks secrets and passes clean code
- [ ] CLAUDE.md covers conventions, security, and Claude-specific instructions
- [ ] Usage tracking and reporting scripts are functional

## Bonus Challenges
- **30-Day Rollout Plan**: Write `docs/rollout-plan.md` for a 20-person team: Week 1 -- 3 pilot tech leads with Opus; Week 2 -- 8 senior devs with Sonnet default; Week 3 -- full team with role-based policies; Week 4 -- review metrics, adjust, document lessons. Include success criteria for each phase.
- **Policy Test Suite**: Write tests that simulate allowed/denied operations for each role and output a pass/fail report.
