# JavaScript Modules

## Definition

Modules are reusable pieces of code that encapsulate functionality and can be imported/exported between files. They help organize code, avoid global namespace pollution, and enable code reuse.

## Module Systems Overview

### 1. CommonJS (CJS)

```javascript
// Exporting
module.exports = { myFunction };

// Importing
const { myFunction } = require("./myModule");
```

### 2. ES Modules (ESM) - Modern Standard

```javascript
// Exporting
export const myFunction = () => {};
export default class MyClass {}

// Importing
import { myFunction } from "./myModule.js";
import MyClass from "./myModule.js";
import * as Utils from "./utils.js";
```

## ES Modules (ESM) - In Depth

### Named Exports

```javascript
// math.js
export const PI = 3.14159;
export const E = 2.71828;

export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}
```

### Default Exports

```javascript
// Logger.js
export default class Logger {
  constructor(name) {
    this.name = name;
  }

  log(message) {
    console.log(`[${this.name}]: ${message}`);
  }
}

// Only one default export per module
// app.js
import Logger from "./Logger.js";
const logger = new Logger("App");
logger.log("Hello");
```

### Importing Patterns

```javascript
// Named imports
import { add, PI } from "./math.js";

// Renaming imports
import { add as sum, PI as pi } from "./math.js";

// Default import
import Logger from "./Logger.js";

// Namespace import
import * as MathUtils from "./math.js";

console.log(MathUtils.add(1, 2));
console.log(MathUtils.PI);

// Side-effect only import
import "./setup.js";
```

### Dynamic Imports

```javascript
// Lazy loading modules
async function loadModule() {
  const { default: Chart } = await import("./chart.js");
  const chart = new Chart();
  // ...
}

// Conditional imports
if (process.env.NODE_ENV === "development") {
  import("./dev-tools.js").then(module => {
    module.enableDebugging();
  });
}
```

## Code Examples

### Organizing a Project

```
project/
├── src/
│   ├── utils/
│   │   ├── math.js
│   │   └── helpers.js
│   ├── components/
│   │   ├── Button.js
│   │   └── Modal.js
│   └── app.js
└── package.json
```

```javascript
// utils/math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export default { add, subtract };

// components/Button.js
export function createButton(text) {
  const button = document.createElement("button");
  button.textContent = text;
  return button;
}

// app.js
import { add } from "./utils/math.js";
import { createButton } from "./components/Button.js";
```

### Re-exporting

```javascript
// utils/index.js
export { add, subtract } from "./math.js";
export { capitalize, lowercase } from "./strings.js";

// app.js - cleaner imports
import { add, capitalize } from "./utils";
```

## Common Use Cases

- Organizing large codebases
- Creating reusable libraries
- Code splitting and lazy loading
- Avoiding global namespace pollution
- Testing individual components

## Common Mistakes

1. **Forgetting file extensions in browser**: ESM requires full path with extension

```javascript
// Wrong
import { foo } from "./bar";

// Correct
import { foo } from "./bar.js";
```

2. **Using CJS and ESM syntax together**: Can cause errors

3. **Circular imports**: Can lead to undefined values

4. **Missing `type: "module"` in package.json**:

```json
{
  "type": "module"
}
```

## Related Topics

- [[CommonJS]] - Node.js module system
- [[Dynamic-Import]] - Runtime module loading
- [[Tree-Shaking]] - Removing unused code
- [[Bundlers]] - Webpack, Rollup, Vite
- [[Package.json]] - Project configuration
- [[Module-Bundlers]] - Build tools for modules

## Quick Revision Summary

| System | Syntax | Environment |
|--------|--------|-------------|
| CommonJS | `require()` / `module.exports` | Node.js |
| ES Modules | `import` / `export` | Modern browsers & Node.js |
| AMD | `define()` / `require()` | Browser (legacy) |
| UMD | Universal wrapper | Both browser & Node.js |
