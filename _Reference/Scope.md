# Scope in JavaScript

## Definition

Scope determines the accessibility and visibility of variables, functions, and objects at various parts of your code during runtime. It defines where variables are declared and where they can be referenced.

## Types of Scope

### 1. Global Scope

Variables declared outside any function or block have global scope and can be accessed from anywhere.

```javascript
var globalVar = "I'm global";
let globalLet = "Also global";
const globalConst = "Me too";

function example() {
  console.log(globalVar); // "I'm global"
  console.log(globalLet); // "Also global"
}

example();
console.log(globalVar); // "I'm global"
```

### 2. Function Scope

Variables declared inside a function are only accessible within that function.

```javascript
function myFunction() {
  var functionScoped = "I'm function scoped";
  let alsoFunctionScoped = "Me too";
  const meThree = "And me";
  
  console.log(functionScoped); // Works
}

console.log(functionScoped); // ReferenceError
```

### 3. Block Scope

Variables declared with `let` and `const` are block-scoped (contained within `{}`).

```javascript
if (true) {
  let blockLet = "I'm block scoped";
  const blockConst = "Me too";
  var notBlockScoped = "I leak out!";
  
  console.log(blockLet); // Works
}

console.log(blockLet); // ReferenceError
console.log(notBlockScoped); // "I leak out!" (var ignores blocks)
```

### 4. Lexical Scope

Inner functions have access to variables from outer functions.

```javascript
function outer() {
  const outerVar = "I'm from outer";
  
  function inner() {
    console.log(outerVar); // "I'm from outer"
  }
  
  inner();
}

outer();
```

## Scope Chain

```javascript
const global = "global";

function level1() {
  const level1Var = "level 1";
  
  function level2() {
    const level2Var = "level 2";
    
    function level3() {
      const level3Var = "level 3";
      
      // Can access all outer scopes
      console.log(global);     // "global"
      console.log(level1Var);  // "level 1"
      console.log(level2Var);  // "level 2"
      console.log(level3Var);  // "level 3"
    }
    
    level3();
  }
  
  level2();
}

level1();
```

## Hoisting and Scope

```javascript
// var is hoisted
console.log(hoistedVar); // undefined
var hoistedVar = "I'm hoisted";

// let/const are hoisted but not initialized (TDZ)
// console.log(tdz); // ReferenceError: Cannot access before initialization
let tdz = "Temporal Dead Zone";

// Function declarations are fully hoisted
console.log(hoistedFunction()); // "I'm fully hoisted"
function hoistedFunction() {
  return "I'm fully hoisted";
}
```

## Closures and Scope

```javascript
function createCounter() {
  let count = 0; // Private variable
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// count is not directly accessible
```

## Common Use Cases

### Data Privacy

```javascript
function createBankAccount(initialBalance) {
  let balance = initialBalance; // Private
  
  return {
    deposit(amount) {
      if (amount > 0) balance += amount;
    },
    withdraw(amount) {
      if (amount > 0 && amount <= balance) balance -= amount;
    },
    getBalance() {
      return balance;
    }
  };
}
```

### Loop Iteration

```javascript
// ❌ Problem with var in loops
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // 3, 3, 3
}

// ✅ Solution: Use let
for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 100); // 0, 1, 2
}
```

### Module Pattern

```javascript
const module = (() => {
  const privateVar = "I'm private";
  
  return {
    publicMethod() {
      return privateVar;
    }
  };
})();
```

## Common Mistakes

```javascript
// ❌ Wrong: var in conditional
if (true) {
  var leak = "Leaks!";
}
console.log(leak); // "Leaks!" - Unexpected

// ✅ Correct: Use let/const
if (true) {
  const contained = "Contained!";
}
// console.log(contained); // ReferenceError

// ❌ Wrong: Shadowing confusion
let x = 10;
function example() {
  let x = 20; // Shadows outer x
  console.log(x); // 20
}

// ✅ Better: Avoid shadowing
let y = 10;
function betterExample() {
  let z = 20; // Different name
  console.log(y + z); // 30
}
```

## Variable Scope Comparison

| Keyword | Scope | Hoisted | Reassignable |
|---------|-------|---------|--------------|
| `var` | Function | Yes (undefined) | Yes |
| `let` | Block | Yes (TDZ) | Yes |
| `const` | Block | Yes (TDZ) | No |

## Related Topics

- [[Hoisting]] - How declarations are moved to top
- [[Closures]] - Functions with access to outer scope
- [[this-keyword]] - Context binding in different scopes
- [[IIFE]] - Immediately Invoked Function Expressions
- [[Modules]] - Scope isolation in modules
- [[Block-Scope]] - Detailed block scope behavior

## Quick Revision

**Three main scopes:**
- **Global**: Accessible everywhere
- **Function**: Local to function
- **Block**: Local to `{}` (let/const only)

**Key rules:**
- Inner scope can access outer scope (not reverse)
- `var` ignores block scope
- `let`/`const` respect block scope
- Closures preserve scope

**Best practices:**
- Always use `let`/`const` over `var`
- Minimize global variables
- Use closures for data privacy
- Avoid variable shadowing