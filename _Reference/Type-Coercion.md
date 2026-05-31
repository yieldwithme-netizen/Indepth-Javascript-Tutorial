# Type Coercion

## Definition

Type coercion is **automatic type conversion** by JavaScript.

## Implicit Coercion

```javascript
"5" + 3;     // "53" (number → string)
"5" - 3;     // 2 (string → number)
true + 1;    // 2 (boolean → number)
```

## Explicit Coercion

```javascript
Number("5");     // 5
String(5);       // "5"
Boolean(0);      // false
```

## Quick Revision

- Implicit: automatic conversion
- Explicit: manual conversion
- `+` with string = concatenation
- `-`, `*`, `/` = number conversion
- Use `===` to avoid coercion

---

## Related Topics

- [[What-is-Type-Coercion]] - [[What-is-Type-Coercion|Type coercion]]
- [[Type-Coercion]] - [[Type-Coercion|Type coercion]]
- [[Type-Conversion]] - [[Type-Conversion|Type conversion]]
- [[Avoid-Coercion]] - [[Avoid-Coercion|Avoiding coercion]]
