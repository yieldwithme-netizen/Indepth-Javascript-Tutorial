# How to Use Comparison Operators

## Equality Operators

```javascript
// Loose equality (==) - type coercion
5 == "5";      // true
0 == false;    // true
null == undefined; // true

// Strict equality (===) - no coercion
5 === "5";     // false
0 === false;   // false
null === undefined; // false
```

## Inequality Operators

```javascript
// Loose inequality (!=)
5 != "5";      // false

// Strict inequality (!==)
5 !== "5";     // true
```

## Relational Operators

```javascript
5 > 3;         // true
5 < 3;         // false
5 >= 5;        // true
5 <= 4;        // false
```

## String Comparison

```javascript
// Lexicographic (alphabetical)
"apple" > "banana"; // false
"10" > "9";        // true (string comparison!)
```

## Best Practice

```javascript
// ❌ Avoid loose equality
if (value == null) { }

// ✅ Prefer strict equality
if (value === null || value === undefined) { }

// ✅ Or use nullish coalescing
const result = value ?? "default";
```

## Quick Revision

- `==` compares values (with coercion)
- `===` compares values AND types
- Always use `===` and `!==`
- Strings compare lexicographic
- `null == undefined` is true

---

## Related Topics

- [[What-is-Comparison]] - [[What-is-Comparison|Comparison]] overview
- [[What-is-Type-Coercion]] - [[What-is-Type-Coercion|Type coercion]]
- [[What-is-Logical]] - [[What-is-Logical|Logical operators]]
- [[What-is-Operator]] - [[What-is-Operator|Operators]]
