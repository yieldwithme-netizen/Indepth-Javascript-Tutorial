# SOLID Principles

## Definition

SOLID is a set of **5 design principles** for maintainable code.

## Principles

```javascript
// Single Responsibility
function validateUser(user) { }
function saveUser(user) { }

// Open/Closed
class Shape { area() {} }
class Circle extends Shape { area() {} }

// Liskov Substitution
function getArea(shape) { return shape.area(); }

// Interface Segregation
class Printer { print() {} }
class Scanner { scan() {} }

// Dependency Inversion
class UserService {
    constructor(database) { this.db = database; }
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
- [[SOLID-Principles]] - [[SOLID-Principles|SOLID]]
- [[SOLID Principles]] - [[SOLID Principles|SOLID principles]]
- [[What-is-CleanCode]] - [[What-is-CleanCode|Clean code]]
