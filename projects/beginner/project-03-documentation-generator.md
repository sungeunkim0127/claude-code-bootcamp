# Project 3: Documentation Generator

**Level**: Beginner
**Time**: 4-6 hours
**Goal**: Use Claude to generate comprehensive documentation for an existing project

## Overview

Take an undocumented or poorly documented codebase and create professional documentation including README, API docs, and inline comments.

## Prerequisites

- Completed Lessons 1-12
- An existing project without good docs (or use a sample)
- Basic understanding of Markdown

## Project Structure

```
your-project/
├── README.md           # Main documentation (generate)
├── docs/
│   ├── API.md         # API documentation (generate)
│   ├── SETUP.md       # Setup guide (generate)
│   └── CONTRIBUTING.md # Contribution guide (generate)
├── src/
│   └── *.js           # Add inline comments
└── CLAUDE.md          # Project context
```

## Tasks

### Phase 1: Project Analysis (1 hour)

#### Task 1.1: Create CLAUDE.md
Ask Claude to analyze the project and create initial context:

```
"Analyze this project structure and create a CLAUDE.md that describes:
- What the project does
- Main technologies used
- Key files and their purposes
- Coding conventions observed"
```

#### Task 1.2: Identify Documentation Gaps
```
"What documentation is missing from this project? List:
- Essential docs that should exist
- Code areas needing comments
- Configuration that needs explanation"
```

---

### Phase 2: README Creation (1.5 hours)

#### Task 2.1: Generate README Structure
```
"Create a comprehensive README.md with:
- Project title and description
- Features list
- Installation instructions
- Usage examples
- Configuration options
- Contributing guidelines
- License information"
```

#### Task 2.2: Add Code Examples
```
"Add practical code examples to the README showing:
- Basic usage
- Common use cases
- Configuration examples"
```

#### Task 2.3: Add Badges (Optional)
```
"Add appropriate badges for:
- Build status
- License
- Version
- Test coverage"
```

---

### Phase 3: API Documentation (1.5 hours)

#### Task 3.1: Document Public Functions
```
"Create API.md documenting all public functions in src/:
- Function name
- Parameters with types
- Return value
- Usage example
- Error cases"
```

#### Task 3.2: Document Configuration
```
"Document all configuration options:
- Environment variables
- Config file options
- Default values
- Valid ranges/options"
```

#### Task 3.3: Add Examples Section
```
"Add a detailed examples section showing:
- Common patterns
- Best practices
- Error handling"
```

---

### Phase 4: Inline Documentation (1 hour)

#### Task 4.1: Add JSDoc Comments
```
"Add JSDoc comments to all functions in src/main.js:
- @description
- @param with types
- @returns with type
- @throws if applicable
- @example"
```

#### Task 4.2: Add Module Comments
```
"Add module-level comments explaining:
- Module purpose
- Dependencies
- Usage patterns"
```

---

### Phase 5: Setup & Contributing Guides (1 hour)

#### Task 5.1: Create SETUP.md
```
"Create SETUP.md with detailed setup instructions:
- Prerequisites
- Step-by-step installation
- Configuration walkthrough
- Verification steps
- Common issues and solutions"
```

#### Task 5.2: Create CONTRIBUTING.md
```
"Create CONTRIBUTING.md explaining:
- How to report bugs
- How to request features
- Development workflow
- Code style requirements
- PR process"
```

---

## Deliverables

1. **README.md** (500+ words)
   - Clear project description
   - Installation instructions
   - Usage examples
   - All standard sections

2. **docs/API.md** (Complete API reference)
   - All public functions documented
   - Parameters and returns specified
   - Examples included

3. **docs/SETUP.md** (Step-by-step guide)
   - Prerequisites listed
   - Installation steps
   - Configuration explained

4. **docs/CONTRIBUTING.md** (Contribution guide)
   - Bug reporting process
   - Development workflow
   - Code standards

5. **Inline Comments** (JSDoc in source files)
   - All public functions documented
   - Complex logic explained

## Success Criteria

- [ ] README is comprehensive and professional
- [ ] API documentation covers all public interfaces
- [ ] Setup guide allows new developer to get started
- [ ] Inline comments are accurate and helpful
- [ ] Documentation follows consistent style

## Stretch Goals

1. **Generate Changelog**: Create CHANGELOG.md from git history
2. **Architecture Diagram**: Create ASCII or Mermaid diagram
3. **Video Script**: Write script for project walkthrough video
4. **FAQ Section**: Add frequently asked questions

## Tips

1. Read files before documenting them
2. Test installation steps yourself
3. Use consistent Markdown formatting
4. Include both simple and advanced examples
5. Link between documentation files

## Common Issues

**Documentation inconsistent with code:**
- Have Claude re-read the code after changes
- Verify examples actually work

**Too verbose:**
- Ask for more concise versions
- Focus on essential information

**Missing edge cases:**
- Ask specifically about error handling
- Request documentation of limitations
