# How to Document APIs

API documentation describes how to use and integrate with an API. Good documentation reduces support costs, improves developer experience, and accelerates adoption.

## OpenAPI (Swagger) Specification

```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
  description: API for managing users
servers:
  - url: https://api.example.com/v1
paths:
  /users:
    get:
      summary: Get all users
      tags: [Users]
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      summary: Create a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUser'
      responses:
        '201':
          description: User created
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
```

## JSDoc for Function Documentation

```javascript
/**
 * Calculates the total price including tax
 * @param {number} price - The base price of the item
 * @param {number} taxRate - The tax rate as a decimal (e.g., 0.1 for 10%)
 * @returns {number} The total price including tax
 * @throws {Error} If price or taxRate is negative
 * @example
 * // Returns 110
 * calculateTotal(100, 0.1);
 */
function calculateTotal(price, taxRate) {
  if (price < 0 || taxRate < 0) {
    throw new Error('Price and tax rate must be non-negative');
  }
  return price * (1 + taxRate);
}

/**
 * @typedef {Object} User
 * @property {number} id - Unique identifier
 * @property {string} name - User's full name
 * @property {string} email - User's email address
 * @property {Date} createdAt - Account creation date
 */

/**
 * Fetches a user by their ID
 * @param {number} id - The user's ID
 * @returns {Promise<User>} The user object
 */
async function getUserById(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}
```

## JSDoc for Classes

```javascript
/**
 * Manages a collection of items with CRUD operations
 * @class
 * @template T
 */
class Collection {
  /**
   * Create a new Collection
   * @param {string} name - The collection name
   */
  constructor(name) {
    this.name = name;
    this.items = new Map();
  }

  /**
   * Add an item to the collection
   * @param {string} id - Unique identifier for the item
   * @param {T} item - The item to add
   * @returns {void}
   */
  add(id, item) {
    this.items.set(id, item);
  }

  /**
   * Get an item by ID
   * @param {string} id - The item's ID
   * @returns {T|undefined} The item or undefined if not found
   */
  get(id) {
    return this.items.get(id);
  }

  /**
   * Remove an item by ID
   * @param {string} id - The item's ID
   * @returns {boolean} True if item was removed, false otherwise
   */
  remove(id) {
    return this.items.delete(id);
  }
}
```

## REST API Documentation Template

```javascript
/**
 * @api {post} /api/users Create User
 * @apiName CreateUser
 * @apiGroup Users
 * @apiVersion 1.0.0
 *
 * @apiParam {String} name User's full name
 * @apiParam {String} email User's email address
 * @apiParam {String} password User's password (min 8 chars)
 *
 * @apiSuccess {Object} user Created user object
 * @apiSuccess {Number} user.id Unique user ID
 * @apiSuccess {String} user.name User's name
 * @apiSuccess {String} user.email User's email
 *
 * @apiSuccessExample Success Response:
 *     HTTP/1.1 201 Created
 *     {
 *       "id": 1,
 *       "name": "John Doe",
 *       "email": "john@example.com"
 *     }
 *
 * @apiError ExampleError Description
 *
 * @apiErrorExample Error Response:
 *     HTTP/1.1 400 Bad Request
 *     {
 *       "error": "Email already exists"
 *     }
 */
```

## TypeScript API Types

```typescript
interface ApiResponse<T> {
  data: T;
  meta: {
    page: number;
    limit: number;
    total: number;
  };
}

interface ApiError {
  code: string;
  message: string;
  details?: Record<string, string[]>;
}

interface CreateUserRequest {
  name: string;
  email: string;
  password: string;
}

interface UserResponse {
  id: number;
  name: string;
  email: string;
  createdAt: string;
}

// API client with typed methods
class ApiClient {
  /**
   * Create a new user
   * @param data - User creation data
   * @returns Promise resolving to the created user
   */
  async createUser(data: CreateUserRequest): Promise<UserResponse> {
    const response = await fetch('/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return response.json();
  }
}
```

## Common Use Cases

- **SDK development**: Documenting library methods and classes
- **API reference**: Providing endpoint documentation for developers
- **Code maintenance**: Making code understandable for future developers
- **Onboarding**: Helping new team members understand the codebase
- **Code generation**: Tools like Swagger UI auto-generate interactive docs

## Common Mistakes

1. **No documentation at all** - Leaving developers guessing
2. **Outdated documentation** - Docs that don't match current code
3. **Missing parameter descriptions** - Not explaining what each parameter does
4. **No examples** - Not showing how to use the API
5. **Overly verbose** - Being too wordy and losing clarity
6. **Ignoring error cases** - Not documenting error responses

## Related Topics

- [[Implement-Auth]]
- [[What-is-RateLimit]]
- [[What-is-OWASP]]
- [[Store-Secrets]]

## Quick Revision

| Tool | Purpose |
|------|---------|
| OpenAPI/Swagger | REST API specification format |
| JSDoc | JavaScript documentation comments |
| TypeDoc | TypeScript documentation generator |
| API Blueprint | API documentation standard |
| Redoc | OpenAPI documentation renderer |

**Documentation Checklist:**
- [ ] All endpoints documented
- [ ] Request/response examples provided
- [ ] Error responses documented
- [ ] Authentication requirements explained
- [ ] Rate limits documented
- [ ] Code samples in multiple languages
