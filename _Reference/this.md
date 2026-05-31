# The `this` Keyword in JavaScript

## Definition

The `this` keyword refers to the object that is executing the current function. Its value depends on how the function is called (execution context), not where it's defined. `this` is one of the most confusing concepts in JavaScript because it changes behavior based on context.

---

## Different Bindings of `this`

### 1. Global Context

In the global scope, `this` refers to the global object (`window` in browsers, `global` in Node.js).

```javascript
console.log(this); // Window object (in browser)

var name = "Global";
console.log(this.name); // "Global"
```

### 2. Object Method

When a function is called as a method of an object, `this` refers to that object.

```javascript
const person = {
  name: "Alice",
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

person.greet(); // "Hello, I'm Alice"
```

### 3. Constructor Function

In a constructor function or class, `this` refers to the newly created instance.

```javascript
function Person(name) {
  this.name = name;
}

const alice = new Person("Alice");
console.log(alice.name); // "Alice"

class Animal {
  constructor(name) {
    this.name = name;
  }
}

const dog = new Animal("Rex");
console.log(dog.name); // "Rex"
```

### 4. Arrow Functions

Arrow functions do NOT have their own `this`. They inherit `this` from the enclosing lexical scope.

```javascript
const obj = {
  name: "Alice",
  greet: () => {
    console.log(this.name); // undefined (inherits global this)
  }
};

obj.greet();

// Fix: Use regular function
const obj2 = {
  name: "Alice",
  greet() {
    console.log(this.name); // "Alice"
  }
};
```

### 5. Explicit Binding

You can explicitly set `this` using `call()`, `apply()`, or `bind()`.

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const person = { name: "Alice" };

// call() - invokes immediately
greet.call(person, "Hello"); // "Hello, Alice"

// apply() - invokes immediately with array of arguments
greet.apply(person, ["Hello"]); // "Hello, Alice"

// bind() - returns new function with bound this
const greetAlice = greet.bind(person);
greetAlice("Hello"); // "Hello, Alice"
```

### 6. Event Handlers

In DOM event handlers, `this` refers to the element that received the event.

```javascript
const button = document.querySelector("button");

button.addEventListener("click", function() {
  console.log(this); // the button element
});

// Arrow function - different behavior
button.addEventListener("click", () => {
  console.log(this); // window (or enclosing scope)
});
```

---

## Common Use Cases

### Callback Functions

```javascript
const timer = {
  seconds: 0,
  start() {
    setInterval(function() {
      this.seconds++; // Bug: this is window
      console.log(this.seconds);
    }, 1000);
  }
};

// Fix 1: Use arrow function
timer.start = function() {
  setInterval(() => {
    this.seconds++;
    console.log(this.seconds);
  }, 1000);
};

// Fix 2: Use bind
timer.start = function() {
  setInterval(function() {
    this.seconds++;
    console.log(this.seconds);
  }.bind(this), 1000);
};
```

### Class Methods

```javascript
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count++;
    return this; // Enables method chaining
  }

  decrement() {
    this.count--;
    return this;
  }

  reset() {
    this.count = 0;
    return this;
  }
}

const counter = new Counter();
counter.increment().increment().increment().reset();
console.log(counter.count); // 0
```

### Partial Application

```javascript
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);
const triple = multiply.bind(null, 3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

---

## Common Mistakes

### Mistake 1: Losing `this` in Callbacks

```javascript
class Timer {
  constructor() {
    this.seconds = 0;
  }

  start() {
    // Wrong: loses this context
    setInterval(function() {
      this.seconds++;
    }, 1000);

    // Correct: use arrow function
    setInterval(() => {
      this.seconds++;
    }, 1000);
  }
}
```

### Mistake 2: Forgetting `new` with Constructor

```javascript
function Person(name) {
  this.name = name;
}

// Wrong: without new, this is global object
const person = Person("Alice");
console.log(window.name); // "Alice"

// Correct: use new
const person2 = new Person("Alice");
```

### Mistake 3: Arrow Functions in Object Methods

```javascript
const obj = {
  name: "Alice",
  // Wrong: arrow function inherits outer this
  greet: () => {
    console.log(this.name); // undefined
  },
  // Correct: regular function
  greetProper() {
    console.log(this.name); // "Alice"
  }
};
```

---

## Quick Revision Summary

| Binding | `this` refers to |
|---------|-------------------|
| Global scope | `window` / `global` |
| Object method | The object |
| Constructor / `new` | New instance |
| Arrow function | Lexical scope (enclosing) |
| `call()` / `apply()` | Explicitly provided object |
| `bind()` | Returns function with fixed `this` |
| Event handler | Element that triggered event |

---

## Related Topics

- [[Promise]] - Promise callbacks and `this` context
- [[Object]] - Object methods and `this`
- [[Array]] - Array methods and `this` argument
- [[loop]] - Loop patterns and `this` binding
