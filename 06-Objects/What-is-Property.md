# What is a [[Property]]?

## Definition

A [[property]] is a **key-value pair** in an [[object]].

## Syntax

```javascript
const person = {
    name: "John",   // property
    age: 30,        // property
    "email": "j@x.com" // computed key
};
```

## [[Property]] Access

```javascript
const obj = { "my-key": "value" };

// Dot notation (simple keys only)
obj.myKey; // undefined!

// Bracket notation (any key)
obj["my-key"]; // "value"
```

## [[Property]] Shorthand

```javascript
const name = "John";
const age = 30;

// Longhand
const person = { name: name, age: age };

// Shorthand (ES6)
const person = { name, age };
```

## Computed [[Property]] Names

```javascript
const prop = "name";
const obj = {
    [prop]: "John",
    ["age" + 1]: 31
};
console.log(obj); // { name: "John", age1: 31 }
```

## [[Property]] Descriptors

```javascript
const obj = {};

Object.defineProperty(obj, "name", {
    value: "John",
    writable: false,     // can't change
    enumerable: true,    // shows in loop
    configurable: false  // can't delete
});

obj.name = "Jane"; // Error (writable: false)
```

## Quick Revision

- [[Property]] = key-value pair in [[object]]
- Access: dot or bracket notation
- Bracket for dynamic/computed keys
- Shorthand: `{ name, age }` (ES6)
- Descriptors control [[property]] behavior

---

## Related Topics

- [[What-is-Object]] - Objects overview
- [[Access-Properties]] - Accessing properties
- [[What-is-Method]] - Methods
- [[What-is-GetSet]] - Getters/setters
- [[What-is-Private]] - Private properties
