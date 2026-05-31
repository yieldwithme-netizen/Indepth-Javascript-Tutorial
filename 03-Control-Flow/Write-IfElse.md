# How to Write if-else Statements

## Basic Syntax

```javascript
// Simple if
if (condition) {
    // runs if true
}

// if-else
if (condition) {
    // runs if true
} else {
    // runs if false
}

// if-else if-else
if (condition1) {
    // runs if condition1 is true
} else if (condition2) {
    // runs if condition2 is true
} else {
    // runs if all conditions are false
}
```

## Examples

```javascript
// Check age
const age = 20;

if (age >= 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}

// Multiple conditions
const score = 85;

if (score >= 90) {
    console.log("A");
} else if (score >= 80) {
    console.log("B");
} else if (score >= 70) {
    console.log("C");
} else {
    console.log("F");
}
```

## Common Mistakes

```javascript
// ❌ Wrong: Assignment instead of comparison
if (x = 5) { }  // Always true!

// ✅ Right: Use ===
if (x === 5) { }

// ❌ Wrong: Missing braces
if (x > 5)
    console.log("yes");
    console.log("always runs"); // Not part of if!

// ✅ Right: Always use braces
if (x > 5) {
    console.log("yes");
}
```

## Quick Revision

- `if` runs code if condition is true
- `else` runs code if condition is false
- `else if` checks multiple conditions
- Always use `===` for comparison
- Always use curly braces `{}`

---

## Related Topics

- [[What-is-IfElse]] - [[What-is-IfElse|If-else]] overview
- [[What-is-Switch]] - [[What-is-Switch|Switch statements]]
- [[What-is-Ternary]] - [[What-is-Ternary|Ternary operator]]
- [[Write-Switch]] - [[Write-Switch|Writing switch]]
