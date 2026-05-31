# Factory Functions

## Definition

Factory functions are **functions that create and return objects**.

## Basic Syntax

```javascript
function createUser(name, age) {
    return {
        name,
        age,
        greet() {
            return `Hello, ${this.name}!`;
        }
    };
}

const user = createUser("John", 30);
user.greet(); // "Hello, John!"
```

## Factory vs Constructor

```javascript
// Factory function
function createPerson(name) {
    return { name };
}

// Constructor function
function Person(name) {
    this.name = name;
}

// ES6 class
class Person {
    constructor(name) {
        this.name = name;
    }
}
```

## Quick Revision

- Factory = function that creates objects
- No `new` keyword needed
- Can use closures for privacy
- More flexible than classes
- Use for: object creation, encapsulation

---

## Related Topics

- [[What-is-Factory]] - [[What-is-Factory|Factory functions]]
- [[Factory-Functions]] - [[Factory-Functions|Factory functions]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Constructor]] - [[What-is-Constructor|Constructors]]
