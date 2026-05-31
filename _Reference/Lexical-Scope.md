# Lexical Scope

## Definition

Lexical scope means variables are resolved based on **where they are defined**.

## Example

```javascript
const outer = "outer";

function outerFunc() {
    const middle = "middle";
    
    function innerFunc() {
        console.log(outer);  // "outer"
        console.log(middle); // "middle"
    }
    
    innerFunc();
}
```

## Quick Revision

- Lexical scope = definition-based
- Inner functions access outer variables
- Closures preserve lexical scope

---

## Related Topics

- [[What-is-Scope]] - [[What-is-Scope|Scope]]
- [[Lexical Scope]] - [[Lexical Scope|Lexical scope]]
- [[Lexical-Scope]] - [[Lexical-Scope|Lexical scope]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
