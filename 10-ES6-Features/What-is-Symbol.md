# What is Symbol?

## Definition

A Symbol is a **unique, immutable identifier** for object properties.

## Creating Symbols

```javascript
// Basic
const sym1 = Symbol();
const sym2 = Symbol("description"); // optional description

// Symbols are unique
const sym1 = Symbol("id");
const sym2 = Symbol("id");
console.log(sym1 === sym2); // false
```

## Using Symbols as Keys

```javascript
const sym = Symbol("id");

const obj = {
    name: "John",
    [sym]: 123
};

console.log(obj[sym]); // 123
console.log(Object.keys(obj)); // ["name"] (symbol not shown!)
```

## Symbol.for() - Global Symbols

```javascript
// Creates global symbol
const sym1 = Symbol.for("id");
const sym2 = Symbol.for("id");
console.log(sym1 === sym2); // true

// Get key from symbol
const key = Symbol.keyFor(sym1); // "id"
```

## Well-Known Symbols

```javascript
// Symbol.iterator
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator]();
console.log(iterator.next()); // { value: 1, done: false }

// Symbol.toPrimitive
const obj = {
    [Symbol.toPrimitive](hint) {
        return 42;
    }
};
console.log(+obj); // 42
```

## Quick Revision

- Symbol = unique identifier
- Created with `Symbol()` or `Symbol.for()`
- Used as object keys (hidden from for...in)
- Immutable and unique
- Use for: private properties, unique IDs

---

## Related Topics

- [[What-is-Symbol]] - Symbol overview
- [[Use-Symbol]] - Using Symbol
- [[What-is-Iterator]] - Iterator protocol
- [[What-is-Primitive]] - Primitive types
