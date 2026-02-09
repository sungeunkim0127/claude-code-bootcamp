# Exercise 17: Performance & Cost Optimization

**Time**: 45 minutes
**Prerequisites**: Lessons 93-96

## Objective
Minimize token usage, reduce tool calls, and select the right model to optimize speed and cost.

## Setup
```bash
mkdir ~/perf-optimization-lab && cd ~/perf-optimization-lab && git init
mkdir -p src/{utils,services} config
for i in $(seq 1 5); do echo "export function util$i() { return 'utility $i'; }" > src/utils/helper$i.js; done
for i in $(seq 1 3); do echo "export class Service$i { constructor() { this.name = 'svc$i'; } }" > src/services/service$i.js; done
echo "export const config = { debug: false, port: 3000 };" > config/app.js
echo '{ "name": "perf-lab", "version": "1.0.0" }' > package.json
git add -A && git commit -m "Initial setup"
```

## Tasks
### Task 1: Audit Tool Usage for Inefficiencies (10 min)
**Inefficient** -- start a session and read files one at a time:
```
You: "Read src/utils/helper1.js"
You: "Read src/utils/helper2.js"
...repeat for all 5 files...
You: "Summarize what these do"
```
That is 5 Read calls + analysis. Now start a **new session** with the optimized approach:
```
You: "Read all files in src/utils/ and summarize what each function does"
```
Claude batches the reads into fewer tool calls.

**Verify**: Run `/cost` in both sessions. The optimized session uses fewer tool calls and less total tokens.
---
### Task 2: Write Efficient Prompts (10 min)
Combine related requests into single prompts instead of sequential asks:
```
# Bad: 3 separate prompts
You: "Add a logger import to src/services/service1.js"
You: "Add logging in the constructor"
You: "Add an error handler method"

# Good: 1 prompt
You: "Update src/services/service1.js: add a logger import, logging in the constructor,
and an error handler method that logs and rethrows."
```
Similarly for searches -- ask for all exports at once rather than functions, then classes, then configs separately.

**Verify**: Run `/cost` after each approach. The single-prompt version completes the same work in fewer exchanges.
---
### Task 3: Compare Model Costs (10 min)
Switch models and run tasks at appropriate tiers:
```
You: /model haiku
You: "Rename variable 'i' to 'index' in src/utils/helper1.js"

You: /model sonnet
You: "Refactor src/services/service1.js to use dependency injection"

You: /model opus
You: "Analyze the project and design an authentication layer with JWT and RBAC"
```
Build a reference from `/cost` output:

| Task Type | Model | Rationale |
|---|---|---|
| Renames, formatting | Haiku | Fast, cheap, sufficient quality |
| Refactoring, features | Sonnet | Balances quality and cost |
| Architecture, planning | Opus | Best reasoning for complex work |

**Verify**: You used at least two models. `/cost` shows per-model breakdown.
---
### Task 4: Context Management with /compact (10 min)
Build up context, then compact it:
```
You: "Read every file in src/ and summarize each"
You: "List all function and class names across the project"
You: "What design patterns are used?"
You: /cost                    # Note total context tokens
You: /compact Focus on project structure only
You: /cost                    # Context should be smaller now
You: "Add a new AuthService in src/services/ matching existing patterns"
```
**Verify**: Context tokens after `/compact` are lower. Claude still generates a correctly structured service file.
---
### Task 5: Terminal and Indexing Optimization (5 min)
Configure environment for speed:
```bash
# Check shell startup time
time bash -i -c exit

# Review Claude settings
claude config list

# Disable verbose mode for production
claude config set --global verbose false
```
Create `.claudeignore` at project root:
```
node_modules/
dist/
build/
coverage/
*.min.js
*.map
.git/
```
**Verify**: `.claudeignore` exists. `claude config list` shows expected values. New Claude session feels responsive.

## Completion Criteria
- [ ] Demonstrated batched vs sequential reads with measurable difference
- [ ] Wrote combined prompts that reduce tool calls
- [ ] Used at least two models and documented cost differences
- [ ] Used /compact to reduce context mid-session
- [ ] Created .claudeignore with sensible exclusions

## Bonus Challenges
- A/B test the same 5 tasks on Haiku, Sonnet, and Opus -- record time, cost, and quality (1-5) in a comparison table.
- Create a prompt template library in `docs/prompt-templates/` with optimized prompts for review, refactoring, and testing.
