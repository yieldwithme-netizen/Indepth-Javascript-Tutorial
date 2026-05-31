# How to Use Symbol

## Definition
A `Symbol` is a primitive value that is guaranteed to be unique. Symbols are often used as object property keys to avoid name collisions and create private-like properties.

## Creating Symbols

```javascript
// Unique symbol
const sym1 = Symbol();
const sym2 = Symbol();
console.log(sym1 === sym2);  // false (always unique)

// Symbol with description (for debugging)
const sym3 = Symbol("description");
console.log(sym3.toString());  // "Symbol(description)"

// Global symbol registry
const globalSym1 = Symbol.for("app.name");
const globalSym2 = Symbol.for("app.name");
console.log(globalSym1 === globalSym2);  // true (same symbol)
```

## Using Symbols as Object Keys

```javascript
const id = Symbol("id");
const user = {
  name: "John",
  age: 30,
  [id]: 12345  // Symbol property
};

console.log(user[id]);  // 12345
console.log(user.id);   // undefined (regular property)
```

## Well-Known Symbols

```javascript
// Symbol.iterator
class Range {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;

    return {
      next() {
        if (current <= end) {
          return { value: current++, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

const range = new Range(1, 3);
for (const num of range) {
  console.log(num);  // 1, 2, 3
}

// Symbol.toPrimitive
const price = {
  amount: 100,
  currency: "USD",

  [Symbol.toPrimitive](hint) {
    if (hint === "number") return this.amount;
    if (hint === "string") return `${this.amount} ${this.currency}`;
    return this.amount;
  }
};

console.log(+price);         // 100
console.log(`${price}`);     // "100 USD"
console.log(price + 50);     // 150
```

## Getting Symbol Properties

```javascript
const sym = Symbol("secret");
const obj = {
  [sym]: "hidden",
  name: "visible"
};

// Get own symbol properties
const symbols = Object.getOwnPropertySymbols(obj);
console.log(symbols);  // [Symbol(secret)]

// Access via symbol
console.log(obj[symbols[0]]);  // "hidden"
```

## Common Use Cases

### Private Properties

```javascript
const _private = Symbol("private");

class User {
  constructor(name, password) {
    this.name = name;
    this[_private] = { password };
  }

  checkPassword(pwd) {
    return this[_private].password === pwd;
  }
}

const user = new User("John", "secret");
console.log(user.name);       // "John"
console.log(user._private);   // undefined (can't access)
```

### Avoid Property Name Collisions

```javascript
// Library A
const libraryA = {
  [Symbol("toString")]: function() { return "A"; }
};

// Library B (different symbol, no conflict)
const libraryB = {
  [Symbol("toString")]: function() { return "B"; }
};

// Both work without conflict
```

### Custom Type Conversions

```javascript
class Money {
  constructor(amount, currency) {
    this.amount = amount;
    this.currency = currency;
  }

  [Symbol.toPrimitive](hint) {
    switch (hint) {
      case "number":
        return this.amount;
      case "string":
        return `${this.currency} ${this.amount}`;
      default:
        return this.amount;
    }
  }

  [Symbol.toStringTag]() {
    return `Money(${this.currency})`;
  }
}

const price = new Money(100, "USD");
console.log(price + 50);      // 150
console.log(`Price: ${price}`); // "Price: USD 100"
```

## Common Mistakes

```javascript
// Wrong: Comparing symbols with ===
const a = Symbol("test");
const b = Symbol("test");
console.log(a === b);  // false (different symbols)

// Wrong: Using symbols as property names with dot notation
const sym = Symbol("id");
const obj = { [sym]: 123 };
console.log(obj.sym);  // undefined

// Correct: Use bracket notation
console.log(obj[sym]);  // 123

// Wrong: Forgetting Symbol.for creates global symbols
const x = Symbol.for("app");
const y = Symbol.for("app");
console.log(x === y);  // true (same global symbol)
```

## Quick Revision

- `Symbol()` creates unique symbols
- `Symbol.for()` creates global symbols in registry
- Symbols can be object property keys (via `[sym]`)
- Well-known symbols: `Symbol.iterator`, `Symbol.toPrimitive`, `Symbol.toStringTag`
- Great for private properties and avoiding collisions
- `Object.getOwnPropertySymbols()` gets symbol keys

## Related Topics

- [[What-is-Symbol]] - Symbol overview
- [[Well-Known-Symbols]] - Built-in symbols
- [[Symbol-Iterator]] - Iterator protocol
- [[Symbol-ToPrimitive]] - Type conversion
