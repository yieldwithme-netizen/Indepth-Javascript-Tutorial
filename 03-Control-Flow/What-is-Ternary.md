# What is a [[ternary]] [[operator]]?

## Definition

The [[ternary]] [[operator]] is a **shorthand for [[if]]-[[else]]** that returns one of two values based on a [[condition]].

## Syntax

```javascript
condition ? valueIfTrue : valueIfFalse
```

## Examples

```javascript
// Basic
const age = 20;
const status = age >= 18 ? "adult" : "minor";

// With functions
function greet(name) {
    return name ? `Hello, ${name}!` : "Hello, Guest!";
}

// With template literals
const message = `You are ${age >= 18 ? "adult" : "minor"}`;

// With logical operators
const name = user?.name ?? "Anonymous";
```

## Nested [[ternary]] (Avoid!)

```javascript
// ❌ Hard to read
const result = a > b ? "a bigger" : a < b ? "b bigger" : "equal";

// ✅ Use if-else instead
let result;
if (a > b) {
    result = "a bigger";
} else if (a < b) {
    result = "b bigger";
} else {
    result = "equal";
}
```

## When to Use

```javascript
// ✅ Good: Simple condition
const x = isReady ? "Ready" : "Not Ready";

// ❌ Bad: Complex logic
const result = condition1 && condition2 || condition3 ? "A" : "B";

// ✅ Good: Default values
const name = input || "Anonymous";

// ✅ Good: Conditional rendering
element.innerHTML = isLoggedIn ? "Logout" : "Login";
```

## Quick Revision

- [[ternary]]: `condition ? true : false`
- Shorthand for simple [[if]]-[[else]]
- Returns a value (not just executes code)
- Avoid nested ternaries (hard to read)
- Best for simple assignments

---

## Related Topics

- [[What-is-IfElse]] - [[if]]-[[else]] statements
- [[What-is-Switch]] - [[switch]] statements
- [[Write-Ternary]] - Using [[ternary]]
- [[Write-IfElse]] - Writing [[if]]-[[else]]
