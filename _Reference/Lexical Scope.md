# Lexical Scope

## Definition

Lexical scope means variables are resolved based on **where they are defined** in the code.

## Example

```javascript
const outer = "outer";

function outerFunc() {
    const middle = "middle";
    
    function innerFunc() {
        const inner = "inner";
        console.log(outer);  // "outer" (lexical scope)
        console.log(middle); // "middle" (lexical scope)
        console.log(inner);  // "inner" (own scope)
    }
    
    innerFunc();
}

outerFunc();
```

## Closures and Lexical Scope

```javascript
function createCounter() {
    let count = 0; // lexical scope
    
    return {
        increment: () => ++count, // closure over count
        getCount: () => count
    };
}

const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.getCount(); // 2
```

## Quick Revision

- Lexical scope = where variable is defined
- Inner functions access outer variables
- Closures preserve lexical scope
- Scope chain: inner → outer → global

---

## Related Topics

- [[What-is-Scope]] - [[What-is-Scope|Scope]]
- [[Lexical Scope]] - [[Lexical Scope|Lexical scope]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
- [[Function-Scope-and-Closures]] - [[Function-Scope-and-Closures|Scope and closures]]
