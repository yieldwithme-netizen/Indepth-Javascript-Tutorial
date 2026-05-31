# Closures in JavaScript

## Definition

A closure is a function that retains access to variables from its outer (enclosing) function's scope, even after the outer function has returned. Closures are created every time a function is created.

## How Closures Work

```javascript
function outerFunction() {
  const outerVariable = "I'm from outer";

  function innerFunction() {
    console.log(outerVariable); // Accesses outer scope
  }

  return innerFunction;
}

const closure = outerFunction();
closure(); // "I'm from outer" - outerVariable is still accessible
```

## Code Examples

### Basic Closure

```javascript
function createGreeter(greeting) {
  return function(name) {
    return `${greeting}, ${name}!`;
  };
}

const hello = createGreeter("Hello");
const goodbye = createGreeter("Goodbye");

console.log(hello("Alice"));    // "Hello, Alice!"
console.log(goodbye("Bob"));    // "Goodbye, Bob!"
```

### Counter with Closure

```javascript
function createCounter(initial = 0) {
  let count = initial;

  return {
    increment() { return ++count; },
    decrement() { return --count; },
    getCount() { return count; },
    reset() { count = initial; }
  };
}

const counter = createCounter(10);
console.log(counter.increment()); // 11
console.log(counter.increment()); // 12
console.log(counter.getCount());  // 12
console.log(counter.reset());     // 10
```

### Private Variables

```javascript
function createBankAccount(owner, initialBalance) {
  let balance = initialBalance;
  const transactions = [];

  return {
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
        transactions.push({ type: "deposit", amount, date: new Date() });
        return true;
      }
      return false;
    },

    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        transactions.push({ type: "withdrawal", amount, date: new Date() });
        return true;
      }
      return false;
    },

    getBalance() {
      return balance;
    },

    getTransactions() {
      return [...transactions]; // Return copy, not reference
    }
  };
}

const account = createBankAccount("Alice", 1000);
account.deposit(500);
account.withdraw(200);
console.log(account.getBalance()); // 1300
console.log(account.getTransactions());
```

### Event Handler Closure

```javascript
function setupButton(buttonId, message) {
  const button = document.getElementById(buttonId);

  button.addEventListener("click", function() {
    alert(message); // Closure captures message
  });
}

setupButton("btn1", "Hello!");
setupButton("btn2", "Goodbye!");
```

### Loop Closure Issue

```javascript
// Problem: var doesn't create block scope
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // All log 3
  }, 1000);
}

// Solution 1: Use let (block scope)
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 0, 1, 2
  }, 1000);
}

// Solution 2: Use IIFE to create closure
for (var i = 0; i < 3; i++) {
  (function(index) {
    setTimeout(function() {
      console.log(index); // 0, 1, 2
    }, 1000);
  })(i);
}
```

### Memoization

```javascript
function memoize(fn) {
  const cache = {};

  return function(...args) {
    const key = JSON.stringify(args);

    if (!(key in cache)) {
      cache[key] = fn(...args);
    }

    return cache[key];
  };
}

const expensiveCalculation = memoize((n) => {
  console.log("Computing...");
  return n * n;
});

console.log(expensiveCalculation(4)); // Computing... 16
console.log(expensiveCalculation(4)); // 16 (cached, no "Computing...")
```

## Common Use Cases

- Data privacy and encapsulation
- Function factories
- Event handlers and callbacks
- Memoization and caching
- Partial application and currying
- Module pattern

## Common Mistakes

1. **Memory leaks**: Closures keep references to outer variables alive

```javascript
function problematic() {
  const largeData = new Array(1000000).fill("*");

  return function() {
    // largeData stays in memory even if not used
    return "done";
  };
}
```

2. **Loop variable capture**: Classic closure-in-loop issue with `var`

3. **Accidental global variables**: Forgetting `var`/`let`/`const` in inner scope

## Related Topics

- [[Scope]] - Variable scope rules
- [[Lexical-Scope]] - Static scope resolution
- [[IIFE]] - Immediately Invoked Function Expressions
- [[Higher-Order-Functions]] - Functions that return functions
- [[Module-Pattern]] - Using closures for modules
- [[Private-Methods]] - Encapsulation techniques

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| Closure | Function + its lexical environment |
| Scope chain | Inner functions access outer variables |
| Data privacy | Variables hidden from outside |
| Memory | Closures retain references |
| Use cases | Callbacks, factories, encapsulation |
| Fix loop issue | Use `let` or IIFE |
