# Variables

## Definition
Variables are named containers that store data values in JavaScript. They are fundamental building blocks used to hold and manipulate data throughout your programs.

## Variable Declarations

### var (Function-scoped)
```javascript
var name = "John";
var age = 25;
var isActive = true;

// Redeclaration allowed
var name = "Jane";  // OK

// Function-scoped
function example() {
  var x = 10;
  if (true) {
    var y = 20;  // Same scope as x
  }
  console.log(y);  // 20 (accessible)
}
```

### let (Block-scoped)
```javascript
let count = 0;
let name = "John";

// Redeclaration NOT allowed
// let count = 1;  // SyntaxError

// Reassignment allowed
count = 1;  // OK

// Block-scoped
if (true) {
  let x = 10;
  console.log(x);  // 10
}
// console.log(x);  // ReferenceError
```

### const (Block-scoped, constant)
```javascript
const PI = 3.14159;
const API_URL = "https://api.example.com";

// Redeclaration NOT allowed
// const PI = 3;  // SyntaxError

// Reassignment NOT allowed
// PI = 3.14;  // TypeError

// But objects/arrays can be modified
const user = { name: "John" };
user.name = "Jane";  // OK (property change)
// user = {};  // TypeError (reassignment)
```

## Variable Naming Rules

```javascript
// ✅ Valid names
let firstName = "John";
let _private = true;
let $element = document.getElementById("app");
let camelCase = "style";
let PI = 3.14159;

// ❌ Invalid names
// let 1stName = "John";    // Can't start with number
// let my-name = "John";    // No hyphens
// let my name = "John";    // No spaces
// let class = "A";         // Reserved word
```

## Hoisting

```javascript
// var is hoisted
console.log(x);  // undefined
var x = 5;

// let/const are hoisted but not initialized
// console.log(y);  // ReferenceError
let y = 10;

// Temporal Dead Zone
{
  // TDZ starts
  // console.log(z);  // ReferenceError
  const z = 20;
  // TDZ ends
}
```

## Scope Comparison

```javascript
// Global scope
var globalVar = "I'm global";
let globalLet = "I'm global too";

function example() {
  // Function scope
  var functionVar = "I'm function scoped";
  let functionLet = "I'm function scoped too";

  if (true) {
    // Block scope
    var blockVar = "I'm function scoped (var)";
    let blockLet = "I'm block scoped";
    const blockConst = "I'm block scoped too";
  }

  console.log(blockVar);    // OK (function scoped)
  // console.log(blockLet);  // ReferenceError
}
```

## Initialization

```javascript
// Declaration only
let x;
console.log(x);  // undefined

// Declaration with initialization
let y = 10;
console.log(y);  // 10

// Multiple declarations
let a = 1, b = 2, c = 3;

// Underscore and dollar sign
let _count = 0;
let $element = null;
let __private = true;
```

## Common Use Cases
- Storing user input
- Managing state
- Caching values
- Loop counters
- Configuration values

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Using var in modern code | Use let/const instead |
| Not initializing variables | Initialize when declaring |
| Using const for changing values | Use let for reassignment |
| Confusing scope rules | Understand block vs function scope |

## Quick Revision Summary
- `var`: Function-scoped, hoisted, redeclarable
- `let`: Block-scoped, hoisted but not initialized, reassignable
- `const`: Block-scoped, constant, cannot reassign
- Use camelCase for variable names
- Always initialize variables when possible
- Prefer const > let > var

## Related Topics
- [[Scope]]
- [[Hoisting]]
- [[Temporal-Dead-Zone]]
- [[Block-Scope]]
- [[Function-Scope]]
- [[Constants]]
