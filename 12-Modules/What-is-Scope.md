# What-is-Scope (Modules)

## Definition

Module scope means variables are **private to the module**.

## Example

```javascript
// module.js
const private = "I'm private";
export const public = "I'm public";

// app.js
import { public } from './module.js';
console.log(public); // "I'm public"
console.log(private); // ReferenceError
```

## Quick Revision

- Each module has own scope
- Variables not globally accessible
- Use `export` to share
- Use `import` to use

---

## Related Topics

- [[What-is-Scope]] - [[What-is-Scope|Scope]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[What-is-ImportExport]] - [[What-is-ImportExport|Import/export]]
