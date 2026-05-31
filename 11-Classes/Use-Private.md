# How to Use Private Fields

## Basic Syntax

```javascript
class Person {
    #name;
    #age;
    
    constructor(name, age) {
        this.#name = name;
        this.#age = age;
    }
    
    getName() {
        return this.#name;
    }
    
    getAge() {
        return this.#age;
    }
}
```

## Accessing Private Fields

```javascript
const person = new Person("John", 30);

console.log(person.getName()); // "John"
console.log(person.#name); // ❌ SyntaxError!
console.log(person.age); // undefined (not accessible)
```

## Private Methods

```javascript
class Calculator {
    #validate(number) {
        return typeof number === "number";
    }
    
    add(a, b) {
        if (!this.#validate(a) || !this.#validate(b)) {
            throw new Error("Invalid input");
        }
        return a + b;
    }
}
```

## Private vs Public

```javascript
class User {
    // Public field
    name;
    
    // Private field
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

## Quick Revision

- Private fields: `#fieldName`
- Private methods: `#methodName()`
- Cannot access outside class
- Use for encapsulation
- Prefix with `#` to make private

---

## Related Topics

- [[What-is-Private]] - [[What-is-Private|Private fields]] overview
- [[Use-Private]] - [[Use-Private|Using private fields]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Encapsulation]] - [[What-is-Encapsulation|Encapsulation]]
- [[What-is-GetSet]] - [[What-is-GetSet|Getters/setters]]
