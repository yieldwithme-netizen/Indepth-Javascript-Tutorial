# How to Use Ternary Operator

The **ternary operator** is a shorthand for `if...else` statements. It takes three operands: a condition, a value if true, and a value if false.

## Syntax

```javascript
condition ? valueIfTrue : valueIfFalse;
```

## Basic Examples

```javascript
const age = 20;
const status = age >= 18 ? "adult" : "minor";
console.log(status); // "adult"

const num = 5;
const result = num % 2 === 0 ? "even" : "odd";
console.log(result); // "odd"
```

## Nested Ternary

```javascript
const score = 85;
const grade = score >= 90 ? "A" 
  : score >= 80 ? "B" 
  : score >= 70 ? "C" : "F";
console.log(grade); // "B"
```

## Ternary with Functions

```javascript
function getDiscount(isPremium) {
  return isPremium ? 0.2 : 0.1;
}

console.log(getDiscount(true));  // 0.2
console.log(getDiscount(false)); // 0.1
```

## Common Use Cases

- Conditional assignments
- Inline conditional rendering
- Simple conditional logic
- Default values

## Common Mistakes

```javascript
// ❌ Overly complex ternary (hard to read)
const x = a ? b ? c ? d : e : f : g;

// ✅ Use if...else for complex logic
if (a) {
  if (b) {
    // do something
  }
}

// ❌ Forgetting parentheses around condition
const y = age >= 18 ? "adult" : "minor"; // works but can be confusing

// ✅ Use parentheses for clarity
const z = (age >= 18) ? "adult" : "minor";
```

## Related Topics

- [[If-Else-Statement]]
- [[Comparison-Operators]]
- [[Logical-Operators]]
- [[Short-Circuit-Evaluation]]

## Quick Revision

| Concept | Syntax |
|---------|--------|
| Basic | `condition ? true : false` |
| Nested | `a ? b : c ? d : e` |
| With functions | `return cond ? val1 : val2` |

**Key Point:** Use ternary for simple conditions; use `if...else` for complex logic.