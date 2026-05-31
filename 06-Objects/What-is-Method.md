# What is a Method?

## Definition

A method is a **[[function]] stored as an [[object]] [[property]]**.

## Creating Methods

```javascript
const calculator = {
    add(a, b) {
        return a + b;
    },
    subtract(a, b) {
        return a - b;
    }
};

calculator.add(5, 3); // 8
```

## [[this]] in Methods

```javascript
const person = {
    name: "John",
    greet() {
        return `Hello, ${this.name}!`;
    }
};

person.greet(); // "Hello, John!"
```

## Method Chaining

```javascript
const calculator = {
    value: 0,
    add(n) {
        this.value += n;
        return this; // enables chaining
    },
    subtract(n) {
        this.value -= n;
        return this;
    },
    result() {
        return this.value;
    }
};

const result = calculator.add(5).add(3).subtract(2).result();
console.log(result); // 6
```

## Arrow Functions and [[this]]

```javascript
const person = {
    name: "John",
    greet: () => {
        // Arrow function doesn't have its own this!
        return `Hello, ${this.name}!`; // undefined!
    }
};

// Fix: Use regular function
const person = {
    name: "John",
    greet() {
        return `Hello, ${this.name}!`;
    }
};
```

## Quick Revision

- Method = [[function]] in [[object]]
- `this` refers to the [[object]]
- Use regular functions for methods (not arrows)
- Return `this` for method chaining
- Methods can call other methods

---

## Related Topics

- [[What-is-Object]] - Objects overview
- [[Define-Methods]] - Defining methods
- [[What-is-This]] - this keyword
- [[What-is-Chaining]] - Method chaining
- [[What-is-Property]] - Properties
