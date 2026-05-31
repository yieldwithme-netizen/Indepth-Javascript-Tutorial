# Implicit Conversion

## Definition

Implicit conversion is **automatic type conversion** by JavaScript.

## Examples

```javascript
// String concatenation
"5" + 3;     // "53"

// Number conversion
"5" - 3;     // 2
"5" * 2;     // 10

// Boolean to number
true + 1;    // 2
false + 1;   // 1

// Null/undefined
null + 1;    // 1
undefined + 1; // NaN
```

## Quick Revision

- Implicit = automatic conversion
- `+` with string = concatenation
- `-`, `*`, `/` = number conversion
- Avoid with `===`

---

## Related Topics

- [[What-is-Type-Coercion]] - [[What-is-Type-Coercion|Type coercion]]
- [[Implicit-Conversion]] - [[Implicit-Conversion|Implicit conversion]]
- [[Type-Conversion]] - [[Type-Conversion|Type conversion]]
- [[Avoid-Coercion]] - [[Avoid-Coercion|Avoiding coercion]]
