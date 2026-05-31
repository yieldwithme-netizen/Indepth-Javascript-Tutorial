# How to Understand Scope Chain

## Global Scope

```javascript
// Variables declared outside any function/block
const global = "I'm everywhere";

function test() {
    console.log(global); // accessible
}

console.log(global); // accessible
```

## Function Scope

```javascript
function test() {
    const local = "I'm only here";
    console.log(local); // accessible
}

console.log(local); // ReferenceError!
```

## Block Scope

```javascript
if (true) {
    let blockScoped = "I'm in this block";
    var notBlockScoped = "I leak out!";
}

console.log(blockScoped); // ReferenceError!
console.log(notBlockScoped); // "I leak out!"
```

## Scope Chain

```javascript
const outer = "outer";

function outerFunc() {
    const middle = "middle";
    
    function innerFunc() {
        const inner = "inner";
        console.log(outer);  // accessible (scope chain)
        console.log(middle); // accessible (scope chain)
        console.log(inner);  // accessible (own scope)
    }
    
    innerFunc();
}
```

## Quick Revision

- Scope = variable accessibility
- Global: accessible everywhere
- Function: accessible only in function
- Block: accessible only in block (let/const)
- var is function scoped (not block)

---

## Related Topics

- [[What-is-Scope]] - [[What-is-Scope|Scope]] overview
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-Variable]] - [[What-is-Variable|Variables]]
- [[Understand-Scope]] - [[Understand-Scope|Understanding scope]]
