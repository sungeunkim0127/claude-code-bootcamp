# Exercise 11: Building Custom Subagents
**Time**: 1.5 hours
**Prerequisites**: Lessons 63-70

## Objective
Design, configure, and orchestrate custom subagents that divide complex tasks into specialized, tool-restricted units of work.

## Setup
```bash
mkdir -p ~/subagent-lab/src ~/subagent-lab/tests ~/subagent-lab/reports
cd ~/subagent-lab
git init
cat > src/app.js << 'EOF'
const express = require("express");
const app = express();

function calculateTotal(items) {
  let total = 0;
  for (let i = 0; i <= items.length; i++) { // off-by-one bug
    total += items[i].price * items[i].qty;
  }
  return total;
}

app.get("/total", (req, res) => {
  const items = JSON.parse(req.query.items);
  res.json({ total: calculateTotal(items) });
});

app.listen(3000);
module.exports = { calculateTotal };
EOF
npm init -y && npm install express
```

## Tasks

### Task 1: Create a Code Analyzer Subagent (15 min)
Create a `SUBAGENT.md` file at `~/subagent-lab/subagents/code-analyzer/SUBAGENT.md` that defines a read-only code analysis subagent.

The file must include:
- A clear role description: "You are a static code analyzer. You examine source files for bugs, code smells, and style issues."
- An output format specification requiring structured JSON reports with fields: `file`, `line`, `severity`, `message`.
- An explicit instruction to never modify source files.

```bash
mkdir -p ~/subagent-lab/subagents/code-analyzer
```

**Verify**: Run `cat ~/subagent-lab/subagents/code-analyzer/SUBAGENT.md` and confirm it contains the role, output format, and read-only constraint.

---

### Task 2: Configure Tool Restrictions (15 min)
Add a `.claude/settings.json` inside the `code-analyzer` subagent directory that restricts the subagent to read-only tools.

The settings must:
- Allow only `Read`, `Glob`, and `Grep` tools.
- Deny `Write`, `Edit`, `Bash`, and `NotebookEdit` tools.
- Set `SUBAGENT.md` as the system prompt source.

```bash
mkdir -p ~/subagent-lab/subagents/code-analyzer/.claude
```

**Verify**: Run `cat ~/subagent-lab/subagents/code-analyzer/.claude/settings.json` and confirm only read tools are in the allowed list.

---

### Task 3: Set Up Model Selection Logic (15 min)
Edit the `SUBAGENT.md` to include model selection guidance:

- For simple single-file linting tasks (fewer than 100 lines), the subagent should request Haiku for speed and cost efficiency.
- For complex multi-file analysis or architectural review, the subagent should request Sonnet for deeper reasoning.

Add a section titled `## Model Selection` to the `SUBAGENT.md` with these rules and example trigger phrases for each tier.

**Verify**: The `SUBAGENT.md` contains a `## Model Selection` heading with explicit Haiku and Sonnet criteria.

---

### Task 4: Test the Code Analyzer Subagent (20 min)
Invoke the code analyzer subagent against `src/app.js` using Claude Code.

```bash
cd ~/subagent-lab
claude --print "Using the code-analyzer subagent instructions in subagents/code-analyzer/SUBAGENT.md, analyze src/app.js and produce a JSON report."
```

Evaluate the output:
- Does it identify the off-by-one bug on the loop boundary?
- Is the report in the specified JSON format?
- Did the subagent refrain from modifying any files?

**Verify**: Run `git status` in `~/subagent-lab` and confirm no source files were modified. Review the JSON output for at least one finding referencing the loop bug.

---

### Task 5: Create a Fix Suggester Subagent (15 min)
Create a second subagent at `~/subagent-lab/subagents/fix-suggester/SUBAGENT.md` that:

- Accepts JSON reports produced by the code analyzer as input.
- Generates patch-style fix suggestions (unified diff format) for each finding.
- Is allowed to use `Read` and `Bash` (for running `diff`) but NOT `Write` or `Edit`.

```bash
mkdir -p ~/subagent-lab/subagents/fix-suggester/.claude
```

**Verify**: Run `cat ~/subagent-lab/subagents/fix-suggester/SUBAGENT.md` and confirm it references the analyzer's JSON format and outputs diffs.

---

### Task 6: Orchestrate Both Subagents (15 min)
Create a top-level `CLAUDE.md` in `~/subagent-lab/` that instructs Claude to:

1. First invoke the code-analyzer subagent on all files in `src/`.
2. Pass the resulting JSON report to the fix-suggester subagent.
3. Present the combined output: findings plus suggested patches.

Test the full pipeline:
```bash
cd ~/subagent-lab
claude --print "Run the full analysis and fix-suggestion pipeline on the src/ directory."
```

**Verify**: The output contains both a JSON analysis report and at least one unified diff suggestion for the off-by-one bug.

## Completion Criteria
- [ ] Code analyzer `SUBAGENT.md` exists with role, output format, and read-only constraint
- [ ] Tool restrictions properly configured to allow only read operations
- [ ] Model selection section defines Haiku and Sonnet usage criteria
- [ ] Code analyzer correctly identifies bugs without modifying files
- [ ] Fix suggester subagent produces diff-format patches from analyzer output
- [ ] Orchestration `CLAUDE.md` chains both subagents into a pipeline
