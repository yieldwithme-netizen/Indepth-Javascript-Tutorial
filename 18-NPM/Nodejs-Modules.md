# Node.js Modules

## Definition

Node.js modules are **reusable code packages**.

## CommonJS

```javascript
// Export
module.exports = { name: "John" };

// Import
const { name } = require('./module');
```

## ES Modules

```javascript
// Export
export const name = "John";

// Import
import { name } from './module.js';
```

## Quick Revision

- CommonJS: require/module.exports
- ES Modules: import/export
- Each module has own scope

---

## Related Topics

- [[What-is-NodeModules]] - [[What-is-NodeModules|Node modules]]
- [[Node-Modules]] - [[Node-Modules|Node modules]]
- [[Nodejs-Modules]] - [[Nodejs-Modules|Node.js modules]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
