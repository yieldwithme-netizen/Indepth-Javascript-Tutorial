# Function Scope

## Definition

Function scope means variables are **accessible only within** the function.

## Basic Example

```javascript
function test() {
    const x = 10;
    console.log(x); // accessible
}

console.log(x); // ReferenceError
```

## Function vs Block Scope

```javascript
// var: function scoped
function test() {
    if (true) {
        var x = 10;
    }
    console.log(x); // 10
}

// let/const: block scoped
function test() {
    if (true) {
        let x = 10;
    }
    console.log(x); // ReferenceError
}
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
- [[Function-Scope-and-Closures]] - [[Function-Scope-and-Closures|Scope and closures]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
