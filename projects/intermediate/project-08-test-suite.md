# Project 8: Comprehensive Test Suite Generator

**Level**: Intermediate
**Time**: 8-12 hours
**Goal**: Create a complete test suite for an existing project using Claude

## Overview

Take an untested or partially tested codebase and create a comprehensive test suite covering unit tests, integration tests, and edge cases.

## Prerequisites

- Completed Lessons 26-40
- Testing framework knowledge (Jest recommended)
- An existing project to test

## Project Structure

```
project/
├── src/
│   ├── utils/
│   ├── services/
│   └── api/
├── tests/
│   ├── unit/
│   │   ├── utils/
│   │   └── services/
│   ├── integration/
│   └── fixtures/
├── jest.config.js
├── CLAUDE.md
└── .claude/
    └── skills/
        └── test-generator/
```

## Phase 1: Test Infrastructure (2 hours)

### Task 1.1: Analyze Testing Needs
```
"Analyze the src/ directory and identify:
1. All testable functions and modules
2. Dependencies that need mocking
3. External services to stub
4. Database operations to handle"
```

### Task 1.2: Configure Jest
```
"Create a jest.config.js that:
- Handles ES modules or TypeScript
- Sets up code coverage
- Configures test paths
- Sets up any needed transforms"
```

### Task 1.3: Create Test Utilities
```
"Create tests/fixtures/ with:
- Sample data for tests
- Mock generators
- Common test helpers"
```

---

## Phase 2: Unit Tests (4 hours)

### Task 2.1: Utility Function Tests
```
"Create unit tests for src/utils/ covering:
- All exported functions
- Edge cases (null, undefined, empty)
- Error conditions
- Boundary values"
```

### Task 2.2: Service Tests
```
"Create unit tests for src/services/ that:
- Mock all external dependencies
- Test success and failure paths
- Verify function contracts
- Test with various inputs"
```

### Task 2.3: API Handler Tests
```
"Create unit tests for API handlers covering:
- Request validation
- Response formatting
- Error handling
- Authentication checks"
```

---

## Phase 3: Integration Tests (3 hours)

### Task 3.1: API Integration Tests
```
"Create integration tests that:
- Test complete API flows
- Use test database or mocks
- Verify response structures
- Test authentication flow"
```

### Task 3.2: Service Integration
```
"Create tests that verify:
- Service interactions work together
- Data flows correctly through layers
- Error propagation works"
```

---

## Phase 4: Test Coverage & Quality (2 hours)

### Task 4.1: Coverage Analysis
```
"Run coverage report and:
1. Identify untested code paths
2. Add tests for uncovered branches
3. Target 80%+ coverage"
```

### Task 4.2: Edge Case Tests
```
"Add tests for edge cases:
- Invalid inputs
- Concurrent operations
- Resource exhaustion
- Timeout scenarios"
```

### Task 4.3: Create Test Skill
Create `.claude/skills/test-generator/SKILL.md`:

```markdown
---
name: test-generator
description: Generate tests for code
commands:
  - test
  - test-file
tools:
  - Read
  - Write
  - Glob
  - Grep
---

# Test Generator Skill

## Usage
`/test [file-path]` - Generate tests for specified file

## Process
1. Read the source file
2. Identify all testable functions
3. Generate comprehensive tests
4. Include edge cases
5. Add appropriate mocks

## Test Structure
\`\`\`javascript
describe('[FunctionName]', () => {
  describe('success cases', () => {
    it('should...', () => {});
  });

  describe('error cases', () => {
    it('should throw when...', () => {});
  });

  describe('edge cases', () => {
    it('should handle empty input', () => {});
  });
});
\`\`\`
```

---

## Phase 5: Documentation & CI (1 hour)

### Task 5.1: Test Documentation
```
"Create docs/TESTING.md with:
- How to run tests
- Test organization
- Adding new tests
- Mocking guidelines"
```

### Task 5.2: CI Integration
```
"Create GitHub Action for tests:
- Run on PR and push
- Report coverage
- Fail on coverage decrease"
```

---

## Deliverables

1. **Unit Tests** (20+ test files)
   - 100% function coverage target
   - Edge cases covered
   - Mocks for external deps

2. **Integration Tests** (5+ test files)
   - Complete API flows
   - Service interactions
   - Database operations

3. **Test Fixtures** (Reusable test data)
   - Sample objects
   - Mock factories
   - Test helpers

4. **Test Skill** (Reusable generator)
   - Works for new files
   - Consistent test style
   - Includes edge cases

5. **Documentation**
   - TESTING.md guide
   - Coverage reports
   - CI configuration

## Success Criteria

- [ ] All tests pass
- [ ] 80%+ code coverage
- [ ] Integration tests work
- [ ] Test skill generates valid tests
- [ ] CI pipeline functional

## Test Categories Checklist

### Unit Tests
- [ ] Pure functions
- [ ] Class methods
- [ ] Async functions
- [ ] Error handlers
- [ ] Validators

### Integration Tests
- [ ] API endpoints
- [ ] Database operations
- [ ] External services
- [ ] Authentication
- [ ] Authorization

### Edge Cases
- [ ] Null/undefined inputs
- [ ] Empty arrays/objects
- [ ] Very large inputs
- [ ] Special characters
- [ ] Concurrent access

## Example Test Generated

```javascript
// tests/unit/utils/validator.test.js
const { validateEmail } = require('../../../src/utils/validator');

describe('validateEmail', () => {
  describe('valid emails', () => {
    it('should accept standard email format', () => {
      expect(validateEmail('user@example.com')).toBe(true);
    });

    it('should accept email with subdomain', () => {
      expect(validateEmail('user@mail.example.com')).toBe(true);
    });
  });

  describe('invalid emails', () => {
    it('should reject email without @', () => {
      expect(validateEmail('userexample.com')).toBe(false);
    });

    it('should reject email without domain', () => {
      expect(validateEmail('user@')).toBe(false);
    });
  });

  describe('edge cases', () => {
    it('should return false for null', () => {
      expect(validateEmail(null)).toBe(false);
    });

    it('should return false for undefined', () => {
      expect(validateEmail(undefined)).toBe(false);
    });

    it('should return false for empty string', () => {
      expect(validateEmail('')).toBe(false);
    });
  });
});
```

## Tips

1. Start with pure utility functions (easiest)
2. Work up to complex integrations
3. Use descriptive test names
4. Mock at boundaries, not everywhere
5. Test behavior, not implementation
