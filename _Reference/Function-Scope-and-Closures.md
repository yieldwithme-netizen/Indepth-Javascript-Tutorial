# Function Scope and Closures

## Definition

**Function scope** refers to the visibility and accessibility of variables within a function. **Closures** are a powerful feature where a function retains access to variables from its enclosing (outer) scope even after the outer function has finished executing.

Scope determines where variables are accessible, while closures allow functions to "remember" the environment in which they were created.

---

## Scope Types

### Global Scope
```javascript
var globalVar = 'I am global';

function checkScope() {
  console.log(globalVar); // Accessible here
}

checkScope();
console.log(globalVar); // Accessible here too
```

### Function Scope
```javascript
function myFunction() {
  var localVar = 'I am local';
  console.log(localVar); // Accessible
}

console.log(localVar); // Error: localVar is not defined
```

### Block Scope (let/const)
```javascript
function blockScope() {
  if (true) {
    let blockLet = 'I am block scoped';
    const blockConst = 'Me too';
    var notBlock = 'I leak out!';
  }

  console.log(notBlock);     // 'I leak out!'
  console.log(blockLet);     // Error: blockLet is not defined
  console.log(blockConst);   // Error: blockConst is not defined
}
```

---

## Code Examples

### Lexical Scope
```javascript
function outer() {
  const outerVar = 'I am outer';

  function inner() {
    // Inner function has access to outer's variables
    console.log(outerVar);
  }

  inner();
}

outer(); // Output: I am outer
```

### Basic Closure
```javascript
function createGreeter(greeting) {
  // 'greeting' is captured in the closure
  return function(name) {
    return `${greeting}, ${name}!`;
  };
}

const sayHello = createGreeter('Hello');
const sayHi = createGreeter('Hi');

console.log(sayHello('Alice')); // Output: Hello, Alice!
console.log(sayHi('Bob'));      // Output: Hi, Bob!
```

### Closure for Data Privacy
```javascript
function createBankAccount(initialBalance) {
  let balance = initialBalance; // Private variable

  return {
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
        return `Deposited $${amount}. Balance: $${balance}`;
      }
    },
    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        return `Withdrew $${amount}. Balance: $${balance}`;
      }
      return 'Insufficient funds';
    },
    getBalance() {
      return balance;
    }
  };
}

const account = createBankAccount(100);
console.log(account.deposit(50));     // Output: Deposited $50. Balance: $150
console.log(account.withdraw(30));    // Output: Withdrew $30. Balance: $120
console.log(account.getBalance());    // Output: 120
// console.log(account.balance);      // undefined - private!
```

### Closure in Loops (Common Problem)
```javascript
// WRONG - Classic closure problem
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // All print 3!
  }, 100);
}

// CORRECT - Using let (block scope)
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // Prints 0, 1, 2
  }, 100);
}

// CORRECT - Using IIFE to create closure
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j); // Prints 0, 1, 2
    }, 100);
  })(i);
}
```

### Module Pattern with Closures
```javascript
const Counter = (function() {
  let count = 0; // Private variable

  function increment() { count++; }
  function decrement() { count--; }
  function getCount() { return count; }

  return {
    increment,
    decrement,
    getCount
  };
})();

Counter.increment();
Counter.increment();
Counter.increment();
Counter.decrement();
console.log(Counter.getCount()); // Output: 2
```

### Closure with Event Listeners
```javascript
function setupButton(buttonId, message) {
  const button = document.getElementById(buttonId);

  button.addEventListener('click', function() {
    // Closure captures 'message'
    alert(message);
  });
}

setupButton('btn1', 'Hello from Button 1!');
setupButton('btn2', 'Hello from Button 2!');
```

### Currying with Closures
```javascript
function multiply(a) {
  return function(b) {
    return a * b;
  };
}

const double = multiply(2);
const triple = multiply(3);

console.log(double(5));  // Output: 10
console.log(triple(5));  // Output: 15
console.log(multiply(4)(6)); // Output: 24
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Data Encapsulation** | Hide variables from external access |
| **Factory Functions** | Create objects with persistent state |
| **Memoization** | Cache function results using closures |
| **Partial Application** | Pre-fill function arguments |
| **Event Handlers** | Maintain context in callbacks |
| **Iterators/Generators** | Maintain state across iterations |

---

## Common Mistakes

### 1. Variable Shadowing
```javascript
const x = 10;

function shadow() {
  const x = 20; // Shadows outer 'x'
  console.log(x); // Output: 20
}

shadow();
console.log(x); // Output: 10
```

### 2. For-Loop Closure Issue
```javascript
// This prints 5 five times, not 0-4
for (var i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100);
}

// Fix: use let or IIFE
for (let i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100); // Prints 0, 1, 2, 3, 4
}
```

### 3. Memory Leaks with Closures
```javascript
function problematic() {
  const largeData = new Array(1000000).fill('x');

  return function() {
    // largeData stays in memory as long as this function exists
    return largeData.length;
  };
}

const fn = problematic();
// largeData is retained until fn is garbage collected
```

---

## Quick Revision Summary

- **Global scope**: Variables accessible everywhere
- **Function scope**: Variables accessible only within the function
- **Block scope**: Variables accessible only within the block (let/const)
- **Closures**: Functions that retain access to their outer scope variables
- Closures enable data privacy, factory functions, and stateful functions
- The classic for-loop closure problem is solved with `let` or IIFE
- Closures keep variables in memory — be mindful of memory usage

---

## Related Topics

- [[function]] - Function declarations and expressions
- [[let]] - Block-scoped variable declarations
- [[IIFE]] - Immediately Invoked Function Expressions
- [[JavaScript]] - JavaScript language overview
- [[Logical-Operators]] - Logical operators in conditions
