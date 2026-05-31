# Function Scope

## Definition

Function scope means variables are **accessible only within** the function.

## Example

```javascript
function test() {
    const x = 10;
    console.log(x); // accessible
}

console.log(x); // ReferenceError
```

## Quick Revision

- Function scope: accessible in function
- `var` is function scoped
- `let/const` are block scoped
- Inner functions access outer variables

---

## Related Topics

- [[What-is-Scope]] - [[What-is-Scope|Scope]]
- [[Function-Scope]] - [[Function-Scope|Function scope]]
- [[Function Scope]] - [[Function Scope|Function scope]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
