# Section 7: Skills System (Lessons 33-42)

## Overview
Master the Skills system to create reusable, shareable expertise. Skills are one of Claude Code's most powerful features for customization and automation.

---

## Lesson 33: What Are Skills?

### Objectives
- Understand skills as reusable expertise
- Learn skill scopes (enterprise, personal, project, plugin)
- Explore built-in skills
- Know when to create skills

### Content

#### What Are Skills?

**Skills** are reusable expertise packages that extend Claude's capabilities with specialized knowledge and workflows.

**Think of Skills As**:
- Specialized assistants for specific tasks
- Reusable templates for common workflows
- Codified best practices
- Shared team knowledge

#### How Skills Work

```
User: /code-review

Claude:
  [Loads code-review skill]
  [Follows skill instructions]
  [Uses skill-specific tools]
  [Produces skill-defined output]
```

**Behind the Scenes**:
1. Skill SKILL.md loaded into context
2. Claude follows skill instructions
3. Tools restricted to skill's allowed list
4. Output formatted per skill requirements

#### Skill Components

**SKILL.md File**:
```markdown
---
name: skill-name
description: What this skill does
commands:
  - command-name
triggers:
  - pattern: "trigger phrase"
    command: command-name
model: sonnet
autoInvoke: false
tools:
  - Read
  - Write
---

# Skill Instructions

[Detailed instructions for Claude]
```

**Key Parts**:
- **Frontmatter** (YAML): Configuration
- **Instructions**: What Claude should do
- **Examples**: How to use the skill
- **Supporting Files**: Templates, examples

#### Skill Scopes

**1. Enterprise Skills** (`/enterprise/skills/`)
- Managed by organization
- Available to all team members
- Enforced standards
- Examples: Security review, deployment checklist

**2. Personal Skills** (`~/.claude/skills/`)
- Your personal skills
- Available in all projects
- Examples: Personal code style, common tasks

**3. Project Skills** (`.claude/skills/`)
- Specific to one project
- Shared via git
- Examples: Project-specific workflows

**4. Plugin Skills** (via plugins)
- Bundled with plugins
- Installed from marketplace
- Examples: Community-contributed skills

**Precedence**: Enterprise → Personal → Project → Plugin

#### Built-in vs Custom Skills

**Built-in Skills** (if available):
- Pre-installed with Claude Code
- Common workflows
- Maintained by Anthropic
- Examples: Basic git, testing, documentation

**Custom Skills**:
- Created by you or your team
- Specialized to your needs
- Shareable with others
- Examples: Company-specific workflows

#### When to Create a Skill

**Create a Skill When**:
✅ You do the same task repeatedly
✅ Task has clear, reusable steps
✅ Others could benefit from the workflow
✅ You want consistent results
✅ Task requires specific domain knowledge

**Examples**:
- Code review with company standards
- Creating new components following patterns
- Deployment checklists
- Database migration workflows
- API endpoint generation

**Don't Create a Skill When**:
❌ Task is one-off
❌ Steps vary significantly each time
❌ Better handled by hooks (automatic actions)
❌ Too simple (just ask Claude directly)

#### Skill Discovery

**List Available Skills**:
```bash
claude skills list
# or in session:
/skills
```

**Skill Information**:
```bash
claude skills info code-review
```

**Search Skills**:
```bash
claude skills search "review"
```

#### Invoking Skills

**1. Command Syntax**:
```
/skill-name
/skill-name arg1 arg2
```

**2. Auto-Invocation**:
```
User: "review this code"
→ Triggers /code-review skill automatically
```

**3. Skill Tool**:
```
Use Skill tool with skill name
```

#### Skill Benefits

**Consistency**:
- Same process every time
- Standardized output
- Reduced errors

**Efficiency**:
- Don't repeat instructions
- Faster execution
- One command vs multiple prompts

**Knowledge Sharing**:
- Codify best practices
- Onboard new team members
- Share expertise

**Quality**:
- Comprehensive checklists
- Don't forget steps
- Proven workflows

### Exercise

**Task**: Explore and understand skills

**Setup**:
```bash
# Check if you have any skills
ls ~/.claude/skills/

# If not, create directory
mkdir -p ~/.claude/skills/
```

**Tasks**:

1. **Conceptual Understanding**:
```
Think of 3 tasks you do repeatedly in your work.
For each, answer:
- What are the steps?
- Could this be a skill?
- What would the skill do?
```

2. **Explore Skill Structure**:
```
Look at the skill template:
cat ~/claude-code-bootcamp/templates/SKILL-TEMPLATE.md

Identify:
- Where is the configuration?
- Where are the instructions?
- Where are the examples?
```

3. **Read Example Skill**:
```
Read the code review skill:
cat ~/claude-code-bootcamp/resources/example-skills/code-review-skill/SKILL.md

Understand:
- What does it do?
- What tools does it use?
- How is it structured?
```

4. **Plan Your First Skill**:
```
Write down:
- Skill name: ___________
- What it does: ___________
- Steps it follows: ___________
- Tools it needs: ___________
```

### Verification

Before moving on, ensure you understand:
- [ ] What skills are and how they work
- [ ] The four skill scopes
- [ ] When to create a skill vs not
- [ ] How to invoke skills
- [ ] Basic skill structure

### Key Concepts

**Skill vs Prompt**:
- Prompt: One-time instruction
- Skill: Reusable workflow

**Skill vs Hook**:
- Skill: Manually invoked
- Hook: Automatically triggered

**Skill vs Subagent**:
- Skill: Instructions + limited tools
- Subagent: Full agent with custom behavior

---

## Lesson 34: Using Existing Skills

### Objectives
- Invoke skills with /command syntax
- Pass arguments to skills
- Understand auto-invocation
- Use Skill tool

### Content

#### Basic Skill Invocation

**Command Syntax**:
```
/skill-name
```

**Example**:
```
You: /code-review

Claude:
  [Loads code-review skill]
  "I'll perform a comprehensive code review..."
  [Executes review workflow]
```

#### Passing Arguments

**With Arguments**:
```
/skill-name arg1 arg2 arg3
```

**Example**:
```
You: /code-review src/api/users.js

Claude:
  "I'll review src/api/users.js..."
  [Reviews specified file]
```

**Multiple Arguments**:
```
You: /deploy production --skip-tests

Claude:
  "Deploying to production, skipping tests..."
```

#### Auto-Invocation

**Trigger Patterns**:
Skills can auto-trigger based on patterns:

```yaml
triggers:
  - pattern: "review (this|the) code"
    command: review
```

**Example**:
```
You: "Can you review this code?"

Claude:
  [Auto-invokes code-review skill]
  "I'll perform a code review..."
```

**Benefits**:
- Natural language interaction
- No need to remember exact commands
- Seamless experience

**Disable Auto-Invoke**:
```yaml
autoInvoke: false
```

#### Using the Skill Tool

**Explicit Skill Tool**:
```
"Use the code-review skill on these files"
```

Claude will:
1. Recognize skill reference
2. Load skill
3. Execute workflow

**With Fully Qualified Name**:
```
"Use enterprise:code-review skill"
"Use plugin:awesome-plugin:formatter skill"
```

#### Skill Context

**What Skills See**:
- Current conversation history
- Files you've mentioned
- Current working directory
- Skill instructions
- Supporting files

**What Skills Don't See**:
- Other skills' internals
- Hook configurations (unless specified)
- Your full chat history (only current session)

#### Common Built-in Skills (if available)

**Git Skills**:
```
/commit              # Create git commit
/commit-fix          # Fix commit message
/branch              # Create/switch branch
```

**Testing Skills**:
```
/test                # Run tests
/test-gen            # Generate tests
/coverage            # Check coverage
```

**Documentation Skills**:
```
/doc                 # Generate docs
/readme              # Create/update README
/api-doc             # API documentation
```

**Deployment Skills**:
```
/deploy              # Deploy application
/rollback            # Rollback deployment
```

#### Skill Best Practices

**When Using Skills**:

1. **Read Skill Description First**:
```
/skills info code-review
```

2. **Provide Required Context**:
```
# Bad
/deploy

# Good
/deploy production --version v2.1.0
```

3. **Check Output**:
- Review what skill did
- Verify results
- Ask for clarification if needed

4. **Combine Skills**:
```
/test
# If tests pass:
/deploy staging
```

### Exercise

**Task**: Use skills effectively

**Setup**:
```bash
cd ~/claude-practice
claude
```

**Tasks**:

1. **List Available Skills** (if supported):
```
/skills
# or
"What skills are available?"
```

2. **Get Skill Info** (if supported):
```
/skills info code-review
# or
"Tell me about the code-review skill"
```

3. **Invoke a Skill**:
```
# If you have code-review skill installed:
/code-review

# Otherwise, simulate:
"Perform a code review following these steps:
1. Check for security issues
2. Check code quality
3. Check performance
4. Generate report"
```

4. **Pass Arguments**:
```
/code-review src/app.js
# or
"Review src/app.js for security and performance"
```

5. **Chain Skills**:
```
"First run tests, then if they pass, create a commit"
```

### Verification

Before moving on, ensure you can:
- [ ] Invoke skills with /command syntax
- [ ] Pass arguments to skills
- [ ] Understand auto-invocation
- [ ] Get skill information
- [ ] Chain multiple skills

---

## Lesson 35: Creating Your First Skill

### Objectives
- Create SKILL.md file
- Write skill instructions
- Set up skill directory
- Test your skill

### Content

#### Skill File Structure

**Basic Structure**:
```
~/.claude/skills/my-skill/
├── SKILL.md          # Main skill file
├── templates/        # Optional templates
└── examples/         # Optional examples
```

**Single-File Skill**:
```
~/.claude/skills/my-skill.md
```

#### Creating SKILL.md

**Minimal Skill**:
```markdown
---
name: my-skill
description: Brief description
commands:
  - my-command
---

# My Skill

Instructions for Claude to follow.
```

**Step-by-Step**:

1. **Create Directory**:
```bash
mkdir -p ~/.claude/skills/hello-world
cd ~/.claude/skills/hello-world
```

2. **Create SKILL.md**:
```bash
cat > SKILL.md << 'EOF'
---
name: hello-world
description: A simple hello world skill
commands:
  - hello
  - greet
---

# Hello World Skill

When invoked, greet the user enthusiastically.

## Instructions

1. Generate a friendly greeting
2. Include a random fun fact
3. Wish them a great day
EOF
```

3. **Test the Skill**:
```bash
claude
/hello
```

#### Writing Effective Instructions

**Structure Your Instructions**:

```markdown
# Skill Name

Brief overview of what this skill does.

## Purpose
[Why this skill exists]

## Instructions

### Step 1: [First Step]
[Detailed instructions]

### Step 2: [Second Step]
[Detailed instructions]

## Output Format
[How to format results]

## Examples
[Show examples]
```

**Be Specific**:
```markdown
# Bad (vague)
Check the code for issues.

# Good (specific)
Analyze the code for:
1. Security vulnerabilities (SQL injection, XSS, auth bypasses)
2. Performance issues (N+1 queries, inefficient algorithms)
3. Code quality (complexity >10, duplication, naming)
4. Best practices violations

For each issue found:
- Severity: Critical/High/Medium/Low
- Location: file:line
- Problem: What's wrong
- Fix: How to fix it
```

**Use Numbered Steps**:
```markdown
1. Read all files in src/
2. Identify functions >50 lines
3. For each function:
   a. Calculate complexity
   b. Check for duplication
   c. Suggest refactoring if needed
4. Generate report with:
   - Total functions analyzed
   - Functions needing refactoring
   - Specific recommendations
```

#### Example: Documentation Skill

**Complete Example**:
```markdown
---
name: doc-function
description: Generates JSDoc documentation for functions
commands:
  - doc-function
  - document
model: sonnet
tools:
  - Read
  - Edit
---

# Function Documentation Skill

Generates comprehensive JSDoc documentation for JavaScript/TypeScript functions.

## Purpose
Ensure all functions have proper documentation following JSDoc standards.

## Instructions

### Step 1: Identify Function
If file and function name provided, read that file.
If not provided, ask which function to document.

### Step 2: Analyze Function
Analyze the function:
- Parameters and their types
- Return value and type
- What the function does
- Potential errors/exceptions
- Examples of usage

### Step 3: Generate JSDoc
Create JSDoc comment with:
```javascript
/**
 * [Brief description]
 *
 * [Detailed description if complex]
 *
 * @param {type} paramName - Description
 * @param {type} [optionalParam] - Description (optional)
 * @returns {type} Description of return value
 * @throws {ErrorType} When/why it throws
 * @example
 * // Example usage
 * functionName(arg1, arg2)
 */
```

### Step 4: Add Documentation
Use Edit tool to add JSDoc above the function.

### Step 5: Verify
Show the documented function for review.

## Example Usage

```
User: /doc-function src/utils.js calculateTotal

Output: [Adds JSDoc to calculateTotal function]
```

## Notes
- Follow existing project JSDoc style if present
- Include @example for non-trivial functions
- Use TypeScript types if .ts file
```

### Exercise

**Task**: Create your first useful skill

**Skill to Build**: Code formatter checker

**Requirements**:
```markdown
Name: format-check
Description: Checks if code follows formatting rules
Commands: format-check

Should check:
- Consistent indentation (2 spaces)
- No trailing whitespace
- Semicolons present (for JS)
- Proper line breaks
- Max line length (80 chars)

Output: List of formatting issues found
```

**Steps**:

1. **Create Directory**:
```bash
mkdir -p ~/.claude/skills/format-check
```

2. **Write SKILL.md**:
```bash
# Use your editor or ask Claude:
"Create SKILL.md for a format-check skill that:
[paste requirements above]

Follow the structure from the documentation skill example."
```

3. **Test It**:
```bash
# Create a test file with bad formatting
cat > ~/claude-practice/messy.js << 'EOF'
function test(){
var x=1;
  return x
}
EOF

claude
/format-check messy.js
```

4. **Refine**:
```
Based on the results, refine your skill:
- Add more checks
- Improve output format
- Add color coding (if wanted)
```

### Verification

Before moving on, ensure you can:
- [ ] Create SKILL.md file with frontmatter
- [ ] Write clear, specific instructions
- [ ] Organize instructions in steps
- [ ] Test your skill
- [ ] Refine based on results

### Common Mistakes

1. **Too Vague**:
```markdown
# Bad
Check the code.

# Good
Check the code for:
1. [Specific thing]
2. [Specific thing]
```

2. **No Structure**:
```markdown
# Bad
Just a wall of text instructions...

# Good
## Step 1: ...
## Step 2: ...
```

3. **Missing Output Format**:
```markdown
# Add this section:
## Output Format
Present results as:
- Summary
- Details
- Recommendations
```

---

*Continue with Lessons 36-42 following similar depth and practical focus...*

Would you like me to:
1. Continue with remaining lessons in Section 7 (Lessons 36-42)?
2. Create more advanced sections (MCP, Subagents)?
3. Build additional project templates?
4. Create assessment tests and quizzes?
5. Add video course outline?

Let me know what would be most valuable!