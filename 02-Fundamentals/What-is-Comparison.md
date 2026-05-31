# What is a Comparison Operator?

## Definition

Comparison operators compare two values and return a [[What-is-DataType#boolean|boolean]] (`true` or `false`).

## Equality Operators

### Loose Equality ([[What-is-Operator#Loose Equality (==)|==]])

```javascript
// Compares VALUE only (type coercion happens)
5 == 5;        // true
5 == "5";      // true (string → number)
0 == false;    // true
"" == false;   // true
null == undefined; // true
```

### Strict Equality ([[What-is-Operator#Strict Equality (===)|===]])

```javascript
// Compares VALUE and TYPE
5 === 5;       // true
5 === "5";     // false (different types)
0 === false;   // false
"" === false;  // false
null === undefined; // false
```

## Inequality Operators

### Loose Inequality ([[What-is-Operator#Loose Inequality (!=)|!=]])

```javascript
5 != "5";      // false (type coercion)
5 != 6;        // true
```

### Strict Inequality ([[What-is-Operator#Strict Inequality (!==)|!==]])

```javascript
5 !== "5";     // true (different types)
5 !== 6;       // true
```

## Relational Operators

```javascript
5 > 3;         // true (greater than)
5 < 3;         // false (less than)
5 >= 5;        // true (greater or equal)
5 <= 4;        // false (less or equal)
```

## Comparison Rules

```javascript
// [[What-is-DataType#number|Numbers]]
5 > 3;          // true
5 < 3;          // false

// Strings (lexicographic)
"apple" > "banana"; // false
"10" > "9";    // true (string comparison!)

// Mixed types
"5" > 3;       // true (string → number)
"5" < "3";     // false (string comparison)

// null/undefined
null == undefined; // true
null === undefined; // false
null > 0;      // false
null == 0;     // false
null >= 0;     // true (!)
```

## Best Practice

```javascript
// ❌ Avoid: Loose equality
if (value == null) { }

// ✅ Prefer: Strict equality
if (value === null || value === undefined) { }

// ✅ Or use nullish coalescing
const result = value ?? "default";
```

## Quick Revision

- `==` compares values (with type coercion)
- `===` compares values AND types (no coercion)
- Always use `===` and `!==`
- Strings compare lexicographic (alphabetical)
- `null == undefined` is true, `null === undefined` is false

---

## Related Topics

- [[What-is-Operator]] - Operators overview
- [[Use-Comparison]] - Using comparison operators
- [[What-is-Type-Coercion]] - Type coercion
- [[What-is-Logical]] - Logical operators