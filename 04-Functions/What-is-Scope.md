# What is Scope?

## Definition

[[Scope]] determines **where [[Variable|variables]] are accessible** in your code.

## Three Types of [[Scope]]

### Global [[Scope]]

```javascript
// Variables declared outside any function/block
const global = "I'm everywhere";

function test() {
    console.log(global); // accessible
}

console.log(global); // accessible
```

### Function [[Scope]]

```javascript
function test() {
    const local = "I'm only here";
    console.log(local); // accessible
}

console.log(local); // ReferenceError!
```

### Block [[Scope]]

```javascript
if (true) {
    let blockScoped = "I'm in this block";
    var notBlockScoped = "I leak out!";
}

console.log(blockScoped); // ReferenceError!
console.log(notBlockScoped); // "I leak out!"
```

## [[Var]] vs [[Let]] vs [[Const]]

```javascript
// var: function scoped
function test() {
    if (true) {
        var x = 10;
    }
    console.log(x); // 10 (accessible!)
}

// let/const: block scoped
function test() {
    if (true) {
        let y = 10;
        const z = 20;
    }
    console.log(y); // ReferenceError!
    console.log(z); // ReferenceError!
}
```

## [[Scope]] Chain

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

- [[Scope]] = [[Variable]] accessibility
- Global: accessible everywhere
- Function: accessible only in function
- Block: accessible only in block ([[Let]]/[[Const]])
- [[Var]] is function scoped (not block)

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Understand-Scope]] - Scope chain
- [[What-is-Closure]] - Closures
- [[What-is-Variable]] - Variables
- [[Declare-Var]] - var keyword
- [[Declare-Let]] - let keyword