# What is a Closure?

## Definition

A closure is a function that **remembers its lexical scope** even when executed outside that scope.

## Basic Example

```javascript
function outer() {
    const message = "Hello";
    
    function inner() {
        console.log(message); // remembers 'message'
    }
    
    return inner;
}

const greet = outer();
greet(); // "Hello" (closure over 'message')
```

## Practical Examples

```javascript
// Counter
function createCounter() {
    let count = 0;
    return {
        increment: () => ++count,
        decrement: () => --count,
        getCount: () => count
    };
}

const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.getCount();  // 2

// Private variables
function createPerson(name) {
    return {
        getName: () => name,
        setName: (newName) => name = newName
    };
}

const person = createPerson("John");
person.getName(); // "John"
person.setName("Jane");
person.getName(); // "Jane"
```

## Closures in Loops

```javascript
// ❌ Problem: var is function scoped
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000);
}
// Output: 3, 3, 3

// ✅ Solution: let is block scoped
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000);
}
// Output: 0, 1, 2
```

## Quick Revision

- Closure = function + its lexical scope
- Remembers variables from outer function
- Use for: private data, factory functions
- Common in callbacks and event handlers
- `let` in loops avoids closure bugs

---

## Related Topics

- [[What-is-Closure]] - Closures overview
- [[Create-Closure]] - Creating closures
- [[What-is-Scope]] - Scope
- [[What-is-Function]] - Functions
- [[What-is-IIFE]] - IIFE
