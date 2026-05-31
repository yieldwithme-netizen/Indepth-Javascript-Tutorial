# How to Avoid Type Coercion Bugs

## The Problem

```javascript
// Implicit coercion can cause unexpected results
"5" + 3;     // "53" (not 8!)
"5" - 3;     // 2 (works)
"5" == 5;    // true (confusing!)
```

## Solutions

### 1. Use Strict Equality

```javascript
// ❌ Bad: loose equality
if (value == null) { }

// ✅ Good: strict equality
if (value === null || value === undefined) { }
```

### 2. Explicit Conversion

```javascript
// ❌ Bad: relying on coercion
const result = "5" + 3; // "53"

// ✅ Good: explicit conversion
const result = Number("5") + 3; // 8
const result = String(5) + "3"; // "53"
```

### 3. Use Number.isNaN()

```javascript
// ❌ Bad: isNaN() converts first
isNaN("hello"); // true

// ✅ Good: Number.isNaN()
Number.isNaN("hello"); // false
Number.isNaN(NaN); // true
```

### 4. Use Nullish Coalescing

```javascript
// ❌ Bad: || treats 0 and "" as falsy
const port = config.port || 3000; // 0 becomes 3000!

// ✅ Good: ?? only treats null/undefined as nullish
const port = config.port ?? 3000; // 0 stays 0
```

## Quick Revision

- Always use `===` over `==`
- Convert types explicitly
- Use `Number.isNaN()` over `isNaN()`
- Use `??` over `||` for defaults
- Be aware of coercion rules

---

## Related Topics

- [[What-is-Type-Coercion]] - [[What-is-Type-Coercion|Type coercion]] overview
- [[What-is-Comparison]] - [[What-is-Comparison|Comparison operators]]
- [[Type-Conversion]] - [[Type-Conversion|Type conversion]]
- [[What-is-Nullish]] - [[What-is-Nullish|Nullish coalescing]]
