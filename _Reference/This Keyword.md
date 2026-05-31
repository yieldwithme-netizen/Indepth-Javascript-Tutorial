# The `this` Keyword

## Definition

The `this` keyword in JavaScript refers to the context in which a function is executed. Its value depends on how the function is called, not where it's defined. Understanding `this` is essential for writing object-oriented and functional JavaScript code.

## Code Examples

### Global Context

```javascript
// In the global scope, 'this' refers to the global object
console.log(this); // window (browser) or global (Node.js)

// In strict mode, 'this' is undefined at top level
"use strict";
console.log(this); // undefined
```

### Object Method

```javascript
const person = {
  name: "Alice",
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  },
};

person.greet(); // "Hello, I'm Alice"
// 'this' refers to the object calling the method
```

### Function Context

```javascript
function showThis() {
  console.log(this);
}

showThis(); // window (non-strict) or undefined (strict)

const obj = { name: "Bob" };
obj.show = showThis;
obj.show(); // { name: "Bob" }
```

### Arrow Functions

```javascript
const person = {
  name: "Alice",
  greet: () => {
    // Arrow functions don't have their own 'this'
    console.log(this); // window - NOT the person object
  },
  delayedGreet() {
    setTimeout(() => {
      // Arrow function inherits 'this' from delayedGreet
      console.log(`Hello, I'm ${this.name}`);
    }, 1000);
  },
};

person.greet();          // window
person.delayedGreet();   // "Hello, I'm Alice" (after 1 second)
```

### Explicit Binding

```javascript
function greet(greeting) {
  console.log(`${greeting}, I'm ${this.name}`);
}

const alice = { name: "Alice" };
const bob = { name: "Bob" };

// call - invokes immediately, args passed individually
greet.call(alice, "Hello");  // "Hello, I'm Alice"

// apply - invokes immediately, args passed as array
greet.apply(bob, ["Hi"]);    // "Hi, I'm Bob"

// bind - returns a new function with 'this' bound
const greetAlice = greet.bind(alice);
greetAlice("Hey");           // "Hey, I'm Alice"
```

### Constructor Functions

```javascript
function Person(name) {
  this.name = name;
  this.greet = function () {
    return `Hello, I'm ${this.name}`;
  };
}

const alice = new Person("Alice");
console.log(alice.greet()); // "Hello, I'm Alice"
// 'this' refers to the new object being created
```

### Class Context

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a noise`;
  }

  bindSpeak() {
    // Arrow function captures 'this' from the class
    return () => `${this.name} makes a noise`;
  }
}

const dog = new Animal("Rex");
const speak = dog.bindSpeak();
console.log(speak()); // "Rex makes a noise"
```

### Event Handlers

```javascript
// DOM event handler - 'this' is the element
const button = document.querySelector("button");

button.addEventListener("click", function () {
  console.log(this); // <button> element
});

// Arrow function in event handler
button.addEventListener("click", () => {
  console.log(this); // window - NOT the button!
});
```

## Rules Summary

| Calling Pattern | `this` Value |
|----------------|--------------|
| Global call | `window` / `global` |
| Object method | The object |
| Constructor | New instance |
| `call`/`apply` | Specified object |
| `bind` | Permanently bound object |
| Arrow function | Inherits from enclosing scope |
| Event handler | The element (if regular function) |

## Common Mistakes

```javascript
// Mistake 1: Losing 'this' in callbacks
const person = {
  name: "Alice",
  greetLater() {
    setTimeout(function () {
      console.log(this.name); // undefined - 'this' is window
    }, 1000);
  },
};

// Fix: use arrow function
const personFixed = {
  name: "Alice",
  greetLater() {
    setTimeout(() => {
      console.log(this.name); // "Alice"
    }, 1000);
  },
};

// Mistake 2: Arrow functions don't have their own 'this'
class Counter {
  count = 0;

  // Wrong: arrow in class property doesn't work with 'this' properly
  // increment = () => {
  //   this.count++; // 'this' might not be the instance
  // };

  increment() {
    this.count++;
  }
}

// Mistake 3: Assuming 'this' in nested functions
const obj = {
  name: "outer",
  method() {
    function inner() {
      console.log(this.name); // undefined, not "outer"
    }
    inner();
  },
};
```

## Related Topics

- [[Arrow-Functions]]
- [[Classes]]
- [[Prototypes]]
- [[Object-Methods]]
- [[Function-Scope]]
- [[Event-Handling]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| `this` | Context object for function execution |
| Global scope | `window` (browser) / `global` (Node) |
| Method call | The object the method belongs to |
| Arrow function | Inherits `this` from parent scope |
| `call()` | Invoke with explicit `this`, individual args |
| `apply()` | Invoke with explicit `this`, array of args |
| `bind()` | Return new function with fixed `this` |
| Constructor | The new object being created |
