# SOLID Principles

## Definition

SOLID is a set of **5 design principles** for writing maintainable code.

## 1. Single Responsibility

```javascript
// ❌ Bad
function processUser(user) { }

// ✅ Good
function validateUser(user) { }
function saveUser(user) { }
```

## 2. Open/Closed

```javascript
// Open for extension, closed for modification
class Shape {
    area() { throw new Error('Not implemented'); }
}

class Circle extends Shape {
    area() { return Math.PI * this.r ** 2; }
}
```

## 3. Liskov Substitution

```javascript
// Subtypes must be substitutable for base types
function getArea(shape) {
    return shape.area(); // works for any Shape
}
```

## 4. Interface Segregation

```javascript
// Many specific interfaces over one general
class Printer { print() { } }
class Scanner { scan() { } }
class Copier { print() { this.scan(); } }
```

## 5. Dependency Inversion

```javascript
// Depend on abstractions, not concretions
class UserService {
    constructor(database) {
        this.database = database; // injected
    }
}
```

## Quick Revision

- **S**ingle Responsibility: one job
- **O**pen/Closed: extend, don't modify
- **L**iskov Substitution: subtypes work
- **I**nterface Segregation: small interfaces
- **D**ependency Inversion: depend on abstractions

---

## Related Topics

- [[What-is-SOLID]] - [[What-is-SOLID|SOLID]]
- [[SOLID Principles]] - [[SOLID Principles|SOLID principles]]
- [[What-is-CleanCode]] - [[What-is-CleanCode|Clean code]]
- [[What-is-DRY]] - [[What-is-DRY|DRY]]
