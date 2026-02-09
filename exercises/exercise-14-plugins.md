# Exercise 14: Building Your First Plugin
**Time**: 2 hours
**Prerequisites**: Lessons 81-86

## Objective
Build a complete Claude Code plugin from scratch, including a skill, a hook, local testing, and documentation.

## Setup
```bash
mkdir -p ~/plugin-lab/my-plugin/skills ~/plugin-lab/my-plugin/hooks
mkdir -p ~/plugin-lab/test-project/src
cd ~/plugin-lab/test-project
git init
cat > src/index.js << 'EOF'
function greet(name) {
  console.log("Hello, " + name);
}

function farewell(name) {
  console.log("Goodbye, " + name);
}

greet("World");
farewell("World");
EOF
cat > src/utils.js << 'EOF'
function isEmpty(value) {
  return value === null || value === undefined || value === "";
}

function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

module.exports = { isEmpty, capitalize };
EOF
git add -A && git commit -m "Initial test project"
```

## Tasks

### Task 1: Create the Plugin Manifest (20 min)
Create the `plugin.json` manifest file that defines your plugin's metadata and capabilities.

```bash
cd ~/plugin-lab/my-plugin
```

Create `~/plugin-lab/my-plugin/plugin.json` with the following structure:
- `name`: `"code-quality-plugin"`
- `version`: `"1.0.0"`
- `description`: A short description of the plugin's purpose (code quality checks and conventions enforcement).
- `author`: Your name or handle.
- `skills`: An array referencing the skill file(s) you will create in Task 2.
- `hooks`: An array referencing the hook file(s) you will create in Task 3.
- `permissions`: Declare that the plugin requires `Read`, `Glob`, `Grep`, and `Bash` tools.

Ensure the JSON is valid:
```bash
node -e "JSON.parse(require('fs').readFileSync('plugin.json','utf8')); console.log('Valid JSON')"
```

**Verify**: The command above prints `Valid JSON` with no errors. The file contains all required fields (`name`, `version`, `description`, `skills`, `hooks`, `permissions`).

---

### Task 2: Add a Skill to the Plugin (25 min)
Create a skill that performs a code quality review. The skill file lives at `~/plugin-lab/my-plugin/skills/review.md`.

The skill must define:
- **Trigger**: The skill activates when the user types `/review` or asks for a "code quality review."
- **Behavior**: The skill scans all `.js` files in the current project using `Glob` and `Read`, then checks for:
  1. Functions longer than 20 lines.
  2. Missing `module.exports` in files that define functions.
  3. Use of `var` instead of `const` or `let`.
  4. Console.log statements that should be removed in production.
- **Output format**: A markdown table with columns: File, Line, Issue, Severity (low/medium/high).

Example output the skill should produce:
```
| File          | Line | Issue                        | Severity |
|---------------|------|------------------------------|----------|
| src/index.js  | 2    | console.log in production    | medium   |
| src/index.js  | 6    | console.log in production    | medium   |
| src/index.js  | -    | Missing module.exports       | low      |
```

**Verify**: Run `cat ~/plugin-lab/my-plugin/skills/review.md` and confirm it contains the trigger definition, all four check rules, and the output format specification.

---

### Task 3: Add a Hook to the Plugin (25 min)
Create a hook that runs automatically before every commit. The hook file lives at `~/plugin-lab/my-plugin/hooks/pre-commit-check.sh`.

The hook must:
1. Scan staged `.js` files for `debugger` statements and `TODO` comments.
2. If any `debugger` statements are found, block the commit and print a clear error message listing the offending files and line numbers.
3. If `TODO` comments are found, print a warning (but allow the commit to proceed).
4. Exit with code 0 on success or code 1 if `debugger` statements were found.

```bash
cat > ~/plugin-lab/my-plugin/hooks/pre-commit-check.sh << 'HOOKEOF'
#!/bin/bash
# Pre-commit hook: block debugger statements, warn on TODOs

STAGED_JS=$(git diff --cached --name-only --diff-filter=ACM | grep '\.js$')

if [ -z "$STAGED_JS" ]; then
  exit 0
fi

HAS_ERROR=0

# Check for debugger statements
for file in $STAGED_JS; do
  DEBUGGER_LINES=$(grep -n 'debugger' "$file" 2>/dev/null)
  if [ -n "$DEBUGGER_LINES" ]; then
    echo "ERROR: debugger statement found in $file:"
    echo "$DEBUGGER_LINES"
    HAS_ERROR=1
  fi
done

# Check for TODO comments (warning only)
for file in $STAGED_JS; do
  TODO_LINES=$(grep -n 'TODO' "$file" 2>/dev/null)
  if [ -n "$TODO_LINES" ]; then
    echo "WARNING: TODO found in $file:"
    echo "$TODO_LINES"
  fi
done

exit $HAS_ERROR
HOOKEOF
chmod +x ~/plugin-lab/my-plugin/hooks/pre-commit-check.sh
```

Now register the hook in `plugin.json` by ensuring the `hooks` array references `"hooks/pre-commit-check.sh"` with event type `"pre-commit"`.

**Verify**: Run `bash ~/plugin-lab/my-plugin/hooks/pre-commit-check.sh` from inside `~/plugin-lab/test-project` (after staging files) and confirm it exits cleanly with warnings about console.log but no debugger errors.

---

### Task 4: Test the Plugin Locally (25 min)
Install and test the plugin against the test project.

Step 1 -- Link the plugin to the test project:
```bash
cd ~/plugin-lab/test-project
mkdir -p .claude/plugins
ln -s ~/plugin-lab/my-plugin .claude/plugins/code-quality-plugin
```

Step 2 -- Test the skill:
```bash
cd ~/plugin-lab/test-project
claude --print "Run /review on this project"
```

Evaluate the output:
- Does it scan both `src/index.js` and `src/utils.js`?
- Does it flag the `console.log` calls in `src/index.js`?
- Does it flag the missing `module.exports` in `src/index.js`?
- Is the output in the specified markdown table format?

Step 3 -- Test the hook by adding a debugger statement:
```bash
cd ~/plugin-lab/test-project
echo "debugger;" >> src/index.js
git add src/index.js
bash .claude/plugins/code-quality-plugin/hooks/pre-commit-check.sh
echo "Exit code: $?"
```

Step 4 -- Remove the debugger and verify the hook passes:
```bash
cd ~/plugin-lab/test-project
git checkout -- src/index.js
git add src/index.js
bash .claude/plugins/code-quality-plugin/hooks/pre-commit-check.sh
echo "Exit code: $?"
```

**Verify**: The hook exits with code 1 when `debugger` is present (Step 3) and code 0 when it is absent (Step 4). The skill output contains a markdown table with findings.

---

### Task 5: Write Plugin Documentation (20 min)
Create a `README.md` at `~/plugin-lab/my-plugin/README.md` that documents the plugin for other users.

The README must include:
1. **Plugin name and description** -- one-paragraph summary.
2. **Installation** -- step-by-step instructions for linking the plugin to a project.
3. **Skills reference** -- describe the `/review` skill, its trigger, what it checks, and example output.
4. **Hooks reference** -- describe the pre-commit hook, what it blocks, what it warns about.
5. **Configuration** -- list any environment variables or settings the user can customize.
6. **Permissions** -- state which tools the plugin requires and why.

```bash
cd ~/plugin-lab/my-plugin
```

**Verify**: The README contains all six sections listed above. Run `grep -c "^##" ~/plugin-lab/my-plugin/README.md` and confirm at least 6 section headings exist.

---

### Task 6: Package and Validate the Plugin (15 min)
Perform a final validation of the entire plugin structure:

```bash
cd ~/plugin-lab/my-plugin
echo "=== Structure ==="
find . -type f | sort
echo ""
echo "=== Manifest ==="
node -e "const p=JSON.parse(require('fs').readFileSync('plugin.json','utf8')); console.log('Name:', p.name); console.log('Skills:', p.skills.length); console.log('Hooks:', p.hooks.length); console.log('Permissions:', p.permissions.join(', '));"
echo ""
echo "=== Hook executable? ==="
test -x hooks/pre-commit-check.sh && echo "Yes" || echo "No"
```

Confirm the output shows:
- At least 5 files: `plugin.json`, `skills/review.md`, `hooks/pre-commit-check.sh`, `README.md`, and any additional config.
- The manifest reports 1 skill, 1 hook, and the correct permissions.
- The hook file is executable.

**Verify**: All three validation checks pass. The plugin directory is self-contained and ready for distribution.

## Completion Criteria
- [ ] `plugin.json` is valid JSON with name, version, description, skills, hooks, and permissions
- [ ] `/review` skill defines trigger, four code quality checks, and markdown table output
- [ ] Pre-commit hook blocks `debugger` statements and warns on `TODO` comments
- [ ] Plugin tested locally: skill produces findings and hook enforces rules
- [ ] README documents all six required sections
- [ ] Final validation confirms correct structure, manifest, and file permissions
