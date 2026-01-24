# CLAUDE.md Template

CLAUDE.md is a special file that provides project context to Claude Code. Place it in your project root.

---

## Basic Template

```markdown
# Project Name

## Project Overview
[Brief description of what this project does]

## Tech Stack
- **Language**: [e.g., TypeScript, Python, Rust]
- **Framework**: [e.g., React, Express, FastAPI]
- **Database**: [e.g., PostgreSQL, MongoDB]
- **Other**: [e.g., Redis, Docker]

## Project Structure
```
project-root/
├── src/           # Source code
├── tests/         # Tests
├── docs/          # Documentation
└── config/        # Configuration
```

## Development Workflow
[How to work on this project]

## Important Conventions
[Project-specific conventions Claude should follow]
```

---

## Comprehensive Template

```markdown
# Project Name

> Brief tagline or description

## Table of Contents
- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Development Workflow](#development-workflow)
- [Code Conventions](#code-conventions)
- [Testing Strategy](#testing-strategy)
- [Common Tasks](#common-tasks)
- [Important Notes](#important-notes)

---

## Overview

### What This Project Does
[2-3 sentences describing the project's purpose]

### Key Features
- Feature 1
- Feature 2
- Feature 3

### Current Status
- **Version**: [e.g., v1.2.0]
- **Stage**: [e.g., Production, Beta, Development]
- **Active Development**: [Yes/No]

---

## Tech Stack

### Core Technologies
- **Language**: [e.g., TypeScript 5.x]
- **Runtime**: [e.g., Node.js 18+]
- **Framework**: [e.g., Express 4.x]
- **Database**: [e.g., PostgreSQL 15]

### Key Dependencies
- **Authentication**: [e.g., Passport.js, JWT]
- **ORM**: [e.g., Prisma, TypeORM]
- **Testing**: [e.g., Jest, Vitest]
- **Validation**: [e.g., Zod, Joi]

### Development Tools
- **Linting**: [e.g., ESLint]
- **Formatting**: [e.g., Prettier]
- **Type Checking**: [e.g., TypeScript]
- **Pre-commit**: [e.g., Husky]

---

## Architecture

### High-Level Architecture
[Describe the overall architecture - monolith, microservices, etc.]

### Design Patterns Used
- [e.g., Repository Pattern for data access]
- [e.g., Factory Pattern for object creation]
- [e.g., Middleware Pattern for request processing]

### Key Architectural Decisions
1. **Decision**: [e.g., Using PostgreSQL over MongoDB]
   **Reason**: [e.g., Need for ACID transactions and complex queries]

2. **Decision**: [e.g., Monolithic architecture]
   **Reason**: [e.g., Team size and simplicity requirements]

---

## Project Structure

```
project-root/
├── src/
│   ├── api/           # API routes and controllers
│   ├── services/      # Business logic
│   ├── models/        # Data models
│   ├── middleware/    # Express middleware
│   ├── utils/         # Utility functions
│   └── types/         # TypeScript type definitions
├── tests/
│   ├── unit/          # Unit tests
│   ├── integration/   # Integration tests
│   └── e2e/           # End-to-end tests
├── docs/              # Additional documentation
├── scripts/           # Build and deployment scripts
└── config/            # Configuration files
```

### Important Files
- **src/index.ts**: Application entry point
- **src/api/routes.ts**: API route definitions
- **src/services/**: Core business logic
- **prisma/schema.prisma**: Database schema

### File Naming Conventions
- **Services**: `[entity]Service.ts` (e.g., `userService.ts`)
- **Controllers**: `[entity]Controller.ts`
- **Models**: `[entity]Model.ts`
- **Tests**: `[filename].test.ts`
- **Types**: `[entity].types.ts`

---

## Development Workflow

### Setup
```bash
# Install dependencies
npm install

# Set up database
npm run db:migrate

# Seed database (optional)
npm run db:seed

# Start development server
npm run dev
```

### Running Tests
```bash
# All tests
npm test

# Unit tests only
npm run test:unit

# Integration tests
npm run test:integration

# Watch mode
npm run test:watch
```

### Building
```bash
# Build for production
npm run build

# Type check
npm run type-check

# Lint
npm run lint

# Format
npm run format
```

### Database Operations
```bash
# Create migration
npm run db:migrate:create

# Run migrations
npm run db:migrate

# Rollback migration
npm run db:migrate:rollback

# Seed database
npm run db:seed
```

---

## Code Conventions

### Style Guide
- **Indentation**: 2 spaces
- **Quotes**: Single quotes for strings
- **Semicolons**: Yes
- **Trailing commas**: Yes

### Naming Conventions

**Variables & Functions**:
```typescript
// camelCase
const userEmail = 'user@example.com'
function getUserById(id: string) { }
```

**Classes & Interfaces**:
```typescript
// PascalCase
class UserService { }
interface UserData { }
```

**Constants**:
```typescript
// UPPER_SNAKE_CASE
const MAX_RETRY_ATTEMPTS = 3
```

**Files & Folders**:
```
// camelCase for files
userService.ts
// kebab-case for folders
user-management/
```

### Code Organization

**Import Order**:
```typescript
// 1. External dependencies
import express from 'express'
import { PrismaClient } from '@prisma/client'

// 2. Internal modules
import { UserService } from '@/services/userService'
import { authenticate } from '@/middleware/auth'

// 3. Types
import type { User, UserCreateData } from '@/types/user.types'

// 4. Utils and constants
import { logger } from '@/utils/logger'
import { MAX_PAGE_SIZE } from '@/constants'
```

**Function Structure**:
```typescript
// 1. Input validation
// 2. Business logic
// 3. Error handling
// 4. Return value

async function createUser(data: UserCreateData): Promise<User> {
  // Validate input
  validateUserData(data)

  // Business logic
  const user = await userService.create(data)

  // Return
  return user
}
```

### Error Handling Pattern

```typescript
// Use custom error classes
throw new ValidationError('Invalid email format')
throw new NotFoundError('User not found')
throw new UnauthorizedError('Invalid credentials')

// Handle errors at boundaries
try {
  await performOperation()
} catch (error) {
  logger.error('Operation failed', { error })
  throw new ServiceError('Failed to complete operation')
}
```

### Async/Await Pattern

```typescript
// Prefer async/await over promises
async function fetchUserData(id: string): Promise<User> {
  const user = await db.user.findUnique({ where: { id } })
  if (!user) throw new NotFoundError('User not found')
  return user
}

// Not: promises with .then()
```

---

## Testing Strategy

### Test Structure
```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create a new user with valid data', async () => {
      // Arrange
      const userData = { email: 'test@example.com', name: 'Test' }

      // Act
      const user = await userService.createUser(userData)

      // Assert
      expect(user).toBeDefined()
      expect(user.email).toBe(userData.email)
    })

    it('should throw ValidationError for invalid email', async () => {
      // Arrange
      const userData = { email: 'invalid', name: 'Test' }

      // Act & Assert
      await expect(userService.createUser(userData))
        .rejects.toThrow(ValidationError)
    })
  })
})
```

### Test Coverage Goals
- **Overall**: >80%
- **Services**: >90%
- **Utils**: >95%
- **Controllers**: >70%

### What to Test
- ✅ Business logic (services)
- ✅ Validation logic
- ✅ Error handling
- ✅ Edge cases
- ✅ Integration points
- ❌ Simple getters/setters
- ❌ Third-party library internals

---

## Common Tasks

### When Adding a New Feature

1. **Create Feature Branch**:
```bash
git checkout -b feature/feature-name
```

2. **Write Tests First** (TDD):
```typescript
// tests/unit/services/featureService.test.ts
describe('FeatureService', () => {
  it('should do the thing', () => {
    // Test here
  })
})
```

3. **Implement Feature**:
```typescript
// src/services/featureService.ts
export class FeatureService {
  async doThing() {
    // Implementation
  }
}
```

4. **Add API Endpoint** (if needed):
```typescript
// src/api/routes/feature.ts
router.post('/feature', authenticate, handleFeature)
```

5. **Update Documentation**:
```markdown
# docs/api.md - add API docs
# README.md - update if needed
```

6. **Run Full Test Suite**:
```bash
npm test
npm run lint
npm run type-check
```

7. **Create PR**:
Follow PR template, request review

### When Fixing a Bug

1. **Create Bug Branch**:
```bash
git checkout -b fix/bug-description
```

2. **Write Failing Test**:
```typescript
it('should not error when [bug condition]', () => {
  // Test that currently fails
})
```

3. **Fix the Bug**:
Modify code until test passes

4. **Verify**:
```bash
npm test
# Ensure all tests pass
```

5. **Document**:
Add comment explaining the fix if non-obvious

### When Refactoring

1. **Ensure Tests Pass**:
```bash
npm test
# All green before starting
```

2. **Refactor Incrementally**:
Small changes, run tests after each

3. **No Behavior Changes**:
Tests should still pass without modification

4. **Document Changes**:
Update comments and docs as needed

---

## Important Notes

### Security Considerations
- Never commit `.env` files
- Never log sensitive data (passwords, tokens)
- Always use parameterized queries (no string concatenation)
- Validate all user input
- Use HTTPS in production

### Performance Considerations
- Database queries should use indexes
- Use pagination for large result sets (limit: 100 per page)
- Cache frequently accessed data (TTL: 5 minutes)
- Optimize images before upload (max: 2MB)

### Known Issues
1. **Issue**: [Description]
   **Workaround**: [Temporary solution]
   **Tracking**: [Issue #123]

### Dependencies to Avoid
- ❌ `package-name` - reason why
- ❌ `another-package` - use `alternative` instead

### Deployment Notes
- **Production**: Deploy via CI/CD (GitHub Actions)
- **Staging**: Auto-deploy on merge to `develop`
- **Database Migrations**: Run before deploying code
- **Environment Variables**: Set in deployment platform

---

## Quick Reference

### Useful Commands
```bash
# Development
npm run dev              # Start dev server
npm run dev:debug        # Start with debugger

# Testing
npm test                 # Run all tests
npm run test:watch       # Watch mode
npm run test:coverage    # Generate coverage report

# Database
npm run db:migrate       # Run migrations
npm run db:reset         # Reset database

# Deployment
npm run build            # Build for production
npm run start            # Start production server
```

### Important Endpoints
- **Health Check**: `GET /health`
- **API Docs**: `GET /api/docs`
- **Metrics**: `GET /metrics` (requires auth)

### Environment Variables
```bash
# Required
DATABASE_URL=postgresql://...
JWT_SECRET=...
PORT=3000

# Optional
REDIS_URL=redis://...
LOG_LEVEL=info
```

---

## Contact & Resources

### Team
- **Lead**: [Name]
- **Backend**: [Name]
- **Frontend**: [Name]

### Resources
- **API Docs**: [link]
- **Design Docs**: [link]
- **Monitoring**: [link]
- **Error Tracking**: [link]

### Getting Help
- **Slack**: #project-channel
- **Issues**: GitHub Issues
- **Docs**: `/docs` folder

---

**Last Updated**: [Date]
**Maintainer**: [Name]
```

---

## Minimal Template (For Small Projects)

```markdown
# Project Name

Quick description.

## Stack
TypeScript + Express + PostgreSQL

## Setup
```bash
npm install
npm run dev
```

## Structure
- `src/api/` - Routes
- `src/services/` - Business logic
- `src/models/` - Data models

## Conventions
- Use async/await
- camelCase for variables
- Write tests for services
- Lint before commit

## Common Tasks
```bash
npm test           # Run tests
npm run build      # Build
npm run db:migrate # Migrate DB
```
```

---

## Advanced: Dynamic Context

```markdown
# Project Context

## Current State

**Branch**: `$(git branch --show-current)`

**Recent Changes**:
```
$(git log -5 --oneline)
```

**Modified Files**:
```
$(git status --short)
```

## Active Tasks

Current sprint tasks:
- [ ] Task from JIRA/Linear/etc.
- [ ] Another task

## Notes

Last deployment: [date]
Known issues: See GitHub Issues
```

---

## Tips for Great CLAUDE.md

### 1. Be Specific
Don't just say "use TypeScript" - specify version, tsconfig settings, type patterns.

### 2. Include Examples
Show code examples of your conventions.

### 3. Update Regularly
Keep it current as the project evolves.

### 4. Cover Common Scenarios
Include "when adding a feature" or "when fixing a bug" sections.

### 5. Link to Other Docs
Don't duplicate - link to existing documentation.

### 6. Mention What NOT to Do
Anti-patterns are as important as patterns.

### 7. Keep It Scannable
Use headers, lists, code blocks. Make it easy to find information.

---

**Create CLAUDE.md to give Claude the context it needs to work effectively on your project!**
