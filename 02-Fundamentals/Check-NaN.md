# How to Check for NaN

## The Problem

```javascript
// NaN is the only value not equal to itself!
NaN === NaN; // false!

// This makes checking difficult
let x = NaN;
if (x === NaN) { // This never works!
    console.log("Is NaN");
}
```

## Method 1: isNaN() (Global)

```javascript
isNaN(NaN);       // true
isNaN("hello");   // true (converts to number first)
isNaN("123");     // false
isNaN(undefined); // true
isNaN(null);      // false (converts to 0)
```

## Method 2: Number.isNaN() (Recommended)

```javascript
Number.isNaN(NaN);       // true
Number.isNaN("hello");   // false (no conversion!)
Number.isNaN("123");     // false
Number.isNaN(undefined); // false
Number.isNaN(null);      // false
```

## isNaN vs Number.isNaN

| Value | isNaN | Number.isNaN |
|-------|-------|--------------|
| NaN | true | true |
| "hello" | true | false |
| undefined | true | false |
| null | false | false |
| "123" | false | false |

## Polyfill

```javascript
// For old browsers
if (!Number.isNaN) {
    Number.isNaN = function(value) {
        return typeof value === 'number' && isNaN(value);
    };
}
```

## Quick Revision

- `NaN !== NaN` (unique property!)
- Use `Number.isNaN()` (recommended)
- `isNaN()` converts before checking
- `Number.isNaN()` checks actual NaN
- Always use `Number.isNaN()`

---

## Related Topics

- [[What-is-NaN]] - [[What-is-NaN|NaN]] overview
- [[What-is-Primitive]] - [[What-is-Primitive|Primitive types]]
- [[What-is-DataType]] - [[What-is-DataType|Data types]]
- [[Type-Conversion]] - [[Type-Conversion|Type conversion]]
