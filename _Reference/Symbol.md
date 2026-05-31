# Symbol

## Definition

Symbol is a **unique, immutable identifier** for object properties.

## Creating Symbols

```javascript
const sym1 = Symbol();
const sym2 = Symbol("description"); // optional description

// Symbols are unique
console.log(sym1 === sym2); // false
```

## Using as Keys

```javascript
const sym = Symbol("id");
const obj = {
    name: "John",
    [sym]: 123
};

console.log(obj[sym]); // 123
```

## Global Symbols

```javascript
const sym1 = Symbol.for("id");
const sym2 = Symbol.for("id");
console.log(sym1 === sym2); // true
```

## Quick Revision

- Symbol = unique identifier
- Created with `Symbol()`
- Used as object keys
- `Symbol.for()` for global symbols

---

## Related Topics

- [[What-is-Symbol]] - [[What-is-Symbol|Symbol]]
- [[Symbol]] - [[Symbol|Symbol]]
- [[Use-Symbol]] - [[Use-Symbol|Using Symbol]]
- [[What-is-Primitive]] - [[What-is-Primitive|Primitives]]
