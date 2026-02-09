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

## Lesson 9: Finding Files with Glob Tool

### Objectives
- Understand the Glob tool and glob patterns
- Find files by name, extension, and path
- Use wildcards for flexible searching
- Combine Glob with Read for efficient exploration

### Content

#### The Glob Tool

The **Glob tool** finds files by matching patterns against file paths. It's Claude's go-to when it needs to locate files in your project.

**What Glob Does**:
- Finds files matching a pattern
- Returns matching file paths sorted by modification time
- Works with any codebase size
- Fast — much quicker than reading directories manually

**When Claude Uses Glob**:
```
You: "Find all JavaScript files in src/"
Claude: [Uses Glob with pattern "src/**/*.js"]
```

#### Glob Pattern Syntax

**Basic Wildcards**:

| Pattern | Matches | Example |
|---------|---------|---------|
| `*` | Any characters in one segment | `*.js` → `app.js`, `index.js` |
| `**` | Any number of directories | `**/*.js` → `src/app.js`, `src/lib/utils.js` |
| `?` | Single character | `app.?s` → `app.js`, `app.ts` |
| `{a,b}` | Either a or b | `*.{js,ts}` → `app.js`, `app.ts` |

**Common Patterns**:

```
# All JavaScript files
**/*.js

# All files in src/ directory
src/**/*

# All test files
**/*.test.js
**/*.spec.ts

# All config files
**/config.*
**/.*.json

# TypeScript and JavaScript
**/*.{js,ts,jsx,tsx}

# Specific directories
src/components/**/*.tsx
tests/**/*.test.js
```

#### Practical Examples

**Finding Source Files**:
```
You: "Find all TypeScript files in the project"
Claude: [Glob pattern: **/*.ts]
→ src/index.ts
→ src/app.ts
→ src/routes/users.ts
→ src/models/User.ts
```

**Finding Config Files**:
```
You: "Where are the configuration files?"
Claude: [Glob pattern: **/{config,*.config}.*]
→ webpack.config.js
→ jest.config.ts
→ src/config.json
```

**Finding Tests**:
```
You: "Find all test files"
Claude: [Glob pattern: **/*.{test,spec}.{js,ts,jsx,tsx}]
→ tests/auth.test.js
→ tests/users.spec.ts
→ src/__tests__/app.test.tsx
```

#### Combining Glob with Read

**Exploration Pattern**:
```
You: "Find all route files and show me the API endpoints"

Claude:
  [Glob: src/routes/**/*.js]
  → Found: users.js, auth.js, products.js
  [Read: src/routes/users.js]
  [Read: src/routes/auth.js]
  [Read: src/routes/products.js]
  → "Here are all the API endpoints..."
```

**Bug Investigation**:
```
You: "Find all files that import the database module"

Claude:
  [Glob: src/**/*.js]
  [Grep: "import.*database" in those files]
  [Read: matching files]
  → "These files import the database module..."
```

#### Glob vs Other Search Methods

| Method | Best For |
|--------|----------|
| **Glob** | Finding files by name/path pattern |
| **Grep** | Finding files by content |
| **Bash `ls`** | Listing directory contents |
| **Bash `find`** | Complex criteria (size, date) |

**Rule of thumb**: Know the file name? Use Glob. Know what's inside? Use Grep.

### Exercise

**Task**: Master file discovery with Glob

**Setup**:
```bash
cd ~/claude-practice
mkdir -p myproject/src/components myproject/src/utils myproject/tests myproject/config
touch myproject/src/index.js myproject/src/app.ts
touch myproject/src/components/Header.tsx myproject/src/components/Footer.tsx
touch myproject/src/utils/helpers.js myproject/src/utils/format.ts
touch myproject/tests/app.test.js myproject/tests/helpers.test.js
touch myproject/config/dev.json myproject/config/prod.json
touch myproject/package.json myproject/tsconfig.json
cd myproject
claude
```

**Tasks**:

1. **Find by Extension**:
```
"Find all TypeScript files in this project"
```

2. **Find in Specific Directory**:
```
"Find all files in the components directory"
```

3. **Find by Naming Pattern**:
```
"Find all test files"
```

4. **Find Config Files**:
```
"Find all JSON configuration files"
```

5. **Combined Search**:
```
"Find all TypeScript files in src/ and read each one"
```

### Verification

Before moving on, ensure you can:
- [ ] Understand basic glob patterns (`*`, `**`, `?`, `{}`)
- [ ] Find files by extension
- [ ] Search in specific directories
- [ ] Combine Glob with Read
- [ ] Choose between Glob and Grep

### Common Mistakes

1. **Using `*` When You Need `**`**:
   - `*.js` only matches root-level JS files
   - `**/*.js` matches JS files at any depth

2. **Forgetting File Extensions**:
   - `src/**/*` matches everything including non-code files
   - Be specific: `src/**/*.{js,ts}`

3. **Not Using Glob Before Reading**:
   - Don't guess file paths
   - Glob first, then Read the results

### Pro Tips

**Discovery Workflow**:
```
"First find all route files, then read each one and
summarize the API structure"
```

**Multi-Pattern Search**:
```
"Find all .js and .ts files but exclude node_modules and dist"
```

**Combine with Context**:
```
"Find all files that were recently modified (glob sorts by
modification time)"
```

---

## Lesson 10: Searching Content with Grep Tool

### Objectives
- Use Grep to search inside files
- Understand regex patterns for search
- Filter results with output modes
- Combine Grep with other tools

### Content

#### The Grep Tool

The **Grep tool** searches for patterns inside file contents. While Glob finds files by name, Grep finds files by what's *inside* them.

**What Grep Does**:
- Searches file contents using regex patterns
- Filters by file type or glob pattern
- Returns matching files, lines, or counts
- Built on ripgrep for speed

**When Claude Uses Grep**:
```
You: "Find where the user model is defined"
Claude: [Uses Grep with pattern "class User" or "const User"]
```

#### Grep Output Modes

**1. `files_with_matches`** (default) — just file paths:
```
You: "Which files mention authentication?"
Claude: [Grep: "authentication"]
→ src/auth.js
→ src/middleware/auth.js
→ docs/security.md
```

**2. `content`** — matching lines with context:
```
You: "Show me where PORT is defined"
Claude: [Grep: "PORT.*=", output_mode: "content"]
→ src/config.js:5: const PORT = process.env.PORT || 3000
→ src/app.js:12:   app.listen(PORT, () => {
```

**3. `count`** — how many matches per file:
```
You: "How many TODO comments are in the codebase?"
Claude: [Grep: "TODO", output_mode: "count"]
→ src/auth.js: 3
→ src/app.js: 1
→ src/utils.js: 5
```

#### Regex Patterns

**Common Search Patterns**:

```
# Literal string
"authentication"

# Function definition
"function\\s+\\w+"

# Import statement
"import.*from"

# Variable assignment
"const PORT\\s*="

# Class definition
"class\\s+User"

# TODO/FIXME comments
"(TODO|FIXME|HACK)"

# Email pattern
"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+"
```

#### Filtering Results

**By File Type**:
```
You: "Search for 'fetch' only in JavaScript files"
Claude: [Grep: "fetch", type: "js"]
```

**By Glob Pattern**:
```
You: "Search for 'database' only in config files"
Claude: [Grep: "database", glob: "**/config.*"]
```

**With Context Lines**:
```
You: "Show me the error handling around the database connection"
Claude: [Grep: "database.*connect", -C: 5]
→ Shows 5 lines before and after each match
```

#### Practical Search Patterns

**Finding Dead Code**:
```
You: "Find all exported functions, then find which ones
are never imported anywhere"

Claude:
  [Grep: "export (function|const|class)" → list of exports]
  [For each export: Grep for import references]
  → "These exports appear unused..."
```

**Security Audit**:
```
You: "Search for potential security issues"

Claude:
  [Grep: "eval\\("]
  [Grep: "innerHTML"]
  [Grep: "password.*=.*['\"]"]
  → "Found potential security concerns..."
```

**Dependency Usage**:
```
You: "How is the lodash library used in our code?"

Claude:
  [Grep: "from 'lodash'" or "require.*lodash"]
  [Read matching files]
  → "Lodash is used in these files for..."
```

#### Grep vs Glob Decision

```
Question: "Where is the User model?"
→ Try Glob first: **/*User*.{js,ts}
→ If not found, try Grep: "class User" or "model.*User"

Question: "Which files handle authentication?"
→ Grep: "auth|authenticate|login"

Question: "Find all test files"
→ Glob: **/*.test.{js,ts}
```

### Exercise

**Task**: Master content searching

**Setup**:
```bash
cd ~/claude-practice
mkdir -p search-demo/src

cat > search-demo/src/app.js << 'EOF'
const express = require('express')
const { connectDB } = require('./database')

// TODO: Add rate limiting
const app = express()
const PORT = process.env.PORT || 3000

app.get('/users', async (req, res) => {
  const users = await getUsers()
  res.json(users)
})

// FIXME: Add proper error handling
app.listen(PORT, () => {
  console.log(`Server on ${PORT}`)
})
EOF

cat > search-demo/src/database.js << 'EOF'
const mongoose = require('mongoose')

// TODO: Move connection string to env
const DB_URL = 'mongodb://localhost:27017/myapp'

async function connectDB() {
  await mongoose.connect(DB_URL)
  console.log('Database connected')
}

module.exports = { connectDB }
EOF

cat > search-demo/src/auth.js << 'EOF'
const jwt = require('jsonwebtoken')
const SECRET = 'my-secret-key'

// TODO: Use environment variable for secret
function authenticate(token) {
  return jwt.verify(token, SECRET)
}

function generateToken(user) {
  return jwt.sign({ id: user.id }, SECRET)
}

module.exports = { authenticate, generateToken }
EOF

cd search-demo
claude
```

**Tasks**:

1. **Basic Search**:
```
"Find all files that contain 'TODO' comments"
```

2. **Content Search**:
```
"Show me all TODO and FIXME comments with their surrounding code"
```

3. **Pattern Search**:
```
"Find all require() statements across the project"
```

4. **Filtered Search**:
```
"Search for 'connect' only in JavaScript files"
```

5. **Security Search**:
```
"Find any hardcoded secrets or credentials in the code"
```

### Verification

Before moving on, ensure you can:
- [ ] Search for text patterns in files
- [ ] Use different output modes (files, content, count)
- [ ] Filter searches by file type or glob
- [ ] Use context lines to see surrounding code
- [ ] Combine Grep with Read for investigation

### Common Mistakes

1. **Too Broad a Search**:
   - `"a"` matches almost everything
   - Be specific: `"async function"` instead of `"function"`

2. **Not Using File Type Filters**:
   - Searching all files including `node_modules`
   - Use `type: "js"` or `glob: "src/**"`

3. **Confusing Grep and Glob**:
   - Glob = file names/paths
   - Grep = file contents

### Pro Tips

**Case-Insensitive Search**:
```
Grep with -i flag: "error" matches "Error", "ERROR", etc.
```

**Multiline Search**:
```
Use multiline: true to match patterns spanning multiple lines
```

**Combine for Full Picture**:
```
"Grep for 'database' to find all related files,
then read each one to understand the full database layer"
```

---

## Lesson 11: Running Commands with Bash Tool

### Objectives
- Understand when Claude uses Bash
- Learn safe command execution
- Run builds, tests, and scripts
- Understand permission model for commands

### Content

#### The Bash Tool

The **Bash tool** lets Claude execute terminal commands. It's how Claude runs builds, installs packages, runs tests, and performs system operations.

**What Bash Can Do**:
- Run npm/yarn/pnpm commands
- Execute tests
- Build projects
- Install dependencies
- Run scripts
- Git operations
- Docker commands
- Any terminal command

**When Claude Uses Bash**:
```
You: "Install express and run the tests"
Claude:
  [Bash: npm install express]
  [Bash: npm test]
```

#### Permission Model

Claude asks permission before running commands, especially anything that changes your system.

**Auto-Allowed** (common, safe operations):
- `ls`, `pwd`, `echo`
- `git status`, `git diff`, `git log`
- Reading operations

**Requires Permission** (changes to files/system):
- `npm install`
- `git commit`, `git push`
- `rm`, `mkdir`
- Running scripts
- Database commands

**You Can Configure**:
```
# Allow all npm commands
Permission: npm *

# Allow tests
Permission: npm test, jest, pytest
```

#### Safe Command Execution

**Claude's Safety Rules**:
1. Never runs destructive commands without permission
2. Avoids `rm -rf` on important directories
3. Won't run commands that leak secrets
4. Warns about irreversible operations
5. Prefers dedicated tools over Bash when available

**Example — Safe Approach**:
```
You: "Delete the old build directory"

Claude:
  "I'll remove the build/ directory. This will delete all
  compiled files. Shall I proceed?"

  [Bash: rm -rf build/]  ← Asks permission first
```

#### Common Bash Workflows

**Package Management**:
```
You: "Set up this project"
Claude:
  [Bash: npm install]
  → "Dependencies installed. Found 3 vulnerabilities..."
  [Bash: npm audit fix]
  → "Fixed 2 vulnerabilities"
```

**Running Tests**:
```
You: "Run the tests"
Claude:
  [Bash: npm test]
  → "Tests: 12 passed, 2 failed"
  → "Let me look at the failing tests..."
```

**Build & Verify**:
```
You: "Build the project and make sure it works"
Claude:
  [Bash: npm run build]
  → "Build succeeded"
  [Bash: npm start]
  → "Server running on port 3000"
```

**Git Operations**:
```
You: "Show me what's changed"
Claude:
  [Bash: git status]
  [Bash: git diff]
  → "Here's what changed..."
```

#### Bash Tool Limitations

**What to Watch Out For**:
- **Working directory resets** between calls (use absolute paths or `&&` chaining)
- **Interactive commands don't work** (`vim`, `nano`, interactive prompts)
- **Timeout**: Commands timeout after 2 minutes by default
- **Output truncation**: Very long output gets truncated

**Chaining Commands**:
```bash
# Good — single Bash call with chaining
npm install && npm run build && npm test

# Also works — sequential commands
npm install; npm run build; npm test
```

**Long-Running Commands**:
```
You: "Run the full test suite (takes ~5 minutes)"
Claude:
  [Bash with extended timeout: npm run test:full]
  → Uses timeout parameter to allow more time
```

#### When NOT to Use Bash

Claude prefers dedicated tools over Bash for many operations:

| Task | Don't Use | Use Instead |
|------|-----------|-------------|
| Read files | `cat file.js` | **Read** tool |
| Search content | `grep -r "pattern"` | **Grep** tool |
| Find files | `find . -name "*.js"` | **Glob** tool |
| Edit files | `sed -i 's/old/new/'` | **Edit** tool |
| Create files | `echo "content" > file` | **Write** tool |

**Bash is for**: system commands, package managers, builds, tests, git, scripts.

### Exercise

**Task**: Practice command execution

**Setup**:
```bash
cd ~/claude-practice
mkdir bash-demo && cd bash-demo
npm init -y
cat > index.js << 'EOF'
console.log("Hello from bash-demo!")
EOF
cat > math.js << 'EOF'
function add(a, b) { return a + b }
function subtract(a, b) { return a - b }
module.exports = { add, subtract }
EOF
claude
```

**Tasks**:

1. **Install Dependencies**:
```
"Install jest as a dev dependency"
```

2. **Create and Run Tests**:
```
"Create a test file for math.js and run the tests"
```

3. **Check Git Status**:
```
"Initialize a git repo and show me the status"
```

4. **Run a Script**:
```
"Add a 'start' script to package.json and run it"
```

5. **Chain Commands**:
```
"Install dependencies, run tests, and show git status"
```

### Verification

Before moving on, ensure you can:
- [ ] Understand when Claude uses Bash vs other tools
- [ ] Run package management commands
- [ ] Execute tests and interpret results
- [ ] Chain commands together
- [ ] Know which operations need permission

### Common Mistakes

1. **Using Bash for File Operations**:
   - Claude has dedicated Read/Write/Edit/Glob/Grep tools
   - Bash should be for system commands, not file manipulation

2. **Forgetting Command Chaining**:
   - Working directory doesn't persist between Bash calls
   - Use `&&` to chain dependent commands

3. **Running Long Commands Without Timeout**:
   - Default timeout is 2 minutes
   - Request extended timeout for long builds/tests

### Pro Tips

**Background Execution**:
```
"Run the full test suite in the background while we
work on something else"
```

**Parallel Commands**:
```
"Run linting and tests in parallel"
→ Claude can make multiple Bash calls simultaneously
```

**Script Discovery**:
```
"Show me what scripts are available in package.json
and explain what each one does"
```

---

## Lesson 12: Combining Tools for Complex Tasks

### Objectives
- Understand multi-tool workflows
- Build efficient search-read-edit pipelines
- Orchestrate tools for bug fixes and features
- Develop effective prompting for multi-step tasks

### Content

#### Why Combine Tools?

Real tasks rarely use a single tool. A typical workflow involves several tools working together:

```
Bug Fix Workflow:
  Grep  → Find where the bug might be
  Read  → Understand the code
  Edit  → Fix the bug
  Bash  → Run tests to verify

Feature Addition:
  Glob  → Find related files
  Read  → Understand existing patterns
  Write → Create new file
  Edit  → Update imports and references
  Bash  → Build and test
```

#### The Search → Read → Act Pattern

The most common multi-tool pattern:

**Step 1: Search** (Glob or Grep)
```
Find relevant files
```

**Step 2: Read** (Read tool)
```
Understand the code
```

**Step 3: Act** (Edit, Write, or Bash)
```
Make changes or run commands
```

**Example — Finding and Fixing a Bug**:
```
You: "Users report that the login endpoint returns 500 errors"

Claude:
  [Grep: "login" in src/]
  → Found: src/routes/auth.js, src/controllers/authController.js

  [Read: src/routes/auth.js]
  → POST /login route defined here

  [Read: src/controllers/authController.js]
  → Found: missing null check on user lookup

  [Edit: src/controllers/authController.js]
  → Added null check: if (!user) return res.status(401)...

  [Bash: npm test]
  → All tests pass

  "Fixed! The login controller wasn't checking for null user
  results from the database query."
```

#### Common Multi-Tool Workflows

**1. Code Exploration**:
```
You: "Explain the authentication system"

Claude:
  [Glob: **/*auth*.*]         → Find auth-related files
  [Read: each file]           → Understand the code
  → Provides comprehensive explanation
```

**2. Refactoring**:
```
You: "Rename UserService to UserManager everywhere"

Claude:
  [Grep: "UserService"]       → Find all references
  [Read: each file]           → Understand context
  [Edit: each file]           → Replace references (replace_all)
  [Bash: npm test]            → Verify nothing broke
```

**3. Adding a Feature**:
```
You: "Add a /health endpoint to the API"

Claude:
  [Glob: src/routes/**/*.js]  → Find route files
  [Read: existing route]       → Understand pattern
  [Edit: src/routes/index.js]  → Add health route
  [Write: tests/health.test.js] → Create test
  [Bash: npm test]             → Run tests
```

**4. Dependency Update**:
```
You: "Update lodash and fix any breaking changes"

Claude:
  [Bash: npm outdated]        → Check current version
  [Grep: "lodash" in src/]    → Find all usages
  [Read: each file]           → Understand how it's used
  [Bash: npm install lodash@latest] → Update
  [Bash: npm test]            → Check for breakage
  [Edit: files as needed]     → Fix any breaking changes
```

#### Prompting for Multi-Tool Tasks

**Be Specific About the Goal**:
```
Bad:  "Fix the code"
Good: "Find the authentication bug causing 500 errors,
      fix it, and run the tests to verify"
```

**Provide Context**:
```
Bad:  "Add a feature"
Good: "Add a GET /health endpoint that returns
      { status: 'ok', uptime: process.uptime() }
      following the same pattern as other routes"
```

**Request Verification**:
```
"After making the change, run the tests and show
me the git diff of what you changed"
```

#### Efficiency Tips

**1. Let Claude Explore First**:
```
"Explore the project structure, then tell me how
the data flows from API to database"
```

**2. Request Parallel Operations**:
```
"Read these 5 files in parallel and summarize
the architecture"
```

**3. Batch Related Changes**:
```
"Update all API endpoints to use the new response
format in one pass"
```

**4. Ask for a Plan First**:
```
"Before making changes, tell me your plan for
refactoring the auth module"
```

### Exercise

**Task**: Complete a multi-tool workflow

**Setup**:
```bash
cd ~/claude-practice
mkdir -p multi-tool-demo/src multi-tool-demo/tests

cat > multi-tool-demo/src/app.js << 'EOF'
const express = require('express')
const app = express()

app.get('/users', (req, res) => {
  res.json([
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
  ])
})

app.get('/users/:id', (req, res) => {
  const users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
  ]
  const user = users.find(u => u.id === parseInt(req.params.id))
  res.json(user)
})

app.listen(3000)
EOF

cat > multi-tool-demo/package.json << 'EOF'
{
  "name": "multi-tool-demo",
  "version": "1.0.0",
  "scripts": {
    "start": "node src/app.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
EOF

cd multi-tool-demo
claude
```

**Tasks** (each uses multiple tools):

1. **Bug Fix Workflow**:
```
"The GET /users/:id endpoint crashes when the user is
not found. Find the bug, fix it, and add a test."
```

2. **Feature Addition**:
```
"Add a POST /users endpoint. Follow the same patterns
as existing endpoints. Add validation, error handling,
and a test."
```

3. **Refactoring**:
```
"The user data is hardcoded. Extract it to a separate
data module, update the routes to use it, and make
sure everything still works."
```

4. **Code Review**:
```
"Review all the code in src/ for security issues,
missing error handling, and best practices violations.
Fix everything you find."
```

5. **Documentation**:
```
"Explore the project, then create a comprehensive
README.md with API documentation, setup instructions,
and example usage."
```

### Verification

Before moving on, ensure you can:
- [ ] Describe the Search → Read → Act pattern
- [ ] Prompt Claude for multi-step workflows
- [ ] Understand tool selection for different subtasks
- [ ] Request verification after changes
- [ ] Write prompts that guide efficient tool usage

### Common Mistakes

1. **Being Too Vague**:
   - Claude needs clear goals to choose the right tools
   - Specify what to find, change, and verify

2. **Not Requesting Verification**:
   - Always ask Claude to test after making changes
   - "Run the tests" should be part of most workflows

3. **Not Providing Context**:
   - "Fix the API" → Claude doesn't know which API or what's wrong
   - "Fix the 500 error on POST /users" → Clear and actionable

### Pro Tips

**Multi-Step Prompt Template**:
```
"I need you to:
1. [Search/Find]: Locate the relevant code
2. [Understand]: Read and analyze it
3. [Change]: Make the modification
4. [Verify]: Test that it works
5. [Report]: Show me what changed"
```

**Requesting a Plan**:
```
"Before you start, outline your plan for this change.
Then I'll approve it and you can proceed."
```

**Iterative Workflow**:
```
"Let's work on this step by step.
First, find all files related to the payment system."
[Review results]
"Good. Now read the main payment controller."
[Review code]
"Now fix the double-charge bug on line 45."
```

---

## Section 2 Summary

You've now mastered Claude Code's core file tools:

| Tool | Purpose | Key Usage |
|------|---------|-----------|
| **Read** | View file contents | Understanding code, reviewing changes |
| **Write** | Create/replace files | New files, boilerplate, full rewrites |
| **Edit** | Modify existing files | Targeted changes, bug fixes, updates |
| **Glob** | Find files by pattern | File discovery, project exploration |
| **Grep** | Search file contents | Code search, pattern finding, auditing |
| **Bash** | Run terminal commands | Builds, tests, packages, git |
| **Combined** | Multi-tool workflows | Real-world tasks use multiple tools |

### What's Next?

In **Section 3: Git Integration**, you'll learn how Claude works with Git for commits, branches, pull requests, and code review — building on these file tools to manage your entire development workflow.