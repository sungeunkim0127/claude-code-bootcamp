# Project 1: Todo CLI Application

**Level**: Beginner
**Time**: 2-3 hours
**Prerequisites**: Lessons 1-12

---

## 🎯 Project Goals

Build a command-line todo application to practice:
- File operations (Read, Write, Edit)
- Command execution (Bash)
- Git workflow
- Code organization
- Testing basics

---

## 📋 Requirements

### Core Features
- [ ] Add todo items
- [ ] List all todos
- [ ] Mark todos as complete
- [ ] Delete todos
- [ ] Save todos to file (persistence)

### Technical Requirements
- [ ] Use Node.js
- [ ] Store todos in JSON file
- [ ] Command-line interface
- [ ] Error handling
- [ ] At least 5 unit tests
- [ ] README with usage examples

---

## 🏗️ Project Structure

```
todo-cli/
├── src/
│   ├── index.js          # CLI entry point
│   ├── todoManager.js    # Core logic
│   └── storage.js        # File operations
├── tests/
│   └── todoManager.test.js
├── data/
│   └── todos.json
├── package.json
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Step 1: Initialize Project

**Ask Claude**:
```
"Create a new Node.js project called todo-cli with:
- package.json (name: todo-cli, version: 1.0.0)
- .gitignore (node_modules, data/)
- README.md with project description
- Initialize git repository"
```

### Step 2: Create Storage Module

**Ask Claude**:
```
"Create src/storage.js that:
- Reads todos from data/todos.json
- Writes todos to data/todos.json
- Creates file if it doesn't exist
- Handles errors gracefully
- Exports loadTodos() and saveTodos(todos)"
```

**Example API**:
```javascript
const { loadTodos, saveTodos } = require('./storage')

// Load all todos
const todos = await loadTodos()
// Returns: [{ id: 1, text: 'Buy milk', done: false }, ...]

// Save todos
await saveTodos(todos)
```

### Step 3: Create Todo Manager

**Ask Claude**:
```
"Create src/todoManager.js with a TodoManager class that has:
- addTodo(text) - adds new todo
- listTodos() - returns all todos
- completeTodo(id) - marks todo as done
- deleteTodo(id) - removes todo
- Uses storage module for persistence
- Generates unique IDs
- Validates inputs"
```

**Example Usage**:
```javascript
const TodoManager = require('./todoManager')
const manager = new TodoManager()

await manager.addTodo('Buy groceries')
await manager.completeTodo(1)
const todos = await manager.listTodos()
```

### Step 4: Create CLI

**Ask Claude**:
```
"Create src/index.js as the CLI entry point:
- Use process.argv for command parsing
- Commands:
  - node src/index.js add 'todo text'
  - node src/index.js list
  - node src/index.js complete <id>
  - node src/index.js delete <id>
- Colorful output (use chalk if you want)
- Help command showing usage
- Error messages for invalid commands"
```

**Example**:
```bash
$ node src/index.js add "Buy milk"
✓ Added: Buy milk (ID: 1)

$ node src/index.js list
📝 Todos:
  [1] ☐ Buy milk
  [2] ☑ Read book (completed)

$ node src/index.js complete 1
✓ Completed: Buy milk
```

### Step 5: Add Tests

**Ask Claude**:
```
"Create tests/todoManager.test.js using Jest:
- Test adding todos
- Test listing todos
- Test completing todos
- Test deleting todos
- Test error cases (invalid ID, empty text, etc.)
- Use beforeEach to reset state
- Mock the storage module

Also update package.json with jest dependency and test script"
```

### Step 6: Documentation

**Ask Claude**:
```
"Update README.md with:
- Project description
- Installation instructions
- Usage examples for all commands
- API documentation
- Development setup (how to run tests)
- Screenshots of CLI output (as code blocks)"
```

### Step 7: Git Workflow

**Ask Claude**:
```
"Let's commit this project:
1. Show me git status
2. Create meaningful commit for initial implementation
3. Create a feature branch called 'add-priority'
4. Implement priority levels (low, medium, high) for todos
5. Commit the feature
6. Merge back to main"
```

---

## 🎨 Enhancement Ideas

Once basic version works, add:

### Enhancement 1: Priority Levels
```
"Add priority field to todos (low, medium, high):
- Update TodoManager to accept priority
- Sort todos by priority when listing
- Add color coding in CLI output
- Update tests"
```

### Enhancement 2: Due Dates
```
"Add due date support:
- Accept due date when adding todo
- Show overdue todos in red
- Sort by due date
- Add 'due-soon' command for items due within 7 days"
```

### Enhancement 3: Categories
```
"Add category/tag support:
- Todos can have multiple tags
- List todos by tag
- Filter commands (list --tag work)
- Update data structure and tests"
```

### Enhancement 4: Search
```
"Add search functionality:
- Search todos by text
- Filter by status (done/undone)
- Combine filters (tag + status + search)
- Case-insensitive search"
```

---

## ✅ Verification Checklist

### Functionality
- [ ] Can add todos
- [ ] Can list todos
- [ ] Can complete todos
- [ ] Can delete todos
- [ ] Todos persist across runs
- [ ] Help command works
- [ ] Errors handled gracefully

### Code Quality
- [ ] Code is organized (separate modules)
- [ ] Error handling present
- [ ] Input validation
- [ ] No hardcoded values
- [ ] Comments where needed

### Testing
- [ ] At least 5 tests
- [ ] All tests pass
- [ ] Edge cases covered
- [ ] Can run `npm test`

### Documentation
- [ ] README is comprehensive
- [ ] Usage examples clear
- [ ] Installation instructions work
- [ ] API documented

### Git
- [ ] Git initialized
- [ ] .gitignore present
- [ ] Meaningful commits
- [ ] Feature branch workflow used

---

## 🎓 Learning Objectives

By completing this project, you will have:

✅ **Used All Core Tools**:
- Read (reading existing code)
- Write (creating new files)
- Edit (modifying code)
- Bash (git commands, npm commands)

✅ **Practiced Git Workflow**:
- Initialize repository
- Make commits
- Create branches
- Merge branches

✅ **Built Complete Application**:
- Multiple modules
- CLI interface
- Data persistence
- Error handling

✅ **Written Tests**:
- Unit tests
- Test structure
- Assertions

✅ **Created Documentation**:
- README
- Code comments
- Usage examples

---

## 💡 Tips

### Working with Claude

**Break It Down**:
```
Don't ask for everything at once
Build incrementally:
1. Storage module
2. TodoManager
3. CLI
4. Tests
5. Documentation
```

**Review Each Step**:
```
After each major component:
- Test it manually
- Ask Claude to explain design decisions
- Suggest improvements
```

**Use Verification**:
```
After implementing features:
"Let's test this. Create a sample todo and verify it persists"
```

### Common Issues

**Issue**: Todos don't persist
**Solution**: Check data/todos.json is being written

**Issue**: Tests fail
**Solution**: Mock storage module properly

**Issue**: CLI doesn't parse arguments
**Solution**: Log process.argv to debug

---

## 🚀 Next Steps

After completing this project:

1. **Enhance It**: Add features from Enhancement Ideas
2. **Publish It**: Push to GitHub
3. **Share It**: Add to your portfolio
4. **Learn More**: Move to Project 2

---

## 📸 Example Session

```bash
# Initialize
$ npm install
$ node src/index.js help

Todo CLI - Manage your todos from the command line

Commands:
  add <text>       Add a new todo
  list             List all todos
  complete <id>    Mark todo as complete
  delete <id>      Delete a todo
  help             Show this help

# Add todos
$ node src/index.js add "Buy groceries"
✓ Added: Buy groceries (ID: 1)

$ node src/index.js add "Write blog post"
✓ Added: Write blog post (ID: 2)

# List todos
$ node src/index.js list
📝 Todos:
  [1] ☐ Buy groceries
  [2] ☐ Write blog post

# Complete
$ node src/index.js complete 1
✓ Completed: Buy groceries

# List again
$ node src/index.js list
📝 Todos:
  [1] ☑ Buy groceries
  [2] ☐ Write blog post

# Tests
$ npm test
✓ should add a todo
✓ should list todos
✓ should complete a todo
✓ should delete a todo
✓ should handle errors

5 passing (25ms)
```

---

**Ready to build? Start with Step 1 and work your way through!**

**Estimated time**: 2-3 hours
**Difficulty**: Beginner
**Skills practiced**: All core tools, Git workflow, testing, documentation
