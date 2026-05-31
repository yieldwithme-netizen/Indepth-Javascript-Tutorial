# Constants

## Definition

Constants are **values that cannot be reassigned**.

## Basic Usage

```javascript
const PI = 3.14159;
const API_URL = "https://api.example.com";

// Cannot reassign
PI = 3.14; // TypeError!

// But can mutate objects/arrays
const arr = [1, 2, 3];
arr.push(4); // OK!

const obj = { name: "John" };
obj.name = "Jane"; // OK!
```

## Quick Revision

- Constants: no reassignment
- Must initialize when declared
- Objects/arrays can be mutated
- Use for: configuration, fixed values

---

## Related Topics

- [[Declare-Const]] - [[Declare-Const|const]]
- [[const-keyword]] - [[const-keyword|const keyword]]
- [[Constants]] - [[Constants|Constants]]
- [[What-is-LetConst]] - [[What-is-LetConst|let/const]]
