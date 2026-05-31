# What is Type Coercion?

## Definition

Type coercion is JavaScript's **automatic conversion** of a value from one data type to another.

## Two Types

### Implicit Coercion (Automatic)

```javascript
// String concatenation
"5" + 3;       // "53" (number → string)
"5" + true;    // "5true" (boolean → string)

// Number conversion
"5" - 3;       // 2 (string → number)
"5" * 2;       // 10 (string → number)
"5" / 2;       // 2.5 (string → number)

// Boolean to number
true + 1;      // 2 (true → 1)
false + 1;     // 1 (false → 0)
true + true;   // 2

// Null and undefined
null + 1;      // 1 (null → 0)
undefined + 1; // NaN (undefined → NaN)
```

### Explicit Coercion (Manual)

```javascript
// To number
Number("5");      // 5
Number("hello");  // NaN
Number(true);     // 1
Number(false);    // 0
Number(null);     // 0
Number(undefined); // NaN
parseInt("5.5");  // 5
parseFloat("5.5"); // 5.5
+"5";            // 5

// To string
String(5);        // "5"
String(true);     // "true"
String(null);     // "null"
(5).toString();   // "5"

// To boolean
Boolean(0);       // false
Boolean("");      // false
Boolean(null);    // false
Boolean(undefined); // false
Boolean(NaN);     // false
Boolean("hello"); // true
Boolean(5);       // true
Boolean([]);      // true
Boolean({});      // true
```

## Gotchas

```javascript
// Equality with coercion
0 == false;      // true
"" == false;     // true
" " == false;    // true
null == undefined; // true
NaN == NaN;      // false

// No coercion with strict equality
0 === false;     // false
"" === false;    // false
null === undefined; // false
```

## Quick Revision

- Type coercion = automatic type conversion
- Implicit: JS does it automatically
- Explicit: You do it manually with Number(), String(), Boolean()
- Avoid: Use `===` to prevent coercion bugs
- Common: `"5" + 3 = "53"`, `"5" - 3 = 2`

---

## Related Topics

- [[What-is-DataType]] - Data types
- [[Avoid-Coercion]] - Avoiding coercion bugs
- [[Type-Conversion]] - Type conversion
- [[What-is-Comparison]] - Comparison operators