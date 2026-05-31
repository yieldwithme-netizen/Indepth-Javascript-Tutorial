# Symbol.toPrimitive

## Definition
`Symbol.toPrimitive` is a well-known Symbol that specifies a method for converting an object to a primitive value. It allows you to customize how your objects behave when used with operators or type conversion.

## Syntax
```javascript
obj[Symbol.toPrimitive](hint)
```

The `hint` parameter can be:
- `"string"` - When a string is expected
- `"number"` - When a number is expected
- `"default"` - When no preference is specified

## Basic Usage

```javascript
const temperature = {
  celsius: 25,
  [Symbol.toPrimitive](hint) {
    if (hint === "number") {
      return this.celsius;
    }
    if (hint === "string") {
      return `${this.celsius}°C`;
    }
    // default
    return this.celsius;
  }
};

console.log(+temperature);         // 25 (number)
console.log(`${temperature}`);     // "25°C" (string)
console.log(temperature + 5);      // 30 (default)
console.log("Temperature: " + temperature);  // "Temperature: 25°C"
```

## Practical Example: Money Class

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
        return `${this.currency} ${this.amount.toFixed(2)}`;
      default:
        return this.amount;
    }
  }

  valueOf() {
    return this.amount;
  }

  toString() {
    return `${this.currency} ${this.amount.toFixed(2)}`;
  }
}

const price = new Money(99.99, "USD");

console.log(price + 10);          // 109.99 (number)
console.log(`Price: ${price}`);   // "Price: USD 99.99"
console.log(-price);              // -99.99 (number)
```

## Conversion Order

When converting an object to a primitive, JavaScript follows this order:

1. `Symbol.toPrimitive("default")` if defined
2. `valueOf()` if defined and returns a primitive
3. `toString()` if defined and returns a primitive
4. Throws TypeError if no primitive can be returned

```javascript
const obj = {
  [Symbol.toPrimitive](hint) {
    console.log(`toPrimitive called with hint: ${hint}`);
    return 42;
  },
  valueOf() {
    console.log("valueOf called");
    return 100;
  },
  toString() {
    console.log("toString called");
    return "string";
  }
};

// Symbol.toPrimitive takes precedence
console.log(obj + 1);
// Output:
// toPrimitive called with hint: default
// 43
```

## Hint Values Explained

```javascript
const obj = {
  [Symbol.toPrimitive](hint) {
    switch (hint) {
      case "string":
        return "string conversion";
      case "number":
        return 42;
      case "default":
        return "default conversion";
    }
  }
};

// "string" hint - template literals, String()
console.log(`${obj}`);      // "string conversion"
console.log(String(obj));   // "string conversion"

// "number" hint - arithmetic, Number()
console.log(+obj);          // 42
console.log(obj * 2);       // 84
console.log(Number(obj));   // 42

// "default" hint - comparison, + with string
console.log(obj == 1);      // uses default
console.log(obj + "text");  // uses default
```

## Common Use Cases
- Custom type coercion for classes
- Mathematical operators with objects
- String concatenation with objects
- Comparison operators
- Implicit type conversion

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Not handling all hints | Provide logic for all hint types |
| Returning non-primitive | Always return primitive values |
| Confusing with valueOf | Symbol.toPrimitive has higher priority |
| Not considering default | Handle "default" hint case |

## Quick Revision Summary
- `Symbol.toPrimitive` customizes object-to-primitive conversion
- Three hints: "string", "number", "default"
- Takes precedence over valueOf and toString
- Must return a primitive value
- Useful for mathematical and string operations with objects

## Related Topics
- [[Type-Coercion]]
- [[Type-Conversion]]
- [[Operators]]
- [[Symbols]]
- [[valueOf]]
- [[toString]]
- [[Implicit-Conversion]]
