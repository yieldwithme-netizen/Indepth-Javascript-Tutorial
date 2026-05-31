# How to Declare Functions

## Function Declaration

```javascript
// Basic declaration
function greet(name) {
    return `Hello, ${name}!`;
}

// Hoisted (can call before declaration)
greet("John"); // Works!
function greet(name) {
    return `Hello, ${name}!`;
}
```

## Function Expression

```javascript
// Assigned to variable
const greet = function(name) {
    return `Hello, ${name}!`;
};

// NOT hoisted (must declare first)
greet("John"); // Works!
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

## Arrow Function (ES6)

```javascript
// Basic arrow function
const greet = (name) => `Hello, ${name}!`;

// With multiple statements
const greet = (name) => {
    const message = `Hello, ${name}!`;
    return message;
};

// No parameters
const sayHello = () => "Hello!";
```

## Naming Conventions

```javascript
// Use camelCase
function calculateTotal() {}
function getUserData() {}
function isValid() {}

// Use descriptive names
function calculateTotalPrice() {} // ✅ Good
function calc() {} // ❌ Bad
```

## Quick Revision

- Function declaration: `function name() {}`
- Function expression: `const name = function() {}`
- Arrow function: `const name = () => {}`
- Declarations are hoisted, expressions are not
- Use descriptive names (camelCase)

---

## Related Topics

- [[What-is-Function]] - [[What-is-Function|Functions]] overview
- [[What-is-Expression]] - [[What-is-Expression|Function expressions]]
- [[What-is-Arrow]] - [[What-is-Arrow|Arrow functions]]
- [[Write-Arrow]] - [[Write-Arrow|Writing arrow functions]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
