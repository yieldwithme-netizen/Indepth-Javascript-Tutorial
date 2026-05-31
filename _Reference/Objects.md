# Objects

## Definition

An object is a **collection of key-value pairs** (properties and methods).

## Creating Objects

```javascript
// Object literal
const person = { name: "John", age: 30 };

// Constructor
const person2 = new Object();
person2.name = "John";

// Object.create
const person3 = Object.create(null);
person3.name = "John";
```

## Accessing Properties

```javascript
const person = { name: "John", age: 30 };

// Dot notation
person.name; // "John"

// Bracket notation
person["name"]; // "John"

// Dynamic keys
const key = "name";
person[key]; // "John"
```

## Object Methods

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

## Quick Revision

- Object = key-value pairs
- Created with `{}`
- Access: dot or bracket notation
- Methods: functions in objects
- `this` refers to the object

---

## Related Topics

- [[What-is-Object]] - [[What-is-Object|Objects]] overview
- [[Create-Object]] - [[Create-Object|Creating objects]]
- [[Access-Properties]] - [[Access-Properties|Accessing properties]]
- [[Define-Methods]] - [[Define-Methods|Defining methods]]
- [[Object-Methods]] - [[Object-Methods|Object methods]]
