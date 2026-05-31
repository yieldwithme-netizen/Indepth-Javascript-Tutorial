# Named Exports

## Definition

**Named Exports** allow you to export multiple values from a module by name. Unlike default exports, named exports require you to use the exact name when importing, or you can rename them using the `as` keyword.

Named exports are ideal for utility libraries, constants, and modules that expose multiple related functions or values.

## Basic Syntax

### Declaring Named Exports

```javascript
// Inline export
export const PI = 3.14159;
export function add(a, b) { return a + b; }

// Separate export (at end of file)
const PI = 3.14159;
function add(a, b) { return a + b; }
export { PI, add };
```

### Importing Named Exports

```javascript
// Import specific names
import { PI, add } from './math.js';

// Rename during import
import { PI as piValue, add as addNumbers } from './math.js';

// Import all as namespace
import * as math from './math.js';
console.log(math.PI);
console.log(math.add(2, 3));
```

## Common Use Cases

### 1. Utility Function Library
```javascript
// validators.js
export function isEmail(str) {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(str);
}

export function isPhoneNumber(str) {
  const phoneRegex = /^\d{10}$/;
  return phoneRegex.test(str);
}

export function isEmpty(value) {
  if (value === null || value === undefined) return true;
  if (typeof value === 'string') return value.trim() === '';
  if (Array.isArray(value)) return value.length === 0;
  if (typeof value === 'object') return Object.keys(value).length === 0;
  return false;
}

// form.js
import { isEmail, isEmpty } from './validators.js';

function validateForm(email, name) {
  const errors = [];
  if (isEmpty(name)) errors.push('Name is required');
  if (!isEmail(email)) errors.push('Invalid email');
  return errors;
}
```

### 2. Constants Module
```javascript
// constants.js
export const HTTP_METHODS = {
  GET: 'GET',
  POST: 'POST',
  PUT: 'PUT',
  DELETE: 'DELETE'
};

export const STATUS_CODES = {
  OK: 200,
  CREATED: 201,
  BAD_REQUEST: 400,
  UNAUTHORIZED: 401,
  NOT_FOUND: 404,
  SERVER_ERROR: 500
};

export const MAX_FILE_SIZE = 5 * 1024 * 1024; // 5MB
export const ALLOWED_EXTENSIONS = ['.jpg', '.png', '.gif', '.pdf'];

// api.js
import { HTTP_METHODS, STATUS_CODES } from './constants.js';

async function fetchData(url) {
  const response = await fetch(url);
  if (response.status === STATUS_CODES.OK) {
    return response.json();
  }
  throw new Error(`HTTP Error: ${response.status}`);
}
```

### 3. Re-exports for Barrel Pattern
```javascript
// utils/index.js
export { isEmail, isPhoneNumber, isEmpty } from './validators';
export { formatDate, parseDate, daysBetween } from './dateUtils';
export { capitalize, truncate, slugify } from './stringUtils';

// app.js
import { isEmail, formatDate, capitalize } from './utils/index.js';
```

### 4. Class and Type Exports
```javascript
// models.js
export class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  toString() {
    return `${this.name} <${this.email}>`;
  }
}

export class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }

  formatPrice() {
    return `$${this.price.toFixed(2)}`;
  }
}

export const ROLES = Object.freeze({
  ADMIN: 'admin',
  USER: 'user',
  GUEST: 'guest'
});
```

## Advanced Patterns

### 1. Conditional Exports
```javascript
// config.js
const isDev = process.env.NODE_ENV === 'development';

export const API_URL = isDev 
  ? 'http://localhost:3000/api'
  : 'https://api.production.com';

export const DEBUG = isDev;

export const LOG_LEVEL = isDev ? 'debug' : 'error';
```

### 2. Export with Aliasing
```javascript
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;

// Export with new names
export { 
  add as sum, 
  subtract as difference, 
  multiply as product 
};

// Import using new names
import { sum, difference, product } from './math.js';
```

### 3. Re-exporting with Renaming
```javascript
// legacy.js
export function oldFunction() { /* ... */ }
export const OLD_CONSTANT = 'value';

// modern.js
export { oldFunction as newFunction, OLD_CONSTANT as NEW_CONSTANT } from './legacy.js';

// app.js
import { newFunction, NEW_CONSTANT } from './modern.js';
```

### 4. Exporting Everything from Another Module
```javascript
// utils.js
export function helper1() {}
export function helper2() {}
export const CONSTANT = 'value';

// index.js
export * from './utils.js';
export * from './validators.js';
export * from './helpers.js';

// This creates a single entry point for all utilities
```

## Named Exports vs Default Exports

| Feature | Named Exports | Default Export |
|---------|---------------|----------------|
| Number per file | Multiple | One |
| Import syntax | `import { name }` | `import name` |
| Renaming | `import { name as alias }` | `import anyName` |
| Tree shaking | Better support | Good support |
| Use case | Utilities, constants | Main class/function |

## Common Mistakes

### 1. Incorrect Import Name
```javascript
// math.js
export function add(a, b) { return a + b; }

// ❌ Wrong - name doesn't exist
import { sum } from './math.js';

// ✅ Correct - matching name
import { add } from './math.js';
```

### 2. Forgetting Curly Braces
```javascript
// utils.js
export function formatDate() {}

// ❌ Wrong - treating named as default
import formatDate from './utils.js';

// ✅ Correct - using curly braces
import { formatDate } from './utils.js';
```

### 3. Duplicate Export Names
```javascript
// ❌ Wrong - duplicate names cause error
export const value = 1;
export const value = 2;

// ✅ Correct - unique names
export const value1 = 1;
export const value2 = 2;
```

### 4. Circular Dependencies with Named Exports
```javascript
// ❌ Avoid
// a.js
import { b } from './b.js';
export const a = 'a';

// b.js
import { a } from './a.js';
export const b = 'b';
```

## Quick Revision

- **Named exports** export multiple values from one file
- Use `export { name1, name2 }` or inline `export const name = value`
- Import with exact names: `import { name1, name2 } from './module.js'`
- Rename imports: `import { name as alias } from './module.js'`
- Import all: `import * as module from './module.js'`
- Re-export pattern: `export { name } from './other.js'`
- Better for tree-shaking than default exports
- Ideal for utility functions, constants, and related items

## Related Topics

- [[What-is-ImportExport]] - Overview of import/export system
- [[Default-Export]] - Default export patterns
- [[What-is-Module]] - Module system overview
- [[What-is-CommonJS]] - CommonJS module system
- [[What-is-DynamicImport]] - Dynamic import() function
- [[What-is-UMD]] - Universal Module Definition
- [[What-is-Scope]] - Module scope vs global scope
- [[Share-State]] - Sharing state between modules
- [[Lazy-Load]] - Lazy loading modules
