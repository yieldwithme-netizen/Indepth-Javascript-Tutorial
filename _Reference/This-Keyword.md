# This Keyword

## Definition

The `this` keyword refers to the **object that is executing the current function**.

## Binding Rules

### 1. Global Context

```javascript
console.log(this); // Window (browser) or global (Node.js)
```

### 2. Object Method

```javascript
const person = {
    name: "John",
    greet() {
        console.log(this.name); // "John"
    }
};
```

### 3. Function Call

```javascript
function greet() {
    console.log(this); // Window (strict: undefined)
}
greet();
```

### 4. Constructor

```javascript
class Person {
    constructor(name) {
        this.name = name; // new object being created
    }
}
```

### 5. Arrow Function

```javascript
const person = {
    name: "John",
    greet: () => {
        console.log(this.name); // undefined (inherits parent)
    }
};
```

## Explicit Binding

```javascript
function greet() {
    console.log(this.name);
}

const person = { name: "John" };

// call
greet.call(person); // "John"

// apply
greet.apply(person); // "John"

// bind
const greetPerson = greet.bind(person);
greetPerson(); // "John"
```

## Quick Revision

- `this` = current execution context
- Global: `this` = window/global
- Method: `this` = owner object
- Constructor: `this` = new object
- Arrow: inherits parent `this`
- call/apply/bind: explicit binding

---

## Related Topics

- [[What-is-This]] - [[What-is-This|this keyword]]
- [[Use-This]] - [[Use-This|Using this]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Method]] - [[What-is-Method|Methods]]
- [[What-is-Arrow]] - [[What-is-Arrow|Arrow functions]]
