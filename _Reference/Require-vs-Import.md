# Require vs Import

## Definition

Require and import are **module systems** for JavaScript.

## CommonJS (require)

```javascript
// Export
module.exports = { name: "John" };
exports.greet = () => "Hello";

// Import
const { name } = require('./module');
```

## ES Modules (import)

```javascript
// Export
export const name = "John";
export default function greet() {}

// Import
import { name } from './module.js';
import greet from './module.js';
```

## Quick Revision

- CommonJS: `require()` (Node.js)
- ES Modules: `import` (modern)
- ES Modules are async
- Use ES Modules in modern code

---

## Related Topics

- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[Require-vs-Import]] - [[Require-vs-Import|Require vs import]]
- [[What-is-CommonJS]] - [[What-is-CommonJS|CommonJS]]
- [[What-is-ImportExport]] - [[What-is-ImportExport|Import/export]]
