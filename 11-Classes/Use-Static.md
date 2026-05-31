# How to Use Static Methods

## Basic Syntax

```javascript
class MathUtils {
    static add(a, b) {
        return a + b;
    }
    
    static multiply(a, b) {
        return a * b;
    }
}

MathUtils.add(5, 3); // 8
MathUtils.multiply(5, 3); // 15
```

## Calling Static Methods

```javascript
class User {
    static count = 0;
    
    constructor(name) {
        this.name = name;
        User.count++;
    }
    
    static getCount() {
        return User.count;
    }
}

const user1 = new User("John");
const user2 = new User("Jane");

console.log(User.getCount()); // 2
console.log(user1.getCount()); // ❌ TypeError!
```

## Static vs Instance

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    // Instance method
    speak() {
        return `${this.name} makes a noise`;
    }
    
    // Static method
    static create(name) {
        return new Animal(name);
    }
}

const animal = Animal.create("Rex"); // ✅ Static
animal.speak(); // ✅ Instance
Animal.speak(); // ❌ TypeError!
```

## When to Use

```javascript
// ✅ Use static for:
// - Utility functions
// - Factory methods
// - Class-level operations

// ❌ Don't use static for:
// - Instance-specific behavior
// - Methods that need `this`
```

## Quick Revision

- Static methods: `static methodName()`
- Called on class, not instance
- No access to `this` (instance)
- Use for: utilities, factories, class operations
- `static` keyword before method

---

## Related Topics

- [[What-is-Static]] - [[What-is-Static|Static methods]] overview
- [[Use-Static]] - [[Use-Static|Using static methods]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Constructor]] - [[What-is-Constructor|Constructors]]
- [[What-is-GetSet]] - [[What-is-GetSet|Getters/setters]]
