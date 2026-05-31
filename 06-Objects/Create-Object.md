# How to Create Objects

## Object Literal (Preferred)

```javascript
// Empty object
const obj = {};

// With properties
const person = {
    name: "John",
    age: 30,
    city: "New York"
};

// With methods
const calculator = {
    add(a, b) {
        return a + b;
    },
    subtract(a, b) {
        return a - b;
    }
};
```

## Constructor

```javascript
const person = new Object();
person.name = "John";
person.age = 30;
```

## Object.create

```javascript
const proto = {
    greet() {
        return `Hello, ${this.name}!`;
    }
};

const person = Object.create(proto);
person.name = "John";
```

## Shorthand Properties

```javascript
const name = "John";
const age = 30;

// Longhand
const person = { name: name, age: age };

// Shorthand (ES6)
const person = { name, age };
```

## Quick Revision

- Object literal: `{}` (preferred)
- Constructor: `new Object()`
- `Object.create()` for inheritance
- Shorthand: `{ name, age }` (ES6)
- Properties: key-value pairs

---

## Related Topics

- [[What-is-Object]] - [[What-is-Object|Objects]] overview
- [[Create-Object]] - [[Create-Object|Creating objects]]
- [[Access-Properties]] - [[Access-Properties|Accessing properties]]
- [[Define-Methods]] - [[Define-Methods|Defining methods]]
