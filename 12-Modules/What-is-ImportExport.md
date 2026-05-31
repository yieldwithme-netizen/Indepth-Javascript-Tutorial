# What is Import/Export

## Definition

**Import/Export** is a module system in JavaScript that allows you to share code, functions, objects, and variables between different files. It provides a standardized way to modularize code, enabling better organization, reusability, and maintainability.

ES6 modules use `import` and `export` keywords to facilitate this communication between files.

## Syntax Overview

### Exporting from a module

```javascript
// Named export
export const PI = 3.14159;

// Export function
export function add(a, b) {
  return a + b;
}

// Export class
export class User {
  constructor(name) {
    this.name = name;
  }
}

// Default export
export default function greet(name) {
  return `Hello, ${name}`;
}
```

### Importing into a module

```javascript
// Named imports
import { PI, add, User } from './mathUtils.js';

// Default import
import greet from './greetings.js';

// Renaming imports
import { PI as piValue } from './mathUtils.js';

// Import all as namespace
import * as mathUtils from './mathUtils.js';
```

## Common Use Cases

### 1. Utility Functions
```javascript
// utils.js
export function formatCurrency(amount) {
  return `$${amount.toFixed(2)}`;
}

export function formatDate(date) {
  return date.toLocaleDateString();
}

export function slugify(text) {
  return text.toLowerCase().replace(/\s+/g, '-');
}

// main.js
import { formatCurrency, formatDate, slugify } from './utils.js';

console.log(formatCurrency(29.99)); // $29.99
```

### 2. Component-Based Architecture
```javascript
// Button.js
export function Button({ text, onClick }) {
  const button = document.createElement('button');
  button.textContent = text;
  button.addEventListener('click', onClick);
  return button;
}

// Card.js
export function Card({ title, children }) {
  const card = document.createElement('div');
  card.innerHTML = `<h2>${title}</h2>${children}`;
  return card;
}

// App.js
import { Button } from './Button.js';
import { Card } from './Card.js';
```

### 3. Configuration Files
```javascript
// config.js
export const API_URL = 'https://api.example.com';
export const TIMEOUT = 5000;
export default {
  debug: false,
  version: '1.0.0'
};

// main.js
import config, { API_URL, TIMEOUT } from './config.js';
```

### 4. Constants and Enums
```javascript
// constants.js
export const STATUS_CODES = {
  OK: 200,
  NOT_FOUND: 404,
  ERROR: 500
};

export const ROLES = {
  ADMIN: 'admin',
  USER: 'user',
  GUEST: 'guest'
};

// app.js
import { STATUS_CODES, ROLES } from './constants.js';
```

## Types of Exports

### Named Exports
- Can export multiple values per file
- Must be imported with exact names
- Can be renamed during import

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b;
```

### Default Exports
- Can only have one per file
- Can be imported with any name
- Useful for main module exports

```javascript
// logger.js
export default function log(message) {
  console.log(message);
}

// Can be imported as any name
import myLogger from './logger.js';
import log from './logger.js';
```

## Common Mistakes

### 1. Forgetting to Export
```javascript
// ❌ Incorrect - function not exported
function helper() {
  return 'help';
}

// ✅ Correct - function exported
export function helper() {
  return 'help';
}
```

### 2. Mismatched Import Names
```javascript
// math.js
export const add = (a, b) => a + b;

// ❌ Incorrect - wrong import name
import { sum } from './math.js';

// ✅ Correct - matching import name
import { add } from './math.js';
```

### 3. Trying to Import Default Export with Braces
```javascript
// utils.js
export default function formatDate() {}

// ❌ Incorrect
import { formatDate } from './utils.js';

// ✅ Correct
import formatDate from './utils.js';
```

### 4. Circular Dependencies
```javascript
// ❌ Avoid circular imports between modules
// a.js
import { b } from './b.js';
export const a = 'a';

// b.js
import { a } from './a.js';
export const b = 'b';
```

## Quick Revision

- **`export`** - Makes variables/functions available to other modules
- **`import`** - Brings in functionality from other modules
- **Named exports** - Export multiple values, import with exact names
- **Default exports** - One per file, import with any name
- **`import * as name`** - Import all exports as a namespace object
- **`import { name as alias }`** - Import with renaming
- Modules are always in strict mode
- Modules are deferred (executed after HTML parsing)
- Each module has its own scope

## Related Topics

- [[Named-Exports]] - Detailed guide on named exports
- [[Default-Export]] - Default export patterns
- [[What-is-Module]] - Module system overview
- [[What-is-CommonJS]] - CommonJS module system
- [[What-is-DynamicImport]] - Dynamic import() function
- [[What-is-UMD]] - Universal Module Definition
- [[What-is-Scope]] - Module scope vs global scope
- [[Share-State]] - Sharing state between modules
- [[Lazy-Load]] - Lazy loading modules
