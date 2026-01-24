# Express REST API Project

> Modern Express.js REST API with MongoDB, JWT authentication, and comprehensive testing

---

## 🎯 Project Overview

This is a production-ready REST API built with Express.js for a task management application. Users can create accounts, manage tasks, collaborate with teams, and track project progress.

### Current Status
- **Version**: v2.3.1
- **Stage**: Production
- **Last Deploy**: 2026-01-20
- **Active Development**: Yes (v3.0 planning)

---

## 🛠️ Tech Stack

### Core
- **Runtime**: Node.js 18.x LTS
- **Language**: JavaScript (ES modules)
- **Framework**: Express.js 4.18.x
- **Database**: MongoDB 6.0

### Key Dependencies
```json
{
  "express": "^4.18.0",
  "mongoose": "^7.0.0",
  "jsonwebtoken": "^9.0.0",
  "bcrypt": "^5.1.0",
  "express-validator": "^7.0.0",
  "helmet": "^7.0.0",
  "cors": "^2.8.5",
  "dotenv": "^16.0.0"
}
```

### Development Tools
- **Testing**: Jest + Supertest
- **Linting**: ESLint (Airbnb config)
- **Formatting**: Prettier
- **Pre-commit**: Husky + lint-staged
- **Logging**: Winston
- **Monitoring**: New Relic (production)

---

## 📂 Project Structure

```
project-root/
├── src/
│   ├── api/
│   │   ├── routes/           # Route definitions
│   │   ├── controllers/      # Request handlers
│   │   └── middleware/       # Express middleware
│   ├── services/             # Business logic
│   ├── models/               # Mongoose models
│   ├── utils/                # Utility functions
│   ├── config/               # Configuration
│   └── index.js              # Application entry
├── tests/
│   ├── unit/                 # Unit tests
│   ├── integration/          # Integration tests
│   └── e2e/                  # End-to-end tests
├── docs/                     # API documentation
├── scripts/                  # Build/deploy scripts
└── config/                   # Environment configs
```

### File Organization

**Routes**: `src/api/routes/`
- `auth.routes.js` - Authentication endpoints
- `users.routes.js` - User management
- `tasks.routes.js` - Task CRUD
- `teams.routes.js` - Team collaboration

**Controllers**: `src/api/controllers/`
- `authController.js` - Auth logic
- `userController.js` - User operations
- `taskController.js` - Task operations
- `teamController.js` - Team operations

**Services**: `src/services/`
- `authService.js` - Auth business logic
- `userService.js` - User operations
- `taskService.js` - Task management
- `emailService.js` - Email notifications

**Models**: `src/models/`
- `User.js` - User schema & methods
- `Task.js` - Task schema & methods
- `Team.js` - Team schema & methods

---

## 🚀 Development Workflow

### Setup
```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env
# Edit .env with your values

# Start MongoDB (Docker)
docker-compose up -d mongo

# Run migrations
npm run db:migrate

# Seed database (optional)
npm run db:seed

# Start development server
npm run dev
```

### Development Commands
```bash
npm run dev              # Start with nodemon
npm run dev:debug        # Start with Node inspector
npm test                 # Run all tests
npm run test:watch       # Run tests in watch mode
npm run test:coverage    # Generate coverage report
npm run lint             # Run ESLint
npm run lint:fix         # Fix linting issues
npm run format           # Run Prettier
npm run build            # Build for production
npm start                # Start production server
```

### Database Commands
```bash
npm run db:migrate       # Run migrations
npm run db:migrate:undo  # Rollback last migration
npm run db:seed          # Seed database
npm run db:reset         # Reset database (⚠️ drops all data)
```

---

## 💻 Code Conventions

### Style Guide

**ESLint**: Airbnb JavaScript Style Guide
**Prettier**: 2 spaces, single quotes, trailing commas

### Naming Conventions

**Variables & Functions**: camelCase
```javascript
const userId = req.params.id
const authenticateUser = async (email, password) => { }
```

**Classes & Constructors**: PascalCase
```javascript
class TaskService { }
class UserModel extends Model { }
```

**Constants**: UPPER_SNAKE_CASE
```javascript
const MAX_LOGIN_ATTEMPTS = 5
const TOKEN_EXPIRY_HOURS = 24
```

**Files**: camelCase for files, kebab-case for folders
```
userService.js
task-management/
```

**Routes**: kebab-case
```
/api/v1/user-tasks
/api/v1/team-members
```

### Import Organization

```javascript
// 1. Node built-ins
import fs from 'fs'
import path from 'path'

// 2. External dependencies
import express from 'express'
import mongoose from 'mongoose'

// 3. Internal modules - config
import config from './config/index.js'

// 4. Internal modules - services
import { UserService } from './services/userService.js'
import { TaskService } from './services/taskService.js'

// 5. Internal modules - middleware
import { authenticate } from './middleware/auth.js'
import { validateRequest } from './middleware/validation.js'

// 6. Internal modules - utils
import { logger } from './utils/logger.js'
import { AppError } from './utils/errors.js'
```

### Function Structure

```javascript
/**
 * Standard function structure
 */
async function processTask(taskId, userId) {
  // 1. Input validation
  if (!taskId || !userId) {
    throw new AppError('Missing required parameters', 400)
  }

  // 2. Business logic
  const task = await Task.findById(taskId)
  if (!task) {
    throw new AppError('Task not found', 404)
  }

  if (task.userId !== userId) {
    throw new AppError('Unauthorized', 403)
  }

  // 3. Operations
  task.status = 'completed'
  await task.save()

  // 4. Return value
  return task
}
```

### Error Handling Pattern

```javascript
// Use custom error classes
throw new ValidationError('Invalid email format')
throw new NotFoundError('User not found')
throw new UnauthorizedError('Invalid credentials')
throw new ForbiddenError('Insufficient permissions')

// Controllers always use try-catch
const createTask = async (req, res, next) => {
  try {
    const task = await taskService.create(req.body)
    res.status(201).json({ task })
  } catch (error) {
    next(error) // Pass to error handling middleware
  }
}
```

### Async/Await Convention

```javascript
// ✅ ALWAYS use async/await
async function getUser(id) {
  const user = await User.findById(id)
  return user
}

// ❌ NEVER use .then().catch()
function getUser(id) {
  return User.findById(id)
    .then(user => user)
    .catch(err => console.error(err))
}
```

---

## 🧪 Testing Strategy

### Test Structure
```javascript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', async () => {
      // Arrange
      const userData = {
        email: 'test@example.com',
        password: 'SecurePass123!',
        name: 'Test User'
      }

      // Act
      const user = await userService.createUser(userData)

      // Assert
      expect(user).toBeDefined()
      expect(user.email).toBe(userData.email)
      expect(user.password).not.toBe(userData.password) // hashed
    })

    it('should throw ValidationError for invalid email', async () => {
      const userData = { email: 'invalid', password: 'pass', name: 'Test' }
      await expect(userService.createUser(userData))
        .rejects
        .toThrow(ValidationError)
    })
  })
})
```

### Coverage Goals
- **Overall**: >80%
- **Services**: >90% (core business logic)
- **Utils**: >95% (pure functions)
- **Controllers**: >70% (mostly integration tested)
- **Routes**: >60% (covered by integration tests)

### What to Test

✅ **Always Test**:
- Business logic (services)
- Validation logic
- Error handling
- Edge cases
- Security-critical code (auth, permissions)
- Data transformations

❌ **Don't Test**:
- Simple getters/setters
- Third-party libraries
- Configuration constants
- Auto-generated code

---

## 🔐 Security Guidelines

### Authentication
```javascript
// JWT tokens with 24-hour expiry
const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET, {
  expiresIn: '24h'
})

// Refresh tokens with 7-day expiry
const refreshToken = jwt.sign({ userId: user.id }, process.env.REFRESH_SECRET, {
  expiresIn: '7d'
})
```

### Password Security
```javascript
// Bcrypt with 12 rounds (minimum 10)
const hashedPassword = await bcrypt.hash(password, 12)
```

### Input Validation
```javascript
// ALWAYS validate with express-validator
router.post('/users',
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 8 }).matches(/[A-Z]/).matches(/[0-9]/),
  validateRequest, // Middleware that checks validation results
  createUser
)
```

### Database Security
```javascript
// ✅ ALWAYS use parameterized queries (Mongoose does this)
User.findOne({ email: userEmail })

// ❌ NEVER string concatenation (not applicable with Mongoose, but avoid in raw queries)
```

### NEVER Commit
- API keys
- Passwords
- JWT secrets
- Database credentials
- `.env` files

---

## 📊 API Conventions

### RESTful Routes
```
GET    /api/v1/tasks           # List tasks
POST   /api/v1/tasks           # Create task
GET    /api/v1/tasks/:id       # Get single task
PUT    /api/v1/tasks/:id       # Update task
DELETE /api/v1/tasks/:id       # Delete task
```

### Request Format
```json
{
  "title": "Complete API documentation",
  "description": "Write comprehensive API docs",
  "priority": "high",
  "dueDate": "2026-01-30"
}
```

### Response Format
```json
{
  "status": "success",
  "data": {
    "task": {
      "id": "507f1f77bcf86cd799439011",
      "title": "Complete API documentation",
      "status": "pending",
      "createdAt": "2026-01-24T10:00:00Z"
    }
  }
}
```

### Error Response Format
```json
{
  "status": "error",
  "message": "Task not found",
  "code": "TASK_NOT_FOUND",
  "details": {}
}
```

### Status Codes
- `200` - Success
- `201` - Created
- `204` - No Content (successful delete)
- `400` - Bad Request (validation error)
- `401` - Unauthorized (not logged in)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `409` - Conflict (duplicate resource)
- `500` - Internal Server Error

---

## 🎯 Common Tasks

### Adding a New Feature

1. **Create Branch**:
```bash
git checkout -b feature/feature-name
```

2. **Write Tests First** (TDD):
```javascript
// tests/unit/services/featureService.test.js
describe('FeatureService', () => {
  it('should do the thing', async () => {
    // Write failing test
  })
})
```

3. **Implement**:
   - Create model (if needed): `src/models/Feature.js`
   - Create service: `src/services/featureService.js`
   - Create controller: `src/api/controllers/featureController.js`
   - Create routes: `src/api/routes/feature.routes.js`
   - Register routes in `src/api/index.js`

4. **Test**:
```bash
npm test
npm run test:coverage
```

5. **Lint & Format**:
```bash
npm run lint:fix
npm run format
```

6. **Commit**:
```bash
git add .
git commit -m "feat: add feature-name

Implements feature-name with:
- Model and schema
- CRUD operations
- Validation
- Tests (coverage: 95%)

Closes #123"
```

7. **Create PR**:
```bash
gh pr create --title "feat: add feature-name" --body "$(cat <<EOF
## Summary
Adds feature-name functionality

## Changes
- Added Feature model
- Implemented CRUD operations
- Added comprehensive tests

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manually tested

EOF
)"
```

### Fixing a Bug

1. **Create Bug Branch**:
```bash
git checkout -b fix/bug-description
```

2. **Write Failing Test**:
```javascript
it('should not throw error when [bug condition]', async () => {
  // Test that currently fails
})
```

3. **Fix Bug**:
Modify code until test passes

4. **Verify**:
```bash
npm test
# All tests must pass
```

5. **Commit**:
```bash
git commit -m "fix: resolve bug-description

The issue was [explanation].
Now [expected behavior].

Fixes #456"
```

---

## ⚠️ Important Notes

### Performance Considerations
- **Database**: Always use indexes for queried fields
- **Pagination**: Limit to 100 items per page (default: 20)
- **Caching**: Cache frequent queries for 5 minutes
- **Images**: Max 5MB upload, compress before storage

### Known Issues
1. **Rate Limiting**: Currently in-memory, not suitable for multi-instance
   - **Workaround**: Use Redis for production
   - **Tracking**: Issue #234

2. **WebSocket Connection**: Disconnects after 30 minutes of inactivity
   - **Workaround**: Implement ping/pong
   - **Tracking**: Issue #267

### Dependencies to Avoid
- ❌ `moment` - Use native Date or `date-fns` instead (smaller)
- ❌ `request` - Deprecated, use `node-fetch` or `axios`
- ❌ `validator` (standalone) - Use `express-validator` (integrated)

---

## 🚢 Deployment

### Production Checklist
- [ ] All tests passing
- [ ] Linting clean
- [ ] Coverage >80%
- [ ] No console.log statements
- [ ] Environment variables documented
- [ ] Database migrations run
- [ ] Performance tested
- [ ] Security audit clean (`npm audit`)

### Environment Variables
```bash
# Required
NODE_ENV=production
PORT=3000
DATABASE_URL=mongodb://...
JWT_SECRET=...
JWT_REFRESH_SECRET=...

# Optional
REDIS_URL=redis://...
NEW_RELIC_LICENSE_KEY=...
LOG_LEVEL=info
```

### Deployment Process
1. Merge to `develop` branch → Auto-deploy to staging
2. Test on staging
3. Create release PR to `main`
4. Merge to `main` → Auto-deploy to production
5. Monitor New Relic for errors

---

## 📚 Resources

### Documentation
- **API Docs**: `/docs/api.md`
- **Database Schema**: `/docs/schema.md`
- **Architecture**: `/docs/architecture.md`

### Tools
- **Monitoring**: https://rpm.newrelic.com/...
- **Error Tracking**: https://sentry.io/...
- **CI/CD**: GitHub Actions

### Team
- **Lead**: @username
- **Backend**: @username2, @username3
- **DevOps**: @username4

### Getting Help
- **Slack**: #api-development
- **Issues**: GitHub Issues
- **Wiki**: `/docs` folder

---

**Last Updated**: 2026-01-24
**Maintainer**: @teamlead
