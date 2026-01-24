# Exercise 10: Create Your First Skill

**Time**: 2 hours
**Prerequisites**: Lessons 33-35

## Objective
Create a functional, reusable skill

## Task

Build a "find-todos" skill that:
- Searches codebase for TODO/FIXME comments
- Reports location (file:line)
- Categorizes by priority
- Generates report

## Implementation

**Location**: `~/.claude/skills/find-todos/SKILL.md`

**Frontmatter**:
```yaml
name: find-todos
description: Find and report TODO comments
commands:
  - find-todos
  - todos
tools:
  - Grep
  - Read
```

**Instructions**:
1. Use Grep to find TODO/FIXME
2. Read files for context
3. Categorize (TODO, FIXME, HACK, XXX)
4. Generate markdown report

**Output Format**:
```markdown
# TODO Report

## Critical (FIXME)
- file.js:45 - Description

## High (TODO)
- file.js:23 - Description

## Notes (XXX, HACK)
- file.js:67 - Description
```

## Testing
1. Create test files with TODOs
2. Run `/find-todos`
3. Verify all TODOs found
4. Check report format

## Success Criteria
- [ ] Skill file created correctly
- [ ] Finds all TODO types
- [ ] Report is formatted well
- [ ] Can be invoked with `/find-todos`
