# OOP Concepts

## Definition

OOP concepts are **fundamental principles** of object-oriented programming.

## Four Pillars

```javascript
// 1. Encapsulation
class User {
    #password;
    constructor(name, password) {
        this.name = name;
        this.#password = password;
    }
}

// 2. Abstraction
class Calculator {
    add(a, b) { return a + b; }
}

// 3. Inheritance
class Animal {}
class Dog extends Animal {}

// 4. Polymorphism
class Shape { area() {} }
class Circle extends Shape { area() { return Math.PI * this.r ** 2; } }
```

## Quick Revision

- Encapsulation: data hiding
- Abstraction: simplified interface
- Inheritance: code reuse
- Polymorphism: same interface, different behavior

---

## Related Topics

- [[What-is-OOP]] - [[What-is-OOP|OOP]]
- [[OOP-Concepts]] - [[OOP-Concepts|OOP concepts]]
- [[OOP]] - [[OOP|OOP]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
