# in Operator

## Definition

The `in` operator checks if a **property exists** in an object.

## Example

```javascript
const person = { name: "John", age: 30 };

"name" in person;  // true
"email" in person; // false

// With for...in loop
for (const key in person) {
    console.log(key); // "name", "age"
}
```

## Quick Revision

- `in` checks property existence
- Returns boolean
- Use in for...in loops
- Also checks prototype chain

---

## Related Topics

- [[What-is-Operator]] - [[What-is-Operator|Operators]]
- [[What-is-ForIn]] - [[What-is-ForIn|For...in]]
- [[in]] - [[in|in operator]]
- [[What-is-Object]] - [[What-is-Object|Objects]]
