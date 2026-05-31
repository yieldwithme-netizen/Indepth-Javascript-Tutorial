# What is Documentation?

## Definition

Documentation is **written content that explains how code works**, how to use it, and why decisions were made.

## Types of Documentation

| Type | Purpose | Audience |
|------|---------|----------|
| **README** | Project overview | New users |
| **JSDoc** | Function documentation | Developers |
| **API Docs** | Endpoint documentation | API consumers |
| **Architecture** | System design | Team members |
| **Tutorials** | Learning guides | Beginners |

## README Template

```markdown
# Project Name

Brief description of what this project does.

## Installation

```bash
npm install project-name
```

## Usage

```javascript
import { myFunction } from 'project-name';

const result = myFunction('input');
console.log(result);
```

## API Reference

### myFunction(input)
- `input` (string): The input value
- Returns: The processed result

## Contributing

1. Fork the repository
2. Create your feature branch
3. Submit a pull request

## License

MIT
```

## JSDoc Comments

```javascript
/**
 * Calculates the total price including tax.
 * @param {number} price - The base price
 * @param {number} taxRate - Tax rate as decimal (e.g., 0.1 for 10%)
 * @returns {number} The total price with tax
 * @throws {Error} If price or taxRate is negative
 * @example
 * const total = calculateTotal(100, 0.1);
 * console.log(total); // 110
 */
function calculateTotal(price, taxRate) {
  if (price < 0 || taxRate < 0) {
    throw new Error("Price and tax rate must be non-negative");
  }
  return price * (1 + taxRate);
}

/**
 * @typedef {Object} User
 * @property {number} id - User ID
 * @property {string} name - User name
 * @property {string} email - User email
 */

/**
 * Fetches a user by ID.
 * @param {number} id - The user ID
 * @returns {Promise<User>} The user object
 */
async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}
```

## Inline Comments

```javascript
// BAD: Obvious comment
let count = 0; // Initialize count to 0

// GOOD: Explain why, not what
// Retry up to 3 times due to flaky network
const MAX_RETRIES = 3;

// GOOD: Explain complex logic
// Use Fisher-Yates shuffle for unbiased randomness
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}
```

## API Documentation

```markdown
## POST /api/users

Creates a new user.

### Request Body
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "role": "admin"
}
```

### Response (201 Created)
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "admin",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

### Error Responses
- 400: Invalid input
- 409: Email already exists
```

## Documentation Tools

| Tool | Type |
|------|------|
| Swagger/OpenAPI | REST API docs |
| TypeDoc | TypeScript docs |
| JSDoc | JavaScript docs |
| Storybook | Component docs |
| MkDocs | Project docs |

## Common Mistakes

```javascript
// BAD: Outdated documentation
/**
 * @deprecated Use newFunction() instead
 * (but newFunction doesn't exist)
 */
function oldFunction() {}

// GOOD: Keep docs updated
/**
 * Calculates tax for a purchase.
 * @param {number} amount - Purchase amount
 * @returns {number} Tax amount
 */
function calculateTax(amount) {
  return amount * 0.1;
}

// BAD: Redundant comments
// Add 1 to counter
counter++;

// GOOD: Meaningful comments
// Account for zero-based indexing
counter++;
```

## Quick Revision

- README is the first thing users see
- JSDoc documents functions with types
- Explain WHY, not WHAT in comments
- Keep documentation updated
- Use tools like Swagger for API docs
- Documentation is part of the code, not separate

---

## Related Topics

- [[What-is-CleanCode]] - Clean code
- [[What-is-CodeReview]] - Code review
- [[What-is-DRY]] - DRY principle
- [[Use-Git]] - Git workflows
- [[What-is-Deployment]] - Deployment