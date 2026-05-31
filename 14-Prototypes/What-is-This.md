# What is the `this` Keyword

## Definition

The `this` keyword refers to the object that is currently executing the code. Its value depends on how a function is called (the **execution context**).

## Binding Rules

### 1. Default Binding

```javascript
function showThis() {
  console.log(this);
}

showThis(); // globalThis (or undefined in strict mode)
```

### 2. Implicit Binding

```javascript
const obj = {
  name: "Alice",
  greet() {
    console.log(`Hello, ${this.name}`);
  },
};

obj.greet(); // Hello, Alice (this === obj)
```

### 3. Explicit Binding

```javascript
function greet() {
  console.log(`Hello, ${this.name}`);
}

const person = { name: "Bob" };

greet.call(person); // Hello, Bob
greet.apply(person); // Hello, Bob
const boundGreet = greet.bind(person);
boundGreet(); // Hello, Bob
```

### 4. `new` Binding

```javascript
function Person(name) {
  this.name = name;
}

const alice = new Person("Alice");
console.log(alice.name); // Alice
```

### 5. Arrow Functions (Lexical `this`)

```javascript
const obj = {
  name: "Charlie",
  greet: () => {
    console.log(`Hello, ${this.name}`); // this is NOT obj
  },
};

obj.greet(); // Hello, undefined

const obj2 = {
  name: "Dave",
  greet() {
    const arrow = () => console.log(`Hello, ${this.name}`);
    arrow(); // Hello, Dave (inherits from greet)
  },
};

obj2.greet(); // Hello, Dave
```

## Common Use Cases

- **Object methods**: Access object properties
- **Constructors**: Initialize object instances
- **Event handlers**: Reference the element or object
- **Callbacks**: Maintain context with `.bind()` or arrows

## Common Mistakes

```javascript
// Mistake: Losing context
const obj = {
  name: "Alice",
  delayedGreet() {
    setTimeout(function () {
      console.log(`Hello, ${this.name}`); // undefined
    }, 1000);
  },
};

// Fix: Use arrow function
const obj2 = {
  name: "Bob",
  delayedGreet() {
    setTimeout(() => {
      console.log(`Hello, ${this.name}`); // Bob
    }, 1000);
  },
};
```

## Related Topics

- [[Create-Closure]]
- [[Use-Private]]
- [[Prototype-Chain]]

## Quick Revision

| Binding | Trigger | `this` Value |
|---------|---------|--------------|
| Default | Bare function call | `globalThis` / `undefined` |
| Implicit | Method call `obj.fn()` | `obj` |
| Explicit | `call/apply/bind` | Passed object |
| `new` | `new Constructor()` | New instance |
| Arrow | Lexical | Enclosing `this` |
