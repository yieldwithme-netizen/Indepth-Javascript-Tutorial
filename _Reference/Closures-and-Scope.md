# Closures and Scope

## Definition
A closure is a function that retains access to its lexical scope even when executed outside that scope. Scope determines the accessibility of variables at different parts of your code.

## Scope Types

### Global Scope
```javascript
var globalVar = 'I am global';

function checkGlobal() {
  console.log(globalVar); // Accessible
}

console.log(globalVar); // Accessible everywhere
```

### Function Scope
```javascript
function myFunction() {
  var localVar = 'I am local';
  console.log(localVar); // Accessible
}

// console.log(localVar); // Error: localVar is not defined
```

### Block Scope (let/const)
```javascript
if (true) {
  let blockLet = 'Block scoped';
  const blockConst = 'Also block scoped';
  var notBlock = 'Function scoped';
}

// console.log(blockLet); // Error
// console.log(blockConst); // Error
console.log(notBlock); // Works! (var is function scoped)
```

## Closures Explained

### Basic Closure
```javascript
function outerFunction() {
  const outerVar = 'I am outer';
  
  function innerFunction() {
    console.log(outerVar); // Accesses outer scope
  }
  
  return innerFunction;
}

const closure = outerFunction();
closure(); // 'I am outer' - remembers outer scope
```

### Practical Closure Examples

### Counter
```javascript
function createCounter() {
  let count = 0;
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
```

### Function Factory
```javascript
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### Private Variables
```javascript
function createPerson(name) {
  let _name = name;
  
  return {
    getName: () => _name,
    setName: (newName) => { _name = newName; }
  };
}

const person = createPerson('John');
console.log(person.getName()); // 'John'
person.setName('Jane');
console.log(person.getName()); // 'Jane'
```

## Common Use Cases
- Data privacy and encapsulation
- Function factories and currying
- Event handlers with state
- Memoization and caching
- Module patterns

## Common Mistakes
- Accidental global variables in closures
- Loop variable capture issue (use `let` or IIFE)
- Memory leaks from unnecessary closures
- Not understanding lexical scope
- Overusing closures causing complexity

## Quick Revision Summary
- Scope determines variable accessibility
- Closures retain access to outer scope
- `let`/`const` create block scope
- `var` creates function scope
- Closures enable private variables and state
- Useful for factories, callbacks, and encapsulation

## Related Topics
- [[Variables-and-Types]]
- [[Functions]]
- [[Higher-Order-Functions]]
- [[Modules-ES6]]
- [[IIFE]]
