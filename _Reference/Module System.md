# Module System

## Definition

The JavaScript Module System provides a way to organize code into separate, reusable files. Modules allow you to export values (functions, objects, classes) from one file and import them in another. This encapsulates code, prevents global scope pollution, and enables better code organization.

## Code Examples

### ES6 Modules (Import/Export)

```javascript
// math.js
export const PI = 3.14159;

export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export default class Calculator {
  constructor() {
    this.result = 0;
  }

  add(n) {
    this.result += n;
    return this;
  }

  subtract(n) {
    this.result -= n;
    return this;
  }

  getResult() {
    return this.result;
  }
}
```

```javascript
// app.js
import Calculator, { PI, add, multiply } from "./math.js";

const calc = new Calculator();
const result = calc.add(5).subtract(2).getResult();

console.log(result);       // 3
console.log(PI);           // 3.14159
console.log(add(2, 3));    // 5
console.log(multiply(4, 5)); // 20
```

### Named vs Default Exports

```javascript
// utils.js

// Named exports - multiple per file
export const username = "Alice";
export function formatDate(date) {
  return date.toISOString().split("T")[0];
}

// Default export - one per file
export default function greet(name) {
  return `Hello, ${name}!`;
}
```

```javascript
// Using named exports
import { username, formatDate } from "./utils.js";

// Using default export (can rename)
import greet from "./utils.js";
import myGreet from "./utils.js";

// Import all as namespace
import * as Utils from "./utils.js";
console.log(Utils.formatDate(new Date()));
```

### Re-exporting

```javascript
// index.js
export { add, multiply } from "./math.js";
export { default as Calculator } from "./math.js";
export { PI } from "./math.js";

// Export all
export * from "./math.js";
```

### CommonJS Modules (Node.js)

```javascript
// utils.cjs
const PI = 3.14159;

function add(a, b) {
  return a + b;
}

module.exports = { PI, add };
```

```javascript
// app.cjs
const { PI, add } = require("./utils.cjs");
// or
const utils = require("./utils.cjs");

console.log(utils.add(2, 3));
```

### Dynamic Imports

```javascript
// Lazy loading modules
async function loadModule() {
  const { heavyFunction } = await import("./heavy-module.js");
  return heavyFunction();
}

// Conditional import
async function getFormatter(locale) {
  if (locale === "en") {
    return import("./formats/en.js");
  } else if (locale === "fr") {
    return import("./formats/fr.js");
  }
}
```

### Module in HTML

```javascript
// Use type="module" in HTML script tag
// <script type="module" src="app.js"></script>

// Modules are automatically in strict mode
// Each module has its own scope
// Top-level 'this' is undefined in modules
```

## Common Use Cases

- **Code organization** — Split large applications into manageable files
- **Code reuse** — Share utilities across multiple files
- **Encapsulation** — Keep internal state private
- **Dependency management** — Explicitly declare dependencies
- **Lazy loading** — Load modules on demand
- **Tree shaking** — Bundlers remove unused exports

## Common Mistakes

```javascript
// Mistake 1: Using import in CommonJS
// require("const x = require('./module.js'); // Wrong in ESM

// Mistake 2: Mixing module systems
// Don't mix import/export with require/module.exports

// Mistake 3: Missing file extension in ES modules
// import { foo } from './module'; // Wrong
import { foo } from "./module.js"; // Correct

// Mistake 4: Circular imports
// module-a.js imports from module-b.js
// module-b.js imports from module-a.js
// This can cause undefined values

// Mistake 5: Not using type="module" in HTML
// <script src="app.js"></script> // import/export won't work
// <script type="module" src="app.js"></script> // Correct
```

## Related Topics

- [[CommonJS]]
- [[AMD]]
- [[Node-JS]]
- [[ES6-Features]]
- [[Build-Tools]]
- [[NPM]]
- [[Scope]]

## Quick Revision

| Module Type | Syntax | Environment |
|------------|--------|-------------|
| ES6 | `import`/`export` | Browser, Node 14+ |
| CommonJS | `require`/`module.exports` | Node.js |
| AMD | `define`/`require` | Browser (legacy) |
| UMD | Universal wrapper | Browser + Node |

| Feature | Description |
|---------|-------------|
| Named export | `export const x = 1` |
| Default export | `export default function` |
| Namespace | `import * as ns from './file.js'` |
| Dynamic | `const m = await import('./file.js')` |
| Re-export | `export { x } from './file.js'` |
