# What is an [[Object]]?

## Definition

An [[object]] is a **collection of key-value pairs** ([[property|properties]] and [[method|methods]]).

## Creating Objects

```javascript
// Object literal (preferred)
const person = {
    name: "John",
    age: 30,
    greet() {
        return `Hello, ${this.name}!`;
    }
};

// Constructor
const person = new Object();
person.name = "John";

// Object.create
const person = Object.create(null);
person.name = "John";
```

## Accessing [[Property|Properties]]

```javascript
const person = { name: "John", age: 30 };

// Dot notation
person.name; // "John"

// Bracket notation
person["name"]; // "John"
person["age"]; // 30

// Dynamic keys
const key = "name";
person[key]; // "John"
```

## Adding/Modifying [[Property|Properties]]

```javascript
const person = { name: "John" };

// Add
person.age = 30;
person["email"] = "john@example.com";

// Modify
person.name = "Jane";

// Delete
delete person.email;
```

## [[Object]] [[Method|Methods]]

```javascript
const person = {
    name: "John",
    greet() {
        return `Hello, ${this.name}!`;
    }
};

person.greet(); // "Hello, John!"
```

## Quick Revision

- [[Object]] = key-value pairs
- Created with `{}`
- Access: dot (`obj.key`) or bracket (`obj["key"]`)
- [[Method|Methods]]: functions in objects
- `this` refers to the [[object]]

---

## Related Topics

- [[Create-Object]] - Creating objects
- [[Access-Properties]] - Accessing properties
- [[Define-Methods]] - Methods
- [[What-is-Property]] - Properties
- [[What-is-Method]] - Methods
