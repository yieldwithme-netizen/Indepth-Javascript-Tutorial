# Let Keyword

## Definition

`let` declares a **block-scoped** variable that can be reassigned.

## Basic Usage

```javascript
let count = 0;
count = count + 1; // ✅ OK
```

## Block Scope

```javascript
if (true) {
    let x = 10;
}
console.log(x); // ReferenceError
```

## Quick Revision

- `let` = block scoped
- Can be reassigned
- Cannot redeclare in same scope
- Use for: variables that change

---

## Related Topics

- [[What-is-LetConst]] - [[What-is-LetConst|let/const]]
- [[Let-Keyword]] - [[Let-Keyword|let]]
- [[Declare-Let]] - [[Declare-Let|let]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
