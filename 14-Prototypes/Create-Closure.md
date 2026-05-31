# How to Create Closures

## Definition

A **closure** is a function that retains access to its outer (enclosing) function's variables even after the outer function has finished executing. Closures are created every time a function is created.

## Creating Closures

### Basic Closure

```javascript
function outerFunction(outerVariable) {
  function innerFunction(innerVariable) {
    console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
  }
  return innerFunction;
}

const closure = outerFunction("hello");
closure("world"); // Outer: hello, Inner: world
```

### Counter with Closure

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count,
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
```

### Private Variables

```javascript
function createUser(name) {
  let _name = name;

  return {
    getName: () => _name,
    setName: (newName) => { _name = newName; },
  };
}

const user = createUser("Alice");
console.log(user.getName()); // Alice
user.setName("Bob");
console.log(user.getName()); // Bob
```

## Common Use Cases

- **Data privacy**: Encapsulate variables
- **Function factories**: Create specialized functions
- **Event handlers**: Preserve state in callbacks
- **Memoization**: Cache function results

## Common Mistakes

```javascript
// Mistake: Closure in a loop
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
// Output: 3, 3, 3 (not 0, 1, 2)

// Fix: Use let or IIFE
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
// Output: 0, 1, 2
```

## Related Topics

- [[What-is-This]]
- [[Use-Private]]
- [[Prototype-Chain]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Closure | Function retaining outer scope access |
| Created | Every time a function is defined |
| Use case | Data privacy, state preservation |
| Loop issue | Use `let` to avoid stale closures |
