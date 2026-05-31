# What is a Module?

## Definition

A module is a **self-contained piece of code** that exports functionality for other files to use.

## ES6 Modules

```javascript
// math.js - exporting
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export default class Calculator {
    // ...
}

// app.js - importing
import Calculator, { PI, add } from './math.js';

console.log(PI); // 3.14159
console.log(add(5, 3)); // 8
```

## Module Types

| Type | Syntax | Environment |
|------|--------|-------------|
| ES6 | `import`/`export` | Browser, Node.js |
| CommonJS | `require()`/`module.exports` | Node.js |
| AMD | `define()`/`require()` | Browser (legacy) |

## Module Scope

```javascript
// Each module has its own scope
// Variables are not global

// module1.js
const secret = "hidden";
export const public = "visible";

// module2.js
import { public } from './module1.js';
console.log(public); // "visible"
console.log(secret); // ReferenceError!
```

## Quick Revision

- Module = self-contained code unit
- `export` to share, `import` to use
- Each module has own scope
- ES6: `import`/`export`
- CommonJS: `require()`/`module.exports`

---

## Related Topics

- [[What-is-Module]] - [[What-is-Module|Modules]] overview
- [[What-is-ImportExport]] - [[What-is-ImportExport|Import/export]]
- [[Named-Exports]] - [[Named-Exports|Named exports]]
- [[Default-Export]] - [[Default-Export|Default export]]
- [[What-is-Scope]] - [[What-is-Scope|Module scope]]
