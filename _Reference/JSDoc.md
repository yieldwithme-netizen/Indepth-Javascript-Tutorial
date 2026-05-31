# JSDoc

## Definition

**JSDoc** is an API documentation syntax for JavaScript. It uses special comment annotations (called JSDoc comments) to generate documentation for JavaScript code. JSDoc comments are written as multi-line comments starting with `/**` and can include type annotations, descriptions, parameter documentation, return values, and more.

JSDoc is widely used in IDEs for autocompletion, type checking, and generating API documentation.

---

## Syntax

### Basic JSDoc Comment
```javascript
/**
 * Description of the function
 */
```

### Parameter Documentation
```javascript
/**
 * Adds two numbers together.
 * @param {number} a - The first number
 * @param {number} b - The second number
 * @returns {number} The sum of a and b
 */
function add(a, b) {
  return a + b;
}
```

### Common Tags
```javascript
/**
 * @param {Type} name - Description
 * @returns {Type} Description
 * @throws {ErrorType} Description
 * @example
 * @see
 * @deprecated
 * @since
 * @author
 * @version
 */
```

---

## Code Examples

### Function Documentation
```javascript
/**
 * Calculates the area of a rectangle.
 * @param {number} width - The width of the rectangle
 * @param {number} height - The height of the rectangle
 * @returns {number} The area of the rectangle
 */
function calculateArea(width, height) {
  return width * height;
}
```

### Class Documentation
```javascript
/**
 * Represents a person.
 * @class
 */
class Person {
  /**
   * Create a person.
   * @param {string} name - The person's name
   * @param {number} age - The person's age
   */
  constructor(name, age) {
    /** @type {string} */
    this.name = name;
    /** @type {number} */
    this.age = age;
  }

  /**
   * Get the person's greeting.
   * @returns {string} A greeting message
   */
  greet() {
    return `Hello, I'm ${this.name}`;
  }
}
```

### Complex Types
```javascript
/**
 * Processes user data.
 * @param {Object} user - The user object
 * @param {string} user.name - The user's name
 * @param {string} user.email - The user's email
 * @param {number} [user.age] - The user's age (optional)
 * @param {boolean} [user.active=true] - Whether user is active
 * @returns {Object} Processed user data
 */
function processUser(user) {
  return {
    ...user,
    active: user.active ?? true
  };
}
```

### Array and Object Types
```javascript
/**
 * Filters an array of numbers.
 * @param {number[]} numbers - Array of numbers
 * @param {function(number): boolean} predicate - Filter function
 * @returns {number[]} Filtered array
 */
function filterNumbers(numbers, predicate) {
  return numbers.filter(predicate);
}

/**
 * @typedef {Object} User
 * @property {string} name - User name
 * @property {string} email - User email
 * @property {number} age - User age
 */

/**
 * @typedef {Object} ApiResponse
 * @property {boolean} success - Whether request succeeded
 * @property {User[]} data - Array of users
 * @property {string} [error] - Error message if failed
 */

/**
 * Fetches users from API.
 * @returns {Promise<ApiResponse>} The API response
 */
async function fetchUsers() {
  const response = await fetch('/api/users');
  return response.json();
}
```

### Class with Inheritance
```javascript
/**
 * Base class for shapes.
 * @abstract
 */
class Shape {
  /**
   * Calculate the area of the shape.
   * @abstract
   * @returns {number} The area
   */
  area() {
    throw new Error('Method must be implemented');
  }
}

/**
 * Represents a circle.
 * @extends Shape
 */
class Circle extends Shape {
  /**
   * Create a circle.
   * @param {number} radius - The radius of the circle
   */
  constructor(radius) {
    super();
    /** @type {number} */
    this.radius = radius;
  }

  /**
   * Calculate the area of the circle.
   * @returns {number} The area of the circle
   */
  area() {
    return Math.PI * this.radius ** 2;
  }
}
```

### Optional and Default Parameters
```javascript
/**
 * Creates a formatted string.
 * @param {string} name - The name
 * @param {Object} [options] - Formatting options
 * @param {boolean} [options.uppercase=false] - Convert to uppercase
 * @param {string} [options.separator=' '] - Separator between words
 * @returns {string} The formatted string
 */
function formatName(name, options = {}) {
  const { uppercase = false, separator = ' ' } = options;
  return uppercase ? name.toUpperCase() : name;
}
```

### Event Handler Documentation
```javascript
/**
 * Handles click events on the button.
 * @param {MouseEvent} event - The click event
 * @returns {void}
 */
function handleClick(event) {
  console.log('Clicked at:', event.clientX, event.clientY);
}

/**
 * Handles form submission.
 * @param {Event} event - The submit event
 * @throws {Error} If validation fails
 * @returns {void}
 */
function handleSubmit(event) {
  event.preventDefault();
  // validation logic
}
```

### Module Documentation
```javascript
/**
 * @module utils
 * @description Utility functions for data processing
 */

/**
 * Debounce a function call.
 * @param {Function} func - Function to debounce
 * @param {number} wait - Milliseconds to wait
 * @returns {Function} Debounced function
 * @example
 * const debouncedSearch = debounce(search, 300);
 */
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **IDE Support** | Autocomplete, type hints, and hover documentation |
| **API Documentation** | Generate documentation from source code |
| **Type Checking** | Use with TypeScript or Closure Compiler |
| **Team Collaboration** | Document code for other developers |
| **Code Maintenance** | Understand code purpose and usage |

---

## Common Mistakes

### 1. Missing Type Annotations
```javascript
// BAD
/**
 * Adds two numbers
 */
function add(a, b) { return a + b; }

// GOOD
/**
 * Adds two numbers.
 * @param {number} a - First number
 * @param {number} b - Second number
 * @returns {number} Sum of a and b
 */
function add(a, b) { return a + b; }
```

### 2. Inconsistent Documentation Style
```javascript
// BAD - Mixed styles
/** Adds numbers */
/** Subtracts numbers */
/** Multiplies two numbers together */

// GOOD - Consistent style
/**
 * Adds two numbers.
 */
/**
 * Subtracts two numbers.
 */
/**
 * Multiplies two numbers.
 */
```

### 3. Outdated Documentation
```javascript
/**
 * Fetches data from API.
 * @returns {Object} The data
 * (But function was updated to return Promise)
 */
function fetchData() {
  return fetch('/api/data'); // Returns Promise now!
}
```

---

## Quick Revision Summary

- JSDoc is a documentation syntax for JavaScript using special comments
- Comments start with `/**` and end with `*/`
- Common tags: `@param`, `@returns`, `@throws`, `@example`, `@see`
- JSDoc enables IDE autocompletion, type checking, and documentation generation
- Document parameters, return values, and side effects
- Keep documentation up-to-date with code changes
- Use `@typedef` for complex object types

---

## Related Topics

- [[JavaScript]] - JavaScript language overview
- [[function]] - Function documentation patterns
- [[Function-Scope-and-Closures]] - Documenting closure behavior
- [[Local-Storage]] - Documenting storage utilities
- [[let]] - Documenting variable scopes
