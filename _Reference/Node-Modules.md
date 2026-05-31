# Node Modules

## Definition

Node modules are **reusable code packages** in Node.js.

## CommonJS

```javascript
// Export
module.exports = { name: "John" };
exports.greet = () => "Hello";

// Import
const { name } = require('./module');
```

## ES Modules

```javascript
// Export
export const name = "John";
export default function greet() {}

// Import
import { name } from './module.js';
import greet from './module.js';
```

## Quick Revision

- CommonJS: `require()`/`module.exports`
- ES Modules: `import`/`export`
- Each module has own scope
- Use ES Modules in modern code

---

## Related Topics

- [[What-is-NodeModules]] - [[What-is-NodeModules|Node modules]]
- [[Node-Modules]] - [[Node-Modules|Node modules]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[What-is-CommonJS]] - [[What-is-CommonJS|CommonJS]]
