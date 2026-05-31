# Dependency Injection

## Definition

Dependency Injection (DI) **provides dependencies** to a component from outside.

## Basic Example

```javascript
// Without DI
class UserService {
    constructor() {
        this.db = new Database(); // tightly coupled
    }
}

// With DI
class UserService {
    constructor(database) {
        this.db = database; // dependency injected
    }
}

// Usage
const db = new Database();
const userService = new UserService(db);
```

## Quick Revision

- DI: provide dependencies from outside
- Reduces coupling
- Improves testability
- Use: constructor injection, setter injection

---

## Related Topics

- [[Dependency-Injection]] - [[Dependency-Injection|Dependency injection]]
- [[Dependency Injection]] - [[Dependency Injection|Dependency injection]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Constructor]] - [[What-is-Constructor|Constructors]]
