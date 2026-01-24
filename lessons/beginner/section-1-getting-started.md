# Section 1: Getting Started (Lessons 1-5)

## Overview
This section introduces you to Claude Code, covering installation, basic concepts, and your first interactions. By the end, you'll understand what Claude Code is, how to use it, and the fundamental workflow patterns.

---

## Lesson 1: What is Claude Code?

### Objectives
- Understand what Claude Code is and how it differs from ChatGPT
- Learn about the agentic execution model
- Understand Claude's capabilities and limitations
- Learn about Claude models (Sonnet, Opus, Haiku)

### Content

#### What is Claude Code?

Claude Code is an AI-powered coding assistant that can:
- **Read and understand** your codebase
- **Write and edit** code files directly
- **Run commands** in your terminal
- **Search** for patterns across files
- **Integrate with Git** for commits, PRs, and reviews
- **Use tools** to accomplish complex tasks autonomously

#### How It Differs from ChatGPT

| Feature | Claude Code | ChatGPT |
|---------|-------------|---------|
| **Execution** | Can read/write files, run commands | Text-only responses |
| **Codebase Access** | Full access to your files | No direct access |
| **Tool Use** | Autonomous tool execution | Manual copy-paste |
| **Git Integration** | Native support | Manual workflow |
| **Permission Model** | Asks before actions | N/A |

#### The Agentic Execution Model

Claude Code operates as an **agent** that can:
1. **Perceive**: Read files, search code, run commands to understand context
2. **Reason**: Analyze the problem and plan solutions
3. **Act**: Execute tools to make changes
4. **Verify**: Check results and iterate

**Example flow**:
```
You: "Fix the bug in auth.js"
Claude:
  1. Reads auth.js
  2. Identifies the issue
  3. Edits the file
  4. Runs tests to verify
  5. Shows you the result
```

#### Claude Models

Three models available, each optimized for different use cases:

**Sonnet (Default)**
- **Best for**: General coding tasks
- **Speed**: Fast
- **Cost**: Medium
- **Use when**: Most day-to-day coding

**Opus**
- **Best for**: Complex reasoning, architecture decisions
- **Speed**: Slower
- **Cost**: Higher
- **Use when**: Difficult refactoring, system design

**Haiku**
- **Best for**: Simple, repetitive tasks
- **Speed**: Very fast
- **Cost**: Low
- **Use when**: Formatting, simple edits, quick searches

#### Claude's Capabilities

**What Claude Code CAN do**:
- Read any file in your project
- Write and edit code
- Search across your codebase
- Run terminal commands
- Create git commits and PRs
- Install dependencies
- Run tests
- Refactor code
- Generate documentation
- Debug issues
- Search the web for current information

**What Claude Code CANNOT do**:
- Access files outside the project (by default)
- Run GUI applications
- Access your secrets (and shouldn't!)
- Make network requests (except via web tools)
- Modify files without permission

#### Limitations

- **Knowledge Cutoff**: January 2025 (use web search for newer info)
- **Context Length**: Limited by token count (use `/compact` for large codebases)
- **Tool Errors**: Sometimes Claude needs to retry operations
- **Permission Required**: You approve actions in normal mode

### Exercise

**Task**: Research Claude Code to understand its capabilities

1. **Read the official documentation**:
   - Visit https://code.claude.com/docs
   - Read the "Getting Started" section
   - Review the "Core Concepts" page

2. **Watch introduction videos** (if available)

3. **Answer these questions** (in your notes):
   - What are the three Claude models and when would you use each?
   - What is the agentic execution model?
   - What permissions does Claude need and why?
   - How does Claude differ from traditional code editors with AI?

### Verification

Before moving on, ensure you can answer:
- [ ] What is Claude Code?
- [ ] How does the agentic model work?
- [ ] What are the three models and their use cases?
- [ ] What can Claude Code do vs what it cannot do?

### Notes & Reflections

*Write your thoughts and key takeaways here:*




---

## Lesson 2: Installation & Setup

### Objectives
- Install Claude Code CLI
- Set up account (Pro/Max/Teams/Enterprise)
- Start your first session
- Understand basic CLI commands

### Content

#### Prerequisites

Before installing Claude Code:
- **Operating System**: macOS, Linux, or Windows
- **Account**: Claude Pro, Max, Teams, or Enterprise subscription
- **Terminal**: Command-line access

#### Installation

**macOS/Linux**:
```bash
# Using Homebrew (recommended for macOS)
brew install anthropics/tap/claude

# Or using curl
curl -fsSL https://code.claude.com/install.sh | sh
```

**Windows**:
```powershell
# Download installer from https://code.claude.com
# Or use winget
winget install Anthropic.Claude
```

**Verify Installation**:
```bash
claude --version
```

#### First-Time Setup

1. **Authenticate**:
```bash
claude auth login
```
This opens your browser to authenticate with your Claude account.

2. **Verify Authentication**:
```bash
claude auth whoami
```

#### Starting Your First Session

**Basic Start**:
```bash
claude
```

**Start in a Project Directory**:
```bash
cd /path/to/your/project
claude
```

**Name Your Session**:
```bash
claude --session "fixing-auth-bug"
```

**Start with a Specific Model**:
```bash
claude --model opus  # For complex tasks
claude --model haiku # For quick tasks
```

#### Basic CLI Commands

**Help Commands**:
```bash
claude --help           # General help
claude <command> --help # Command-specific help
```

**Session Commands** (use within Claude):
```
/help          # Show available commands
/exit          # Exit current session
/clear         # Clear conversation history
/history       # Show past sessions
/resume        # Resume a previous session
```

**Configuration Commands**:
```bash
claude config list              # Show all settings
claude config set <key> <value> # Set a configuration
```

#### Account Tiers

| Tier | Features |
|------|----------|
| **Pro** | Basic access, standard rate limits |
| **Max** | Higher rate limits, priority access |
| **Teams** | Shared settings, team management |
| **Enterprise** | Managed deployment, SSO, compliance |

### Exercise

**Task**: Install Claude Code and verify setup

1. **Install Claude Code**:
   - Follow installation steps for your OS
   - Verify with `claude --version`

2. **Authenticate**:
   - Run `claude auth login`
   - Complete authentication in browser
   - Verify with `claude auth whoami`

3. **Start a Test Session**:
   ```bash
   mkdir ~/claude-test
   cd ~/claude-test
   claude --session "first-session"
   ```

4. **Try These Commands**:
   - Type `/help` to see available commands
   - Type `/clear` to clear the conversation
   - Type `/exit` to exit (you can resume later)

5. **Resume Your Session**:
   ```bash
   claude --session "first-session"
   ```

### Verification

Before moving on, ensure:
- [ ] Claude Code is installed and shows version
- [ ] You're authenticated (`claude auth whoami` works)
- [ ] You can start a session
- [ ] You can use `/help`, `/clear`, `/exit`
- [ ] You can resume a named session

### Troubleshooting

**"Command not found"**:
- Ensure installation completed successfully
- Check if binary is in PATH
- Restart terminal

**Authentication Issues**:
- Verify you have an active subscription
- Try `claude auth logout` then `claude auth login`
- Check internet connection

**Session Won't Start**:
- Check if you're in a valid directory
- Try without --session flag first
- Check for error messages

### Notes & Reflections

*Installation date:*
*OS and version:*
*Account tier:*
*Any issues encountered:*




---

## Lesson 3: Your First Conversation

### Objectives
- Start a conversation with Claude
- Ask Claude to analyze your current directory
- Learn basic commands: `/help`, `/clear`, `/exit`
- Understand how to give clear instructions

### Content

#### Starting a Conversation

**Best Practices for First Message**:
1. **Be Clear**: State exactly what you want
2. **Provide Context**: Mention what you're working on
3. **Be Specific**: "Fix the login bug" is better than "help with code"

**Example Good First Messages**:
```
"Analyze this project structure and explain what this codebase does"

"I need to add input validation to the user registration form in
src/components/RegisterForm.tsx"

"Help me debug why the API is returning 404 errors"

"Refactor the database queries in models/user.js to use async/await"
```

**Example Poor First Messages**:
```
"Hey" (too vague)
"Fix my code" (no context)
"Help" (what kind of help?)
```

#### Having Claude Analyze Your Directory

Try this in a project:

```
You: "Please analyze this project. Tell me:
1. What tech stack is being used
2. The overall project structure
3. The main entry points
4. Any obvious issues or improvements"
```

Claude will:
1. Use **Glob** to find files
2. Use **Read** to examine key files
3. Analyze the code
4. Provide a comprehensive summary

#### Basic In-Session Commands

**Getting Help**:
```
/help          # See all available commands
```

**Managing Conversation**:
```
/clear         # Clear conversation history (fresh start)
/history       # See past sessions
/resume        # Resume a previous session
```

**Exiting**:
```
/exit          # Exit current session
Ctrl+C         # Also exits
```

**Model Selection**:
```
/model sonnet  # Switch to Sonnet
/model opus    # Switch to Opus (more capable)
/model haiku   # Switch to Haiku (faster, simpler)
```

**Other Useful Commands**:
```
/compact       # Compress conversation history
/settings      # View current settings
```

#### Giving Clear Instructions

**Framework: Be Specific, Provide Context, State Goals**

**Example 1 - Bad**:
```
"Make this better"
```

**Example 1 - Good**:
```
"Refactor the authentication logic in auth.js to:
1. Use async/await instead of callbacks
2. Add proper error handling
3. Extract the token validation into a separate function"
```

**Example 2 - Bad**:
```
"Add a feature"
```

**Example 2 - Good**:
```
"Add a dark mode toggle to the app:
- Add a button in the header
- Store preference in localStorage
- Apply dark theme CSS when enabled
- Default to system preference"
```

#### Understanding Claude's Responses

Claude will:
1. **Acknowledge** your request
2. **Use tools** to gather context (you'll see tool calls)
3. **Explain** what it's doing
4. **Make changes** (if appropriate)
5. **Verify** the results

**You'll see output like**:
```
I'll help you add that feature. Let me first read the current code.

[Tool: Read - src/components/Header.tsx]
[Tool: Read - src/styles/theme.css]

I can see the header component. I'll add the dark mode toggle...

[Tool: Edit - src/components/Header.tsx]
[Tool: Write - src/styles/dark-theme.css]

I've added the dark mode toggle. The changes include:
1. New toggle button in header
2. Dark theme CSS
3. localStorage persistence

Try testing it out!
```

### Exercise

**Task**: Have your first meaningful conversation with Claude

1. **Choose a Project**:
   - Use an existing project, or
   - Create a simple test project:
   ```bash
   mkdir ~/claude-practice
   cd ~/claude-practice
   echo 'console.log("Hello");' > index.js
   echo '# Test Project' > README.md
   claude
   ```

2. **First Analysis**:
   Ask Claude to analyze the project:
   ```
   "Please analyze this directory and tell me what files are here
   and what the project appears to be"
   ```

3. **Follow-Up Question**:
   Ask something about the code:
   ```
   "Can you explain what index.js does?"
   ```

4. **Make a Change**:
   Ask Claude to modify something:
   ```
   "Add a function called greet(name) that returns 'Hello, {name}!'"
   ```

5. **Try Commands**:
   - Use `/help` to see commands
   - Use `/clear` to start fresh
   - Start a new conversation

6. **Practice Clear Instructions**:
   Try giving increasingly specific instructions and see how Claude responds.

### Verification

Before moving on, ensure you can:
- [ ] Start a Claude session
- [ ] Have Claude analyze your project
- [ ] Ask follow-up questions
- [ ] Give clear, specific instructions
- [ ] Use `/help`, `/clear`, `/exit`
- [ ] Understand Claude's tool usage

### Common Pitfalls

1. **Being Too Vague**: "Fix it" - What needs fixing?
2. **Not Providing Context**: Mentioning files Claude hasn't seen
3. **Too Many Requests at Once**: Break down complex tasks
4. **Not Reviewing Changes**: Always check what Claude modified

### Notes & Reflections

*First project analyzed:*
*Clarity of instructions (1-10):*
*How well did Claude understand you?:*
*Key learnings:*




---

## Lesson 4: Understanding Permissions

### Objectives
- Learn about permission modes (normal, auto-accept, plan)
- Understand why Claude asks for permission
- Learn to approve/deny tool usage
- Set permission preferences

### Content

#### Why Permissions Matter

Claude Code can make real changes to your files and system. Permissions ensure:
- **Safety**: You review before execution
- **Control**: You decide what happens
- **Learning**: You see what Claude is doing
- **Trust**: Build confidence in Claude's actions

#### Permission Modes

**1. Normal Mode (Default)**
```bash
claude
```
- Claude asks before each tool use
- You approve or deny each action
- Best for: Learning, important work, sensitive changes

**2. Auto-Accept Mode**
```bash
claude --auto-accept
```
- Claude executes without asking
- Faster workflow
- Best for: Trusted operations, experimentation
- ⚠️ Use carefully!

**3. Plan Mode**
```bash
claude --plan
```
- Claude explores (read-only) first
- Presents a plan
- You approve plan, then execution begins
- Best for: Complex features, large refactors

#### What Claude Asks Permission For

**File Operations**:
- Reading files
- Writing new files
- Editing existing files

**Command Execution**:
- Running bash commands
- Installing packages
- Running tests

**Git Operations**:
- Creating commits
- Pushing code
- Creating PRs

**Web Access**:
- Fetching URLs
- Searching the web

#### Approving Tool Usage

When Claude requests permission, you'll see:

```
Claude wants to use the Read tool:
  File: src/auth.js

[A]pprove  [D]eny  [E]xplain  [Always]  [Never]
```

**Your Options**:
- **A (Approve)**: Allow this specific action
- **D (Deny)**: Block this specific action
- **E (Explain)**: Ask Claude why it needs this
- **Always**: Auto-approve this type of action going forward
- **Never**: Auto-deny this type of action going forward

#### Granular Permissions

You can set permissions at different levels:

**Allow All Reads**:
```
When Claude asks to read: Choose [Always]
```

**Allow Reads in Specific Directory**:
```
In settings:
{
  "permissions": {
    "autoApprove": ["Read:src/**"]
  }
}
```

**Deny Dangerous Operations**:
```
{
  "permissions": {
    "autoDeny": ["Bash:rm -rf*", "Bash:git push --force"]
  }
}
```

#### Setting Permission Preferences

**Via CLI**:
```bash
# Start in auto-accept mode
claude --auto-accept

# Start in plan mode
claude --plan
```

**Via Settings** (`~/.claude/settings.json`):
```json
{
  "defaultPermissionMode": "normal",
  "permissions": {
    "autoApprove": [
      "Read",           // Auto-approve all reads
      "Grep",           // Auto-approve searches
      "Glob"            // Auto-approve file finding
    ],
    "autoDeny": [
      "Bash:rm -rf*",   // Never allow recursive delete
      "Bash:git push --force"  // Never allow force push
    ]
  }
}
```

**Project Settings** (`.claude/settings.json` in project):
```json
{
  "permissions": {
    "autoApprove": [
      "Read:src/**",
      "Read:tests/**"
    ]
  }
}
```

#### Best Practices

**When Learning (First Month)**:
- Use **normal mode**
- Review each action
- Use **[E]xplain** when unsure
- Build understanding

**For Trusted Operations**:
- Auto-approve **Read**, **Grep**, **Glob**
- These are safe, read-only operations
- Speeds up workflow significantly

**For Experimental Projects**:
- Use **auto-accept mode**
- Faster iteration
- Less critical if something breaks

**For Production Code**:
- Use **normal** or **plan mode**
- Review all changes carefully
- Never auto-approve git pushes

**Always Deny**:
- Destructive commands: `rm -rf`, `git reset --hard`
- Force operations: `git push --force`
- System-wide changes: `sudo` commands

### Exercise

**Task**: Experience all three permission modes

1. **Normal Mode**:
   ```bash
   cd ~/claude-practice
   claude

   You: "Read the index.js file and explain what it does"
   ```

   When prompted:
   - Choose **[A]pprove** to allow the read
   - Observe Claude's explanation
   - Try **[E]xplain** on the next request

2. **Auto-Accept Mode**:
   ```bash
   claude --auto-accept

   You: "Create a new file called utils.js with a helper function"
   ```

   Notice:
   - No permission prompts
   - Faster execution
   - Claude proceeds immediately

3. **Plan Mode**:
   ```bash
   claude --plan

   You: "Refactor the code to use ES6 modules"
   ```

   Observe:
   - Claude explores (read-only)
   - Presents a plan
   - Waits for your approval
   - Then executes

4. **Configure Permissions**:
   Edit `~/.claude/settings.json`:
   ```json
   {
     "permissions": {
       "autoApprove": ["Read", "Grep", "Glob"]
     }
   }
   ```

   Start a new session and notice reads are auto-approved.

5. **Test Denial**:
   - Start Claude normally
   - Ask for a file read
   - Choose **[D]eny**
   - See how Claude responds

### Verification

Before moving on, ensure you understand:
- [ ] Why permissions exist
- [ ] The three permission modes
- [ ] How to approve/deny individual actions
- [ ] What [E]xplain does
- [ ] How to auto-approve safe operations
- [ ] When to use each mode

### Safety Checklist

**Never Auto-Approve**:
- [ ] Git push commands
- [ ] Destructive file operations
- [ ] System administration commands
- [ ] Package installations (review first)

**Safe to Auto-Approve**:
- [x] Read operations
- [x] Grep searches
- [x] Glob file finding
- [x] Git status/diff (read-only git ops)

### Notes & Reflections

*Preferred permission mode:*
*Operations I'll auto-approve:*
*Operations I'll always review:*
*Comfort level with permissions (1-10):*




---

## Lesson 5: Basic Workflow Pattern

### Objectives
- Learn the explore → plan → code → verify pattern
- Understand how Claude breaks down tasks
- Learn to review changes before accepting
- Use `/resume` to continue sessions

### Content

#### The Standard Workflow Pattern

Most Claude Code sessions follow this pattern:

```
1. EXPLORE
   ↓ Claude reads files, searches code, gathers context

2. PLAN
   ↓ Claude explains what it will do

3. CODE
   ↓ Claude makes changes (with your permission)

4. VERIFY
   ↓ Claude (and you) check the results
```

#### Phase 1: Explore

**What Claude Does**:
- Uses **Glob** to find relevant files
- Uses **Grep** to search for patterns
- Uses **Read** to examine code
- Runs commands to understand state (e.g., `git status`)

**What You See**:
```
I'll help you add that feature. Let me first explore the codebase.

[Tool: Glob - **/*.js]
[Tool: Read - src/main.js]
[Tool: Read - src/components/App.js]
[Tool: Grep - "export function"]

I can see the structure now...
```

**Best Practice**: Let Claude explore! Don't rush to coding.

#### Phase 2: Plan

**What Claude Does**:
- Explains the approach
- Lists changes needed
- Identifies potential issues
- Asks for clarification if needed

**What You See**:
```
Here's my plan:

1. Add a new function `validateInput()` in src/utils/validation.js
2. Update the form component to call this function
3. Add error display in the UI
4. Add tests in tests/validation.test.js

Does this approach sound good?
```

**Best Practice**: Review the plan! Suggest changes if needed.

#### Phase 3: Code

**What Claude Does**:
- Makes the planned changes
- Uses **Edit** for existing files
- Uses **Write** for new files
- Follows your project's patterns

**What You See**:
```
[Tool: Write - src/utils/validation.js]
[Tool: Edit - src/components/Form.js]
[Tool: Edit - src/components/Form.css]

I've made the changes. Here's what I did:
- Created validation.js with input checking
- Updated Form.js to use the validator
- Added error styling to Form.css
```

**Best Practice**: Review diffs before approving edits!

#### Phase 4: Verify

**What Claude Does**:
- Runs tests if appropriate
- Checks syntax
- Verifies the changes work
- May read changed files to confirm

**What You See**:
```
Let me verify the changes work:

[Tool: Bash - npm test]

Tests pass! The validation is working correctly.
```

**Best Practice**: Do your own verification too!

#### How Claude Breaks Down Tasks

**Simple Task**:
```
You: "Add a console.log statement to the main function"

Claude:
  EXPLORE: Read the file
  PLAN: (implicit - it's obvious)
  CODE: Edit the file
  VERIFY: Read back to confirm
```

**Complex Task**:
```
You: "Add user authentication to the app"

Claude:
  EXPLORE:
    - Read existing code
    - Check for auth libraries
    - Find user-related files

  PLAN:
    - Install dependencies
    - Create auth service
    - Add middleware
    - Update routes
    - Add login UI
    - Add tests

  CODE:
    - Install packages
    - Create new files
    - Update existing files

  VERIFY:
    - Run tests
    - Check for errors
    - Suggest manual testing
```

**Best Practice**: For complex tasks, use Plan Mode!

#### Reviewing Changes

**Before Accepting an Edit**:

1. **Read the Diff**:
   ```diff
   - const user = getUser(id)
   + const user = await getUser(id)
   ```

2. **Check for**:
   - Correctness: Does it solve the problem?
   - Style: Does it match the project?
   - Safety: Are there any issues?
   - Scope: Did it change only what's needed?

3. **Ask Questions**:
   ```
   You: "Why did you remove that error handling?"
   ```

4. **Request Changes**:
   ```
   You: "Can you keep the original error handling but update it
   to work with async/await?"
   ```

#### Using `/resume` to Continue Sessions

**Why Resume?**:
- Continue where you left off
- Maintain context
- Work in multiple sessions

**How to Resume**:

**List Past Sessions**:
```
/history
```

**Resume by Name**:
```
/resume fixing-auth-bug
```

**Resume Most Recent**:
```
claude --resume
```

**From Command Line**:
```bash
# Resume named session
claude --session "fixing-auth-bug"

# Resume last session
claude --resume
```

**What Gets Preserved**:
- Conversation history
- Files that were read
- Changes made
- Context built

**What Doesn't**:
- Transient state (current working directory)
- Running processes

#### Workflow Best Practices

**1. Start with Context**:
```
Good: "I'm working on the user authentication feature.
       I need to add password reset functionality."

Bad: "Add password reset"
```

**2. Let Claude Explore**:
```
Good: Ask question → Let Claude explore → Review plan → Approve
Bad: Give detailed instructions for every step
```

**3. Review Before Approving**:
```
Good: Read diff → Ask questions → Approve or request changes
Bad: Auto-approve everything without looking
```

**4. Verify Results**:
```
Good: Run tests, check manually, confirm it works
Bad: Assume it works, move on
```

**5. Iterate**:
```
Good: "That works, but can you also add error handling?"
Bad: Try to specify everything upfront
```

### Exercise

**Task**: Complete a full workflow cycle

1. **Create a Practice Project**:
   ```bash
   mkdir ~/claude-workflow-practice
   cd ~/claude-workflow-practice

   cat > calculator.js << EOF
   function add(a, b) {
     return a + b;
   }

   console.log(add(2, 3));
   EOF

   claude --session "workflow-practice"
   ```

2. **Give Claude a Task**:
   ```
   You: "Add subtract, multiply, and divide functions to calculator.js.
   Then create a simple test file that tests all four functions."
   ```

3. **Observe the Workflow**:
   - **EXPLORE**: Watch Claude read calculator.js
   - **PLAN**: Note how Claude explains the approach
   - **CODE**: Review the edits and new files
   - **VERIFY**: See Claude test the code

4. **Review Changes**:
   - Before approving edits, read the diffs carefully
   - Ask "why did you structure it this way?"
   - Suggest an improvement

5. **Iterate**:
   ```
   You: "Can you add input validation to ensure numbers are passed?"
   ```

   Watch Claude go through the cycle again.

6. **Exit and Resume**:
   ```
   /exit
   ```

   Then:
   ```bash
   claude --session "workflow-practice"
   ```

   Notice your history is preserved.

7. **Verify Everything Works**:
   ```
   You: "Run the test file to make sure everything passes"
   ```

### Verification

Before moving on, ensure you can:
- [ ] Recognize the explore → plan → code → verify pattern
- [ ] Understand why each phase matters
- [ ] Review changes before accepting
- [ ] Ask Claude to explain decisions
- [ ] Request modifications to Claude's plans
- [ ] Resume previous sessions
- [ ] Verify results yourself

### Workflow Checklist

For every significant task, ensure you:
- [ ] Provide clear context upfront
- [ ] Let Claude explore before coding
- [ ] Review the plan before execution
- [ ] Read diffs before approving edits
- [ ] Ask questions when unsure
- [ ] Verify the results work
- [ ] Iterate if needed

### Notes & Reflections

*Most helpful phase of workflow:*
*Biggest challenge in workflow:*
*How well do you review changes (1-10)?:*
*Key workflow insights:*




---

## Section 1 Summary

### What You've Learned

1. **What Claude Code Is**: An agentic AI coding assistant
2. **Installation**: Set up and authenticate Claude
3. **First Conversation**: Interact with Claude effectively
4. **Permissions**: Understand and control what Claude can do
5. **Workflow Pattern**: Follow the explore → plan → code → verify cycle

### Section Verification

Before moving to Section 2, verify you can:
- [ ] Explain what Claude Code is and how it differs from ChatGPT
- [ ] Install and authenticate Claude
- [ ] Start and manage sessions
- [ ] Give clear, specific instructions
- [ ] Use basic commands (/ help, /clear, /exit, /resume)
- [ ] Understand the three permission modes
- [ ] Approve/deny tool usage appropriately
- [ ] Follow the standard workflow pattern
- [ ] Review and verify changes

### Next Steps

You're ready for **Section 2: Core File Tools**!

In the next section, you'll learn about:
- Reading files with the Read tool
- Creating files with the Write tool
- Editing files with the Edit tool
- Finding files with Glob
- Searching content with Grep
- Running commands with Bash
- Combining tools effectively

### Practice Before Moving On

**Recommended Practice**:
1. Use Claude for a real task in a personal project
2. Practice giving clear instructions
3. Try all three permission modes
4. Resume a session at least once
5. Complete one full workflow cycle

---

**Congratulations on completing Section 1!** 🎉

*Time to practice: Before moving to Section 2, spend at least 2-3 hours using Claude on real projects to solidify these fundamentals.*
