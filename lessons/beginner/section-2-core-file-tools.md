# Section 2: Core File Tools (Lessons 6-12)

## Overview
Master the essential tools Claude uses to interact with your codebase. These are the foundation of everything Claude does.

---

## Lesson 6: Reading Files with Read Tool

### Objectives
- Understand when Claude uses the Read tool
- Learn to reference specific files
- Use @-mentions for file references
- Read multiple files efficiently

### Content

#### The Read Tool

The **Read tool** is Claude's primary way to understand your code. It's used constantly.

**What Read Does**:
- Reads file contents
- Returns with line numbers
- Handles various file types (code, markdown, JSON, etc.)
- Can read specific line ranges
- Shows files visually (images, PDFs)

#### When Claude Uses Read

**Automatic Usage**:
```
You: "Explain what app.js does"
Claude: [Uses Read on app.js] → Explains the code
```

**During Exploration**:
```
You: "Fix the bug in the auth module"
Claude:
  [Uses Glob to find auth files]
  [Uses Read on each file]
  [Identifies the bug]
```

**Before Editing**:
```
You: "Update the config"
Claude:
  [Uses Read to see current config]
  [Makes informed changes]
```

#### Reading Specific Line Ranges

**Full File** (default):
```
You: "Read src/app.js"
Claude: [Reads entire file]
```

**Specific Lines**:
```
You: "Read src/app.js lines 50-100"
Claude: [Reads only lines 50-100]
```

**Why This Matters**:
- Large files consume tokens
- Sometimes you only need specific sections
- More efficient context usage

#### @-Mentions for Files

**Direct File References**:
```
You: "Explain @src/auth.js"
Claude: [Reads and explains auth.js]
```

**Multiple Files**:
```
You: "Compare @src/auth.js and @src/auth-backup.js"
Claude: [Reads both, compares them]
```

**In IDE**: @-mentions trigger autocomplete

#### Reading Multiple Files

**Sequential**:
```
You: "Read app.js, then config.js, then utils.js and explain
how they work together"

Claude:
  [Reads app.js]
  [Reads config.js]
  [Reads utils.js]
  [Provides integrated explanation]
```

**Parallel** (more efficient):
```
You: "Read these files in parallel:
- app.js
- config.js
- utils.js
- database.js

Then explain the architecture"

Claude: [Reads all 4 simultaneously] → Explains
```

#### File Types Read Supports

**Text Files**:
- Source code (.js, .py, .rs, .go, etc.)
- Markdown (.md)
- Configuration (.json, .yaml, .toml)
- Documentation (.txt, .rst)

**Special Handling**:
- **Images**: Displayed visually
- **PDFs**: Processed page by page
- **Jupyter Notebooks**: Shows code + outputs
- **Binary files**: Error (can't read)

#### Read Tool Output Format

```
     1  import express from 'express'
     2  import { connectDB } from './db'
     3
     4  const app = express()
     5  const PORT = process.env.PORT || 3000
     6
     7  app.get('/health', (req, res) => {
     8    res.json({ status: 'healthy' })
     9  })
    10
    11  app.listen(PORT, () => {
    12    console.log(`Server running on port ${PORT}`)
    13  })
```

**Note**: Line numbers for easy reference

#### Efficient Reading Strategies

**1. Read Before Edit**:
```
Bad: "Change the API endpoint to /v2/users"
Good: "Read api/routes.js, then update the users endpoint to /v2/users"
```

**2. Read Related Files**:
```
"Read both the controller and the model to understand the full flow"
```

**3. Use Line Ranges for Large Files**:
```
"Read just the imports section (lines 1-20) of app.js"
```

**4. Combine with Grep**:
```
"Search for 'authentication' in src/, then read the relevant files"
```

### Exercise

**Task**: Master file reading techniques

**Setup**:
```bash
cd ~/claude-practice
cat > sample.js << 'EOF'
// Configuration
const config = {
  port: 3000,
  host: 'localhost',
  database: {
    url: 'mongodb://localhost:27017',
    name: 'myapp'
  }
}

// Helper function
function getConfigValue(key) {
  return config[key]
}

// Export
module.exports = { config, getConfigValue }
EOF

cat > index.js << 'EOF'
const { config } = require('./sample')
const express = require('express')

const app = express()

app.listen(config.port, () => {
  console.log(`Running on ${config.port}`)
})
EOF

claude
```

**Tasks**:

1. **Basic Read**:
```
"Read sample.js and explain what it does"
```

2. **Specific Lines**:
```
"Read lines 1-10 of sample.js"
```

3. **Multiple Files**:
```
"Read both sample.js and index.js and explain how they work together"
```

4. **Using @-mentions**:
```
"What's different between @sample.js and @index.js?"
```

5. **Efficient Reading**:
```
"Read sample.js and index.js in parallel, then tell me
what the application does"
```

### Verification

Before moving on, ensure you can:
- [ ] Request Claude to read specific files
- [ ] Use @-mentions for file references
- [ ] Read specific line ranges
- [ ] Read multiple files in parallel
- [ ] Understand Read tool output format

### Common Mistakes

1. **Not Reading Before Editing**:
   - Claude might make incorrect assumptions
   - Always read current state first

2. **Reading Entire Large Files**:
   - Wastes tokens
   - Use line ranges for targeted reading

3. **Sequential When Parallel Would Work**:
   - Slower than necessary
   - Request parallel reads for independent files

### Pro Tips

**Line References**:
```
"The bug is around line 45 in auth.js - read that section"
```

**Context Building**:
```
"Read the main file, then read its imports, then explain the flow"
```

**Comparison**:
```
"Read the old version (backup.js) and new version (current.js)
and explain what changed"
```

---

## Lesson 7: Creating Files with Write Tool

### Objectives
- Learn when to use Write vs Edit
- Create new files from scratch
- Understand file path handling
- Review file contents before writing

### Content

#### The Write Tool

**Write** creates new files or completely replaces existing files.

**When to Use Write**:
- Creating new files
- Generating code from scratch
- Replacing entire file contents
- Creating configuration files

**When NOT to Use Write**:
- Modifying existing files → Use **Edit** instead
- Changing small sections → Use **Edit**
- Updating configuration → Usually **Edit**

#### Write Tool Syntax

**Basic Write**:
```
File path: src/newFile.js
Content: [the entire file content]
```

**What Happens**:
1. Claude requests permission
2. You approve
3. File is created/overwritten
4. Claude confirms success

#### Creating New Files

**Example Flow**:
```
You: "Create a new file src/utils/logger.js with a basic
logging utility"

Claude:
  "I'll create a logger utility..."

  [Tool: Write - src/utils/logger.js]

  Content:
  const logger = {
    info: (msg) => console.log(`[INFO] ${msg}`),
    error: (msg) => console.error(`[ERROR] ${msg}`),
    debug: (msg) => console.debug(`[DEBUG] ${msg}`)
  }

  module.exports = logger

  "Logger utility created!"
```

#### File Path Handling

**Absolute Paths**:
```
/Users/username/project/src/app.js
```

**Relative Paths** (to current directory):
```
src/app.js
./src/app.js
```

**Creating Directories**:
Claude will create parent directories if they don't exist:
```
Write: src/api/v2/controllers/userController.js
→ Creates src/, api/, v2/, controllers/ if needed
```

#### Overwriting Files

**Warning**: Write REPLACES entire file!

```
You: "Write a new config.js"

Claude: [Creates config.js]

You: "Write config.js with different content"

Claude: [REPLACES entire config.js]
⚠️ Previous content is LOST!
```

**Best Practice**: Use Edit for existing files

#### Reviewing Before Writing

**Always Review**:
```
Claude shows:
  "I'll write this to src/app.js:"

  [Shows full content]

  "Approve to create this file"
```

**Check**:
- File path is correct
- Content is what you want
- Not accidentally overwriting important file

#### Write Tool vs Edit Tool

| Scenario | Tool | Reason |
|----------|------|--------|
| Create new file | Write | File doesn't exist |
| Add a function | Edit | Modifying existing |
| Change a variable | Edit | Small change |
| Rewrite entire file | Write | Complete replacement |
| Generate boilerplate | Write | Creating from scratch |
| Update config value | Edit | Targeted change |

#### Creating Multiple Files

**Sequential**:
```
You: "Create index.js, app.js, and config.js for an Express app"

Claude:
  [Writes index.js]
  [Writes app.js]
  [Writes config.js]
```

**Organized Approach**:
```
You: "Set up a new Express API with:
- src/index.js (entry point)
- src/app.js (Express setup)
- src/routes/users.js (user routes)
- src/controllers/userController.js
- src/models/User.js
- package.json"

Claude: [Creates all files in order]
```

#### File Content Best Practices

**Include Necessary Elements**:
- Imports/requires at top
- Proper exports at bottom
- Comments for clarity
- Error handling
- Following project conventions

**Example Good Write**:
```javascript
// src/services/emailService.js

const nodemailer = require('nodemailer')
const config = require('../config')

/**
 * Email service for sending transactional emails
 */
class EmailService {
  constructor() {
    this.transporter = nodemailer.createTransport({
      host: config.email.host,
      port: config.email.port,
      auth: {
        user: config.email.user,
        pass: config.email.password
      }
    })
  }

  async sendEmail(to, subject, body) {
    try {
      const result = await this.transporter.sendMail({
        from: config.email.from,
        to,
        subject,
        html: body
      })
      return { success: true, messageId: result.messageId }
    } catch (error) {
      console.error('Email send failed:', error)
      return { success: false, error: error.message }
    }
  }
}

module.exports = new EmailService()
```

### Exercise

**Task**: Create a complete module from scratch

**Setup**:
```bash
cd ~/claude-practice
rm -rf math-utils  # Clean slate
mkdir math-utils
cd math-utils
claude
```

**Tasks**:

1. **Create Main Module**:
```
"Create src/math.js with functions for:
- add(a, b)
- subtract(a, b)
- multiply(a, b)
- divide(a, b) with error handling for division by zero

Use modern JavaScript with proper exports"
```

2. **Create Test File**:
```
"Create tests/math.test.js with comprehensive tests for all
math functions using Jest"
```

3. **Create Package.json**:
```
"Create package.json for this project with:
- name: math-utils
- version: 1.0.0
- test script
- jest as dev dependency"
```

4. **Create README**:
```
"Create README.md with:
- Project description
- Installation instructions
- Usage examples
- API documentation"
```

5. **Review All Files**:
```
"Show me all the files you created"
```

### Verification

Before moving on, ensure you can:
- [ ] Create new files with Write tool
- [ ] Understand Write vs Edit distinction
- [ ] Create multiple related files
- [ ] Review content before approving
- [ ] Create proper file structure

### Common Mistakes

1. **Using Write Instead of Edit**:
   - Overwrites entire file
   - Loses existing content
   - Use Edit for modifications!

2. **Not Reviewing Content**:
   - Always check what Claude is writing
   - Verify file path is correct

3. **Missing Directory Creation**:
   - Claude creates dirs automatically
   - But verify path is intended

### Pro Tips

**Template Generation**:
```
"Create a new React component following our standard template"
```

**Boilerplate Creation**:
```
"Set up a new Express route with controller, service, and tests"
```

**Configuration Files**:
```
"Create .eslintrc.json with our team's linting rules"
```

---

## Lesson 8: Editing Files with Edit Tool

### Objectives
- Understand exact string replacement
- Learn Edit tool best practices
- Handle multi-line edits
- Use replace_all for renaming

### Content

#### The Edit Tool

**Edit** makes precise changes to existing files using exact string replacement.

**How It Works**:
1. Claude reads the file (if not already read)
2. Finds the exact string to replace
3. Replaces it with new string
4. Shows you a diff
5. Applies change on approval

#### Exact String Matching

**Critical**: Edit uses EXACT string matching!

**This Works**:
```javascript
Old string (from file):
function greet(name) {
  return `Hello, ${name}!`
}

New string:
function greet(name) {
  return `Hello, ${name}! Welcome!`
}
```

**This Fails**:
```javascript
Old string (mismatched whitespace):
function greet(name){
  return `Hello, ${name}!`
}

❌ Won't find it! Whitespace doesn't match.
```

#### Edit Tool Syntax

**Basic Edit**:
```
File: src/app.js
Old string: const PORT = 3000
New string: const PORT = process.env.PORT || 3000
```

**Multi-line Edit**:
```
File: src/app.js
Old string:
app.get('/users', (req, res) => {
  res.json({ users: [] })
})

New string:
app.get('/users', async (req, res) => {
  const users = await User.find()
  res.json({ users })
})
```

#### Understanding Diffs

**Claude Shows**:
```diff
  const express = require('express')
  const app = express()

- const PORT = 3000
+ const PORT = process.env.PORT || 3000

  app.listen(PORT, () => {
    console.log(`Server on ${PORT}`)
  })
```

**Legend**:
- ` ` (space): Unchanged context
- `-` (red): Removed
- `+` (green): Added

#### Replace All

**Single Replacement** (default):
```
Old: const port = 3000
New: const port = 8080
→ Replaces first occurrence only
```

**Replace All**:
```
Old: oldVariableName
New: newVariableName
Replace all: true
→ Replaces ALL occurrences
```

**When to Use replace_all**:
- Renaming variables
- Updating function names
- Changing repeated strings
- Global find-replace

#### Multi-Line Edits

**Complex Changes**:
```javascript
Old string:
app.get('/users/:id', (req, res) => {
  const user = users.find(u => u.id === req.params.id)
  res.json(user)
})

New string:
app.get('/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id)
    if (!user) {
      return res.status(404).json({ error: 'User not found' })
    }
    res.json(user)
  } catch (error) {
    res.status(500).json({ error: error.message })
  }
})
```

#### Handling Edit Failures

**Common Reasons Edit Fails**:

1. **String Not Found**:
   ```
   Error: "old_string not found in file"
   ```
   - Typo in old string
   - File changed since last read
   - Whitespace mismatch

2. **Not Unique**:
   ```
   Error: "old_string appears multiple times"
   ```
   - Need to use replace_all: true
   - Or provide more context to make unique

**Recovery**:
```
1. Re-read the file
2. Copy exact string from Read output
3. Try edit again
4. If keeps failing, consider Write instead
```

#### Best Practices for Edit

**1. Provide Enough Context**:
```
Bad (might not be unique):
Old: return true

Good (unique):
Old:
function isValid(email) {
  return true
}
```

**2. Match Whitespace Exactly**:
```
Use the exact indentation from the file
```

**3. Include Surrounding Code for Uniqueness**:
```
Old:
  const result = calculate()
  return result

Instead of just:
  return result
```

**4. Read Before Edit**:
```
Always read file first to see exact formatting
```

#### Chaining Edits

**Multiple Changes**:
```
You: "Update app.js to:
1. Change port to 8080
2. Add error handling
3. Update response format"

Claude:
  [Edit 1: Change port]
  [Edit 2: Add error handling]
  [Edit 3: Update response]
```

**Sequential Edits**:
Each edit builds on previous

#### Edit vs Write Decision

**Use Edit When**:
- Changing small sections
- Updating specific functions
- Modifying configuration values
- Adding new imports
- Updating comments

**Use Write When**:
- File is completely restructured
- Edit keeps failing (whitespace issues)
- Easier to rewrite than edit
- Creating from scratch

### Exercise

**Task**: Master file editing

**Setup**:
```bash
cd ~/claude-practice
cat > calculator.js << 'EOF'
function add(a, b) {
  return a + b
}

function subtract(a, b) {
  return a - b
}

const result = add(5, 3)
console.log(result)
EOF

claude
```

**Tasks**:

1. **Simple Edit**:
```
"Read calculator.js, then change the add function to handle
string conversion (parseFloat)"
```

2. **Multi-line Edit**:
```
"Update the subtract function to include error handling for
non-number inputs"
```

3. **Add New Function**:
```
"Add multiply and divide functions after subtract"
```

4. **Replace All**:
```
"Rename 'result' to 'sum' everywhere in the file"
```

5. **Complex Edit**:
```
"Wrap all functions in a Calculator class and export it"
```

### Verification

Before moving on, ensure you can:
- [ ] Use Edit to modify specific sections
- [ ] Understand exact string matching
- [ ] Read diffs correctly
- [ ] Use replace_all for renaming
- [ ] Handle edit failures gracefully

### Common Mistakes

1. **Whitespace Mismatch**:
   - Copy exact string from Read output
   - Pay attention to tabs vs spaces

2. **Not Unique Enough**:
   - Include more context
   - Or use replace_all

3. **Editing Without Reading First**:
   - Always read to see current state
   - Verify exact formatting

### Pro Tips

**Incremental Changes**:
```
Make one logical change at a time
Easier to review and verify
```

**Use Diffs**:
```
Always review the diff before approving
Catch unintended changes
```

**When Edit Keeps Failing**:
```
Just use Write to replace the whole function/section
Sometimes easier than fighting whitespace
```

---

*Continue with Lessons 9-12 in similar detail...*

Would you like me to:
1. Continue with remaining lessons in Section 2 (Lessons 9-12)?
2. Create detailed lesson content for Intermediate sections?
3. Build project templates with starter code?
4. Create example skills and hooks you can use?

Let me know and I'll continue building out the bootcamp!