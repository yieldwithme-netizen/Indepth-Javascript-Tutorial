# Arrow Functions

Arrow functions are a concise syntax for writing functions in JavaScript, introduced in ES6. They provide lexical `this` binding and are ideal for callbacks and functional programming.

## Definition

Arrow functions use `=>` syntax and offer shorter function expressions. They don't have their own `this`, `arguments`, `super`, or `new.target` bindings.

**Syntax:**
```javascript
// Basic syntax
const functionName = (parameters) => {
    // function body
};

// Single parameter - parentheses optional
const square = x => x * x;

// No parameters - empty parentheses required
const greet = () => 'Hello!';

// Single expression - implicit return
const double = x => x * 2;

// Multiple statements - require braces and explicit return
const calculate = (a, b) => {
    const sum = a + b;
    return sum * 2;
};
```

## Basic Examples

```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function equivalent
const addArrow = (a, b) => a + b;

// Callbacks with arrow functions
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(x => x * 2);
const evens = numbers.filter(x => x % 2 === 0);
const sum = numbers.reduce((acc, x) => acc + x, 0);

// Object return (wrap in parentheses)
const createPerson = (name, age) => ({ name, age });
const person = createPerson('Alice', 30); // { name: 'Alice', age: 30 }
```

## Lexical `this` Binding

```javascript
// Arrow functions inherit 'this' from enclosing scope
class Timer {
    constructor() {
        this.seconds = 0;
    }
    
    start() {
        // 'this' refers to Timer instance
        setInterval(() => {
            this.seconds++;
            console.log(this.seconds);
        }, 1000);
    }
}

// Arrow functions don't have own 'this'
const obj = {
    name: 'Object',
    greet: () => {
        // 'this' is NOT obj - it's the outer scope
        console.log(this.name); // undefined (or window.name)
    }
};

// Regular function gets its own 'this'
const obj2 = {
    name: 'Object',
    greet() {
        // 'this' is obj2
        console.log(this.name); // 'Object'
    }
};
```

## No `arguments` Object

```javascript
// Arrow functions don't have 'arguments' object
const arrowFunc = () => {
    // console.log(arguments); // ReferenceError
};

// Use rest parameters instead
const arrowWithRest = (...args) => {
    console.log(args); // Array of arguments
};

arrowWithRest(1, 2, 3); // [1, 2, 3]
```

## Common Use Cases

```javascript
// Array methods
const doubled = [1, 2, 3].map(x => x * 2);
const evens = [1, 2, 3, 4].filter(x => x % 2 === 0);

// Promise chains
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));

// Event handlers
button.addEventListener('click', () => {
    this.classList.toggle('active');
});

// Ternary expressions
const isAdult = age => age >= 18 ? 'Adult' : 'Minor';

// Currying
const multiply = (a) => (b) => a * b;
const double = multiply(2);
double(5); // 10
```

## When NOT to Use Arrow Functions

```javascript
// Object methods - use regular functions or shorthand
const obj = {
    name: 'Object',
    // Bad: arrow function loses 'this' context
    badGreet: () => `Hello, ${this.name}`,
    // Good: method shorthand
    goodGreet() { return `Hello, ${this.name}` }
};

// Constructor functions - cannot use with 'new'
const Person = (name) => { this.name = name; };
// new Person('Alice'); // TypeError

// Prototype methods
function MyClass() {}
MyClass.prototype.method = () => {
    // 'this' won't refer to instance
};

// Event handlers where 'this' should be element
element.addEventListener('click', function() {
    // 'this' is element - correct
});

element.addEventListener('click', () => {
    // 'this' is outer scope - may be wrong
});
```

## Common Use Cases

- Callbacks for array methods (map, filter, reduce)
- Promise chains
- Short utility functions
- Event handlers (when lexical `this` is desired)
- Functional programming patterns

## Common Mistakes

1. **Using for object methods** - Arrow functions don't bind `this` to the object
2. **Using for constructors** - Cannot use `new` with arrow functions
3. **Missing parentheses for single object return** - Need `(obj)` not `obj`
4. **Overusing arrow functions** - Regular functions are clearer for longer logic
5. **Forgetting arrow functions lack `arguments`** - Use rest parameters instead

## Related Topics

- [[Function Expressions]]
- [[This Keyword]]
- [[Lexical Scope]]
- [[Higher-Order Functions]]
- [[Closures]]

## Quick Revision

| Syntax | Example |
|--------|---------|
| No params | `() => expr` |
| One param | `x => expr` |
| Multiple params | `(x, y) => expr` |
| Block body | `(x) => { return x; }` |
| Object return | `() => ({ key: value })` |
| Inherited `this` | Arrow functions share parent's `this` |
