# Dependency Injection

## Definition

Dependency Injection provides **dependencies** to components from outside.

## Basic Example

```javascript
// Without DI (tightly coupled)
class UserService {
    constructor() {
        this.db = new Database();
    }
}

// With DI (loosely coupled)
class UserService {
    constructor(database) {
        this.db = database;
    }
}

const db = new Database();
const userService = new UserService(db);
```

## Quick Revision

- DI: provide dependencies externally
- Reduces coupling
- Improves testability
- Use constructor injection

---

## Related Topics

- [[Dependency-Injection]] - [[Dependency-Injection|Dependency injection]]
- [[Dependency Injection]] - [[Dependency Injection|Dependency injection]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Constructor]] - [[What-is-Constructor|Constructors]]
