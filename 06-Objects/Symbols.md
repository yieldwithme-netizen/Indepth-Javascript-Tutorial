# Symbols

## Definition

Symbols are **unique, immutable identifiers** for object properties.

## Basic Usage

```javascript
const sym1 = Symbol("id");
const sym2 = Symbol("id");
console.log(sym1 === sym2); // false

const obj = {
    [sym1]: "value"
};
console.log(obj[sym1]); // "value"
```

## Quick Revision

- Symbol = unique identifier
- Created with `Symbol()`
- Used as object keys
- Cannot be enumerated

---

## Related Topics

- [[What-is-Symbol]] - [[What-is-Symbol|Symbol]]
- [[Symbols]] - [[Symbols|Symbols]]
- [[Use-Symbol]] - [[Use-Symbol|Using Symbol]]
- [[Well-Known-Symbols]] - [[Well-Known-Symbols|Well-known symbols]]
