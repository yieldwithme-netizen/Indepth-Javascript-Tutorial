# Export

## Definition

`export` makes **functionality available** from a module.

## Named Export

```javascript
export const name = "John";
export function greet() {}
export class User {}
```

## Default Export

```javascript
export default function greet() {}
```

## Re-export

```javascript
export { name, age } from './module.js';
export default from './module.js';
```

## Quick Revision

- Named export: `export const name`
- Default export: `export default`
- Re-export: `export { } from`
- Each module can have one default

---

## Related Topics

- [[What-is-ImportExport]] - [[What-is-ImportExport|Import/export]]
- [[Named-Exports]] - [[Named-Exports|Named exports]]
- [[Default-Export]] - [[Default-Export|Default export]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
