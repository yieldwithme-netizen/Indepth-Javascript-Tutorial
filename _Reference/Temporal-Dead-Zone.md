# Temporal Dead Zone

## Definition

The temporal dead zone (TDZ) is where **variables exist but can't be accessed**.

## Example

```javascript
// TDZ starts here
console.log(x); // ReferenceError
// TDZ ends here
let x = 10;
```

## var vs let/const

```javascript
// var: no TDZ (hoisted, initialized to undefined)
console.log(x); // undefined
var x = 10;

// let/const: has TDZ (hoisted, NOT initialized)
console.log(y); // ReferenceError
let y = 10;
```

## Quick Revision

- TDZ = variables exist but inaccessible
- `let`/`const` have TDZ
- `var` does not have TDZ
- Happens before declaration
- Prevents using variables before init

---

## Related Topics

- [[What-is-Hoisting]] - [[What-is-Hoisting|Hoisting]]
- [[Temporal-Dead-Zone]] - [[Temporal-Dead-Zone|TDZ]]
- [[Declare-Let]] - [[Declare-Let|let]]
- [[Declare-Const]] - [[Declare-Const|const]]
