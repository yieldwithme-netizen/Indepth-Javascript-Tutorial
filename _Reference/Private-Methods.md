# Private Methods

## Definition

Private methods are **methods only accessible within a class** using the `#` prefix.

## Basic Syntax

```javascript
class User {
    #password;
    
    constructor(name, password) {
        this.name = name;
        this.#password = password;
    }
    
    // Public method
    getName() {
        return this.name;
    }
    
    // Private method
    #validatePassword(password) {
        return password.length >= 8;
    }
    
    // Public method using private
    changePassword(newPassword) {
        if (this.#validatePassword(newPassword)) {
            this.#password = newPassword;
        }
    }
}
```

## Private vs Public

```javascript
class Calculator {
    // Public field
    display;
    
    // Private field
    #history = [];
    
    constructor() {
        this.display = 0;
    }
    
    // Public method
    add(a, b) {
        const result = a + b;
        this.#logOperation('add', a, b, result);
        return result;
    }
    
    // Private method
    #logOperation(op, a, b, result) {
        this.#history.push({ op, a, b, result, time: Date.now() });
    }
    
    // Public method to access private data
    getHistory() {
        return [...this.#history];
    }
}
```

## Quick Revision

- Private methods: `#methodName()`
- Cannot access outside class
- Use for encapsulation
- Private fields: `#fieldName`
- Prefix with `#` for private

---

## Related Topics

- [[What-is-Private]] - [[What-is-Private|Private fields]]
- [[Use-Private]] - [[Use-Private|Using private]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Encapsulation]] - [[What-is-Encapsulation|Encapsulation]]
