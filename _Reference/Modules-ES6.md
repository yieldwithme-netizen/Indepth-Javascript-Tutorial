# ES6 Modules

## Definition

ES6 modules are the **standard module system** for JavaScript.

## Exporting

```javascript
// Named export
export const name = "John";
export function greet() {}

// Default export
export default class User {}
```

## Importing

```javascript
// Named import
import { name, greet } from './module.js';

// Default import
import User from './module.js';

// Namespace import
import * as Utils from './utils.js';
```

## Quick Revision

- `export` to share
- `import` to use
- Named vs default exports
- Modules have own scope

---

## Related Topics

- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[Modules-ES6]] - [[Modules-ES6|ES6 modules]]
- [[What-is-ImportExport]] - [[What-is-ImportExport|Import/export]]
- [[Named-Exports]] - [[Named-Exports|Named exports]]
