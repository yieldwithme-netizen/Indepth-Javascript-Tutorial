# What is NaN?

## Definition

`NaN` stands for **"Not a Number"**. It's the result of an invalid or undefined mathematical operation.

## When NaN Occurs

```javascript
// Invalid math operations
0 / 0;           // NaN
Infinity - Infinity; // NaN
"hello" * 5;     // NaN
undefined + 1;   // NaN
null * 2;        // 0 (not NaN!)

// Parsing failures
parseInt("hello"); // NaN
Number("abc");     // NaN
parseFloat("12.5.3"); // 12.5 (not NaN)
```

## Checking for NaN

```javascript
let x = NaN;

// ❌ Wrong: NaN !== NaN (unique property!)
console.log(x === NaN); // false!

// ✅ Right: Use isNaN()
console.log(isNaN(x)); // true

// ✅ Better: Use Number.isNaN()
console.log(Number.isNaN(x)); // true

// isNaN() vs Number.isNaN()
isNaN("hello");      // true (converts to number first)
Number.isNaN("hello"); // false (checks actual NaN)
```

## NaN Properties

```javascript
// NaN is the only value not equal to itself
console.log(NaN === NaN); // false

// NaN is a number type
console.log(typeof NaN); // "number"

// NaN is falsy
if (NaN) {
    console.log("This won't run");
}
```

## Quick Revision

- NaN = "Not a Number" (invalid math)
- Result of: `0/0`, `"hello"*5`, failed parsing
- `NaN !== NaN` (unique property!)
- Use `Number.isNaN()` to check
- typeof NaN returns "number"

---

## Related Topics

- [[Check-NaN]] - How to check for NaN
- [[What-is-DataType]] - Data types
- [[What-is-Primitive]] - Primitive types
- [[Type-Conversion]] - Type conversion