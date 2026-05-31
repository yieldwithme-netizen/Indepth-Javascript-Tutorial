# OOP (Object-Oriented Programming)

## Definition

OOP is a **programming paradigm** based on objects containing data and methods.

## Four Pillars

### 1. Encapsulation

```javascript
class User {
    #password;
    
    constructor(name, password) {
        this.name = name;
        this.#password = password;
    }
}
```

### 2. Abstraction

```javascript
class Calculator {
    add(a, b) { return a + b; } // simplified interface
    // complex implementation hidden
}
```

### 3. Inheritance

```javascript
class Animal {}
class Dog extends Animal {}
```

### 4. Polymorphism

```javascript
class Shape { area() {} }
class Circle extends Shape { area() { return Math.PI * this.r ** 2; } }
class Square extends Shape { area() { return this.s ** 2; } }
```

## Quick Revision

- OOP = objects + methods
- Encapsulation: data hiding
- Abstraction: simplified interface
- Inheritance: code reuse
- Polymorphism: same interface, different behavior

---

## Related Topics

- [[What-is-OOP]] - [[What-is-OOP|OOP]]
- [[OOP]] - [[OOP|OOP]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Encapsulation]] - [[What-is-Encapsulation|Encapsulation]]
- [[What-is-Inheritance]] - [[What-is-Inheritance|Inheritance]]
