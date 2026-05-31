# Let and Const

## Definition

`let` and `const` declare **block-scoped** variables.

## let

```javascript
let count = 0;
count = count + 1; // ✅ OK
```

## const

```javascript
const PI = 3.14;
PI = 3.14; // ❌ TypeError
```

## Quick Revision

- `let`: reassignable, block scoped
- `const`: not reassignable, block scoped
- Default to `const`

---

## Related Topics

- [[What-is-LetConst]] - [[What-is-LetConst|let/const]]
- [[Let-Const]] - [[Let-Const|let and const]]
- [[Declare-Let]] - [[Declare-Let|let]]
- [[Declare-Const]] - [[Declare-Const|const]]
