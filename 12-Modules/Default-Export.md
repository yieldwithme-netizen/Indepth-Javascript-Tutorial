# How to Use Default Export in JavaScript Modules

## Definition

Default export is a way to export a single main value from a JavaScript module. Each module can have only one default export. The importing module can choose any name for the imported value, making it convenient for modules that export a primary function, class, or object.

## Syntax

```javascript
// Exporting
export default value;
// or
export default function functionName() {}
export default class ClassName {}

// Importing
import AnyName from './module.js';
import AnyName from './module.js';
```

## Code Examples

### Basic Default Export

```javascript
// utils.js
export default function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

// main.js
import capitalize from './utils.js';

console.log(capitalize('hello')); // "Hello"
```

### Default Export with Class

```javascript
// User.js
export default class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  greet() {
    return `Hello, ${this.name}!`;
  }

  toString() {
    return `${this.name} <${this.email}>`;
  }
}

// main.js
import User from './User.js';

const user = new User('John', 'john@example.com');
console.log(user.greet()); // "Hello, John!"
```

### Default Export with Object

```javascript
// config.js
export default {
  apiUrl: 'https://api.example.com',
  timeout: 5000,
  retries: 3,
  debug: false
};

// main.js
import config from './config.js';

console.log(config.apiUrl); // "https://api.example.com"
```

### Default Export with Function Expression

```javascript
// calculator.js
export default function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export function multiply(a, b) {
  return a * b;
}

// main.js
import add, { subtract, multiply } from './calculator.js';

console.log(add(5, 3)); // 8
console.log(subtract(5, 3)); // 2
console.log(multiply(5, 3)); // 15
```

### Default Export with Named Exports

```javascript
// mathUtils.js
export default class MathUtils {
  static add(a, b) {
    return a + b;
  }

  static subtract(a, b) {
    return a - b;
  }
}

export const PI = 3.14159;
export const E = 2.71828;

// main.js
import MathUtils, { PI, E } from './mathUtils.js';

console.log(MathUtils.add(2, 3)); // 5
console.log(PI); // 3.14159
console.log(E); // 2.71828
```

### Re-exporting Default Export

```javascript
// utils.js
export default function helper() {
  return 'helper';
}

// index.js
export { default } from './utils.js';
// or rename
export { default as utilsHelper } from './utils.js';

// main.js
import utilsHelper from './index.js';
```

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| Single Responsibility | Module exports one main function/class |
| Library Entry Point | Main export from a library |
| Configuration | Default config object |
| Service Classes | Single service class per file |
| React Components | One component per file |
| Utility Functions | One utility function per file |

## Common Mistakes

```javascript
// ❌ Wrong: Multiple default exports
export default function first() {}
export default function second() {} // SyntaxError: Duplicate export

// ✅ Correct: One default, rest named
export default function main() {}
export function helper1() {}
export function helper2() {}

// ❌ Wrong: Importing default without path
import capitalize from 'utils'; // Missing .js extension in some environments

// ✅ Correct: Include file extension
import capitalize from './utils.js';

// ❌ Wrong: Using curly braces for default import
import { capitalize } from './utils.js'; // Imports nothing

// ✅ Correct: No curly braces for default
import capitalize from './utils.js';

// ❌ Wrong: Wrong order with named imports
import { subtract }, add from './calculator.js'; // SyntaxError

// ✅ Correct: Default first, then named
import add, { subtract, multiply } from './calculator.js';
```

## Related Topics

- [[What-is-DynamicImport]] - Dynamic import() function
- [[Lazy-Load]] - Lazy loading modules
- [[What-is-Scope]] - Module scope concepts
- [[Create-Class]] - Creating classes for export
- [[Implement-Encapsulation]] - Encapsulation principles

## Quick Revision

| Concept | Description |
|---------|-------------|
| Default Export | `export default value` |
| Default Import | `import AnyName from './module.js'` |
| One Per Module | Only one default export allowed |
| Any Name | Importer chooses the name |
| Mixing | Can combine with named exports |
| Re-export | `export { default } from './module.js'` |
| No Curly Braces | Default imports don't use `{}` |
