# How to Write Documentation

## Definition

Documentation is written material that explains how software works, how to use it, and how to contribute to it. Good documentation helps developers understand code faster, reduces onboarding time, and makes maintenance easier. It can range from inline comments to comprehensive API references.

## Types of Documentation

1. **Inline Comments** - Explanations within code
2. **JSDoc** - JavaScript documentation standard
3. **README Files** - Project overview and setup
4. **API Documentation** - Endpoint references
5. **Tutorials** - Step-by-step learning guides
6. **Changelogs** - Version history and updates

## Inline Comments

Use comments to explain **why** something is done, not **what** is done.

```javascript
// BAD: Redundant comment
// Increment counter by 1
counter++;

// GOOD: Explains why
// Counter starts at 1 because user IDs are 1-indexed in legacy database
counter++;

// BAD: Obvious comment
// Check if array is empty
if (array.length === 0) {
  return;
}

// GOOD: Explains business logic
// Skip processing if no items to prevent unnecessary API calls
// that would increase costs on the billing API
if (array.length === 0) {
  return;
}

// TODO comments for future work
// TODO: Implement pagination when dataset exceeds 1000 items
// FIXME: Temporary workaround for CORS issue - remove after backend update
// HACK: Quick fix for deadline - needs proper implementation
```

## JSDoc Comments

JSDoc provides a standardized way to document JavaScript code.

```javascript
/**
 * Calculates the total price including tax and discounts.
 *
 * @param {number} price - The base price of the item
 * @param {number} [taxRate=0.1] - The tax rate (default: 10%)
 * @param {number} [discount=0] - The discount amount
 * @returns {number} The final price after tax and discount
 * @throws {Error} If price is negative
 *
 * @example
 * // Returns 105
 * calculateTotal(100, 0.1, 5);
 *
 * @example
 * // Returns 110 (uses default tax rate)
 * calculateTotal(100);
 */
function calculateTotal(price, taxRate = 0.1, discount = 0) {
  if (price < 0) {
    throw new Error('Price cannot be negative');
  }
  const tax = price * taxRate;
  return price + tax - discount;
}

/**
 * @typedef {Object} User
 * @property {number} id - The user's unique identifier
 * @property {string} name - The user's full name
 * @property {string} email - The user's email address
 * @property {Date} createdAt - Account creation date
 */

/**
 * Fetches a user by their ID from the database.
 *
 * @param {number} userId - The unique identifier of the user
 * @returns {Promise<User|null>} The user object or null if not found
 */
async function getUserById(userId) {
  // Implementation
}

/**
 * An event emitter for handling user authentication events.
 *
 * @example
 * const auth = new AuthEmitter();
 * auth.on('login', (user) => console.log(`${user.name} logged in`));
 */
class AuthEmitter extends EventEmitter {
  // Implementation
}
```

## README Structure

A good README should include:

```markdown
# Project Name

Brief description of what this project does.

## Features

- Feature 1
- Feature 2
- Feature 3

## Prerequisites

- Node.js >= 14.0.0
- npm >= 6.0.0

## Installation

```bash
npm install project-name
```

## Usage

```javascript
const projectName = require('project-name');

// Basic usage example
const result = projectName.doSomething();
```

## API Reference

### `doSomething(param)`

Description of the method.

**Parameters:**
- `param` (string): Description

**Returns:**
- (string): Description

## Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| debug | boolean | false | Enable debug mode |

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License
```

## API Documentation Template

```javascript
/**
 * @api {post} /users Create User
 * @apiName CreateUser
 * @apiGroup Users
 * @apiVersion 1.0.0
 *
 * @apiParam {String} name User's full name
 * @apiParam {String} email User's email address
 * @apiParam {String} password User's password (min 8 characters)
 *
 * @apiSuccess {Number} id User's unique ID
 * @apiSuccess {String} name User's full name
 * @apiSuccess {String} email User's email address
 * @apiSuccess {Date} createdAt Account creation timestamp
 *
 * @apiSuccessExample Success-Response:
 *     HTTP/1.1 201 Created
 *     {
 *       "id": 123,
 *       "name": "John Doe",
 *       "email": "john@example.com",
 *       "createdAt": "2024-01-15T10:30:00Z"
 *     }
 *
 * @apiError {String} message Error description
 *
 * @apiErrorExample Error-Response:
 *     HTTP/1.1 400 Bad Request
 *     {
 *       "message": "Email already exists"
 *     }
 */
app.post('/users', createUser);
```

## Changelog Format

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [2.1.0] - 2024-01-15

### Added
- User authentication system
- Password reset functionality
- Email notifications

### Changed
- Improved API response times by 40%
- Updated dependencies to latest versions

### Fixed
- Fixed memory leak in event listeners
- Resolved race condition in data synchronization

### Removed
- Deprecated v1 API endpoints

## [2.0.0] - 2024-01-01

### Added
- Initial v2 release
- New database schema
- REST API endpoints
```

## Common Use Cases

- Documenting public APIs for other developers
- Writing onboarding guides for new team members
- Creating knowledge base articles
- Maintaining changelogs for version control
- Building help systems for end users
- Recording architectural decisions

## Common Mistakes

- Writing outdated documentation that doesn't match code
- Over-commenting obvious code
- Missing documentation for edge cases
- Not documenting return values and error conditions
- Using inconsistent documentation formats
- Writing documentation only after project completion

## Related Topics

- [[JSDoc]]
- [[Code-Comments]]
- [[API-Design]]
- [[README]]
- [[Changelog]]
- [[Version-Control]]
- [[TypeScript]]

## Quick Revision

| Document Type | Purpose |
|--------------|---------|
| Inline Comments | Explain complex logic within code |
| JSDoc | Document functions, classes, and types |
| README | Project overview and getting started guide |
| API Docs | Endpoint references and usage examples |
| Changelog | Track version history and changes |
| Tutorials | Step-by-step learning instructions |
