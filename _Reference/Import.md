# Import

## Definition

`import` brings **functionality from other modules** into your code.

## Named Import

```javascript
import { name, age } from './module.js';
```

## Default Import

```javascript
import MyComponent from './Component.js';
```

## Namespace Import

```javascript
import * as Utils from './utils.js';
Utils.add(1, 2);
```

## Dynamic Import

```javascript
const module = await import('./module.js');
```

## Quick Revision

- `import` brings module functionality
- Named: `import { name } from`
- Default: `import Name from`
- Namespace: `import * as Name from`
- Dynamic: `import()` (async)

---

## Related Topics

- [[What-is-ImportExport]] - [[What-is-ImportExport|Import/export]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[Named-Exports]] - [[Named-Exports|Named exports]]
- [[Default-Export]] - [[Default-Export|Default export]]
