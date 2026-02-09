# Exercise 13: IDE Integration
**Time**: 45 minutes
**Prerequisites**: Lessons 75-80

## Objective
Configure Claude Code inside VS Code, practice the inline diff workflow, and build efficient habits with @-mentions, multi-conversation management, and keyboard shortcuts.

## Setup
```bash
mkdir -p ~/ide-lab/src ~/ide-lab/tests ~/ide-lab/docs
cd ~/ide-lab
git init
cat > src/calculator.ts << 'EOF'
export class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }

  subtract(a: number, b: number): number {
    return a - b;
  }

  multiply(a: number, b: number): number {
    return a * b;
  }

  divide(a: number, b: number): number {
    return a / b; // no zero-division guard
  }
}
EOF
cat > src/formatter.ts << 'EOF'
export function formatCurrency(amount: number, currency: string): string {
  return currency + amount.toFixed(2);
}

export function formatPercentage(value: number): string {
  return (value * 100).toFixed(1) + "%";
}
EOF
cat > tests/calculator.test.ts << 'EOF'
import { Calculator } from "../src/calculator";

const calc = new Calculator();
console.assert(calc.add(2, 3) === 5, "add failed");
console.assert(calc.subtract(5, 3) === 2, "subtract failed");
console.log("Basic tests passed");
EOF
npm init -y
npx tsc --init --outDir dist --rootDir . --strict true 2>/dev/null || true
git add -A && git commit -m "Initial calculator project"
```

Open the project in VS Code:
```bash
code ~/ide-lab
```

## Tasks

### Task 1: Set Up the VS Code Extension (10 min)
Install and configure the Claude Code VS Code extension:

1. Open the VS Code Extensions panel (`Ctrl+Shift+X`).
2. Search for "Claude Code" and install the official Anthropic extension.
3. Open the Claude Code panel from the sidebar (look for the Claude icon) or use the command palette (`Ctrl+Shift+P` then type "Claude Code").
4. Verify the extension connects by sending a simple prompt: `"What files are in this project?"`
5. Confirm the response lists `src/calculator.ts`, `src/formatter.ts`, and `tests/calculator.test.ts`.

**Verify**: The Claude Code panel is visible in the VS Code sidebar, responds to prompts, and correctly identifies the project files.

---

### Task 2: Use Inline Diff Review (10 min)
Ask Claude to make a change and practice the inline diff review workflow:

1. In the Claude Code panel, type: `"Add a zero-division guard to the divide method in src/calculator.ts that throws an Error if b is zero."`
2. Claude will propose a diff. Instead of auto-accepting, review it inline:
   - The modified lines appear highlighted in the editor.
   - Green lines are additions; red lines are deletions.
   - Use the accept/reject buttons on each change hunk.
3. Accept the zero-division guard change.
4. Now ask: `"Add a modulo method to the Calculator class."`
5. This time, reject the diff to practice declining a suggestion.

**Verify**: Run `git diff src/calculator.ts` and confirm only the zero-division guard was applied, not the modulo method. The divide method should now throw an `Error` when `b === 0`.

---

### Task 3: Reference Files with @-mentions (10 min)
Practice using @-mentions to give Claude precise file context:

1. In the Claude Code panel, type: `@src/formatter.ts Add locale-aware currency formatting using Intl.NumberFormat. Keep the existing functions and add a new formatCurrencyLocale function.`
2. Observe that Claude scopes its analysis and changes to the referenced file.
3. Accept the proposed change.
4. Now use a multi-file reference: `@src/calculator.ts @tests/calculator.test.ts Add tests for the divide method including a test that verifies the zero-division error is thrown.`
5. Accept the test additions.

**Verify**: Run `npx ts-node tests/calculator.test.ts 2>/dev/null || node tests/calculator.test.ts` (or review the file manually) and confirm the new tests reference the divide method and zero-division case.

---

### Task 4: Manage Multiple Conversations (10 min)
Practice working with multiple Claude Code conversations simultaneously:

1. Start a new conversation using the `+` button or command palette (`Claude Code: New Conversation`).
2. In conversation 1, ask: `@src/calculator.ts Explain the design of this Calculator class and suggest improvements.`
3. In conversation 2, ask: `@src/formatter.ts Write JSDoc comments for every exported function.`
4. Switch between conversations using the conversation list in the Claude Code panel.
5. Accept the JSDoc changes from conversation 2 while keeping conversation 1 as a reference discussion.

Observe that each conversation maintains its own context and history independently.

**Verify**: `src/formatter.ts` now has JSDoc comments on each function. Conversation 1's suggestions were not auto-applied (it was advisory only).

---

### Task 5: Configure Keyboard Shortcuts (5 min)
Set up keyboard shortcuts to speed up your Claude Code workflow:

1. Open Keyboard Shortcuts: `Ctrl+K Ctrl+S` (or `Cmd+K Cmd+S` on macOS).
2. Search for "Claude" to see all available Claude Code commands.
3. Assign the following shortcuts (or note the defaults):
   - **Open Claude Code panel**: Bind to `Ctrl+Shift+L` (or your preference).
   - **Accept diff**: Note the default keybinding for accepting inline suggestions.
   - **Reject diff**: Note the default keybinding for rejecting inline suggestions.
   - **New conversation**: Bind to `Ctrl+Shift+N` (or your preference).
4. Test each shortcut to verify it works.
5. Create a cheat sheet file:

```bash
cat > ~/ide-lab/docs/shortcuts.txt << 'EOF'
Claude Code VS Code Shortcuts
==============================
Open Panel:        Ctrl+Shift+L
New Conversation:  Ctrl+Shift+N
Accept Diff:       [your binding]
Reject Diff:       [your binding]
Toggle Inline:     [your binding]
EOF
```

**Verify**: The shortcut file exists at `~/ide-lab/docs/shortcuts.txt` and all listed shortcuts are functional in your VS Code instance.

## Completion Criteria
- [ ] Claude Code VS Code extension is installed and responds to prompts
- [ ] Inline diff review accepted one change and rejected another correctly
- [ ] @-mentions scoped Claude's changes to the specified files
- [ ] Multiple conversations managed independently with separate contexts
- [ ] Keyboard shortcuts configured and documented in shortcuts.txt
