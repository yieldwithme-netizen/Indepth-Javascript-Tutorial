# How to Use the `this` Keyword

## Definition

`this` refers to the **current execution context** — the object that is currently executing the code. Its value depends on how a function is called.

## Different Contexts

```javascript
// 1. Global context
console.log(this); // window (browser) or global (Node.js)

// 2. Object method
const person = {
    name: "John",
    greet() {
        return `Hello, ${this.name}`;
    }
};
console.log(person.greet()); // "Hello, John"

// 3. Regular function
function standalone() {
    return this;
}
console.log(standalone()); // window (non-strict) or undefined (strict)

// 4. Constructor function
function Person(name) {
    this.name = name;
}
const john = new Person("John");
console.log(john.name); // "John"
```

## Method Context

```javascript
const calculator = {
    value: 0,
    add(n) {
        this.value += n;
        return this; // enable chaining
    },
    subtract(n) {
        this.value -= n;
        return this;
    },
    result() {
        return this.value;
    }
};

const calc = calculator.add(5).add(3).subtract(2);
console.log(calc.result()); // 6
```

## Arrow Functions — Lexical `this`

```javascript
class Timer {
    constructor() {
        this.seconds = 0;
    }

    start() {
        // Arrow function inherits `this` from enclosing scope
        setInterval(() => {
            this.seconds++;
            console.log(this.seconds);
        }, 1000);
    }
}

const timer = new Timer();
timer.start(); // Works correctly (this = Timer instance)
```

## Binding `this`

```javascript
function greet(greeting) {
    return `${greeting}, ${this.name}`;
}

const person = { name: "John" };

// 1. call — immediate invocation
console.log(greet.call(person, "Hello")); // "Hello, John"

// 2. apply — arguments as array
console.log(greet.apply(person, ["Hi"])); // "Hi, John"

// 3. bind — returns new function
const greetJohn = greet.bind(person);
console.log(greetJohn("Hey")); // "Hey, John"
```

## Event Handlers

```javascript
class Button {
    constructor(label) {
        this.label = label;
    }

    setup() {
        const btn = document.createElement("button");
        btn.textContent = this.label;

        // ❌ Problem: `this` changes in event handler
        btn.addEventListener("click", function() {
            console.log(this.label); // undefined (this = button element)
        });

        // ✅ Solution 1: Arrow function
        btn.addEventListener("click", () => {
            console.log(this.label); // "Submit" (this = Button instance)
        });

        // ✅ Solution 2: bind
        btn.addEventListener("click", function() {
            console.log(this.label);
        }.bind(this));

        document.body.appendChild(btn);
    }
}
```

## Class Methods

```javascript
class User {
    constructor(name) {
        this.name = name;
    }

    // Regular method — `this` is the instance
    getName() {
        return this.name;
    }

    // Getter — `this` is the instance
    get info() {
        return `User: ${this.name}`;
    }
}

const user = new User("John");
console.log(user.getName()); // "John"
console.log(user.info);      // "User: John"
```

## Explicit `this` with `call`, `apply`, `bind`

```javascript
function introduce(greeting, punctuation) {
    return `${greeting}, I'm ${this.name}${punctuation}`;
}

const john = { name: "John" };
const jane = { name: "Jane" };

// call — pass arguments individually
console.log(introduce.call(john, "Hi", "!")); // "Hi, I'm John!"

// apply — pass arguments as array
console.log(introduce.apply(jane, ["Hello", "."])); // "Hello, I'm Jane."

// bind — create new function
const johnIntroduce = introduce.bind(john);
console.log(johnIntroduce("Hey", "?")); // "Hey, I'm John?"
```

## Common Use Cases

- **Object methods** — accessing object properties
- **Constructors** — initializing object state
- **Callbacks** — preserving context with arrow functions
- **Function borrowing** — using `call`/`apply`/`bind`

## Common Mistakes

```javascript
// ❌ Losing `this` in callbacks
const person = {
    name: "John",
    greet() {
        setTimeout(function() {
            console.log(this.name); // undefined (this = window)
        }, 1000);
    }
};

// ✅ Use arrow function
const personFixed = {
    name: "John",
    greet() {
        setTimeout(() => {
            console.log(this.name); // "John"
        }, 1000);
    }
};

// ❌ Using arrow function as method
const bad = {
    name: "John",
    greet: () => {
        return `Hello, ${this.name}`; // undefined (this = window)
    }
};

// ✅ Use regular function or shorthand method
const good = {
    name: "John",
    greet() {
        return `Hello, ${this.name}`; // "John"
    }
};
```

## Quick Revision

- `this` — current execution context
- Object method — `this` = object
- Regular function — `this` = window/undefined
- Constructor — `this` = new instance
- Arrow function — inherits `this` from parent scope
- `call`/`apply`/`bind` — explicitly set `this`
- Arrow functions for callbacks to preserve `this`

---

## Related Topics

- [[What-is-This]] — this keyword overview
- [[What-is-Function]] — Functions
- [[What-is-Class]] — Classes
- [[Create-Closure]] — Closures
- [[What-is-ProtoInheritance]] — Prototypal inheritance
