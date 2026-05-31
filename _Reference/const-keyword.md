# const Keyword

## Definition

`const` declares a **block-scoped** variable that cannot be reassigned.

## Basic Usage

```javascript
const PI = 3.14159;
PI = 3.14; // TypeError!
```

## Object/Array Mutation

```javascript
const arr = [1, 2, 3];
arr.push(4); // ✅ OK
arr = [];    // ❌ TypeError

const obj = { name: "John" };
obj.name = "Jane"; // ✅ OK
obj = {};          // ❌ TypeError
```

## Quick Revision

- `const` = constant (no reassignment)
- Must initialize when declaring
- Objects/arrays can be mutated
- Use for: constants, config

---

## Related Topics

- [[What-is-LetConst]] - [[What-is-LetConst|let/const]]
- [[const-keyword]] - [[const-keyword|const]]
- [[Declare-Const]] - [[Declare-Const|const]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
