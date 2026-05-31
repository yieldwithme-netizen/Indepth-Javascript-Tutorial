# What are Private Fields?

Private fields are class properties that cannot be accessed or modified from outside the class. They provide true encapsulation by restricting direct access to internal state.

## Definition

Private fields in JavaScript:
- Are declared with `#` prefix
- Can only be accessed within the class definition
- Cannot be accessed by subclasses
- Throw an error when accessed from outside the class
- Are different from the `_` convention (which is just a naming convention, not enforcement)

## Syntax

```javascript
class ClassName {
  #privateField;

  constructor(value) {
    this.#privateField = value;
  }

  // Private methods also use #
  #privateMethod() {
    return this.#privateField;
  }
}
```

## Basic Example

```javascript
class User {
  #name;
  #email;

  constructor(name, email) {
    this.#name = name;
    this.#email = email;
  }

  get name() {
    return this.#name;
  }

  get email() {
    return this.#email;
  }

  #validateEmail(email) {
    return email.includes("@");
  }

  updateEmail(newEmail) {
    if (!this.#validateEmail(newEmail)) {
      throw new Error("Invalid email");
    }
    this.#email = newEmail;
  }
}

const user = new User("Alice", "alice@example.com");
console.log(user.name);  // Output: Alice (via getter)
console.log(user.email); // Output: alice@example.com (via getter)

// user.#name;       // SyntaxError: Private field
// user.#email;      // SyntaxError: Private field
// user.#validateEmail(); // SyntaxError: Private method
```

## Private Fields vs Properties

```javascript
class Person {
  // Public property (convention)
  _internalId = "abc123";

  // Private field (enforced)
  #secret;

  constructor(name) {
    this.name = name;
    this.#secret = Math.random();
  }

  getInfo() {
    return {
      name: this.name,
      id: this._internalId,  // Accessible
      secret: this.#secret   // Accessible within class
    };
  }
}

const person = new Person("Alice");
console.log(person._internalId); // Output: abc123 (accessible but convention says don't)
console.log(person.name);        // Output: Alice
// console.log(person.#secret);  // SyntaxError
```

## Private Methods

```javascript
class Calculator {
  #history = [];

  add(a, b) {
    const result = a + b;
    this.#log("add", a, b, result);
    return result;
  }

  subtract(a, b) {
    const result = a - b;
    this.#log("subtract", a, b, result);
    return result;
  }

  #log(operation, a, b, result) {
    this.#history.push({
      operation,
      inputs: [a, b],
      result,
      timestamp: new Date()
    });
  }

  getHistory() {
    return [...this.#history];
  }
}

const calc = new Calculator();
calc.add(2, 3);
calc.subtract(5, 2);
console.log(calc.getHistory());
// Output: [{ operation: 'add', inputs: [2, 3], result: 5, ... }, ...]
```

## Private Fields in Inheritance

```javascript
class Parent {
  #private = "parent";

  getPrivate() {
    return this.#private;
  }
}

class Child extends Parent {
  constructor() {
    super();
    // this.#private; // SyntaxError - cannot access parent's private fields
  }

  getParentPrivate() {
    return this.getPrivate(); // Must use public method
  }
}

const child = new Child();
console.log(child.getParentPrivate()); // Output: parent
```

## Common Use Cases

- Storing sensitive data (passwords, tokens)
- Internal state management
- Caching and memoization
- Validation logic
- Event handling internals
- Database connection strings
- API keys

## Common Mistakes

1. **Using `_` instead of `#` for "private"**
   ```javascript
   // Wrong - just a convention, not enforced
   class Bad {
     constructor() {
       this._secret = "hidden"; // Accessible externally
     }
   }

   // Correct - truly private
   class Good {
     #secret;
     constructor() {
       this.#secret = "hidden";
     }
   }
   ```

2. **Trying to access private fields outside the class**
   ```javascript
   class User {
     #password;
     constructor(pw) { this.#password = pw; }
   }

   const user = new User("secret");
   // user.#password; // SyntaxError
   ```

3. **Assuming private fields are inherited**
   ```javascript
   class Parent {
     #data = "private";
   }

   class Child extends Parent {
     getData() {
       // return this.#data; // SyntaxError - doesn't exist in Child
     }
   }
   ```

4. **Using private fields with `Object.defineProperty`**
   ```javascript
   // This doesn't work with private fields
   class User {
     #name;
     constructor(name) { this.#name = name; }
   }

   // Can't dynamically add private fields
   ```

## Private vs Convention

```javascript
// Convention (not enforced)
class Convention {
  constructor() {
    this._data = "accessible";
    this.__alsoData = "also accessible";
  }
}

// Private fields (enforced)
class Private {
  #data;
  constructor() {
    this.#data = "truly hidden";
  }
}
```

## Related Topics

- [[What-is-Constructor]]
- [[What-is-GetSet]]
- [[Use-GetSet]]
- [[Use-Extends]]
- [[What-is-Static]]

## Quick Revision

| Feature | Private Field | Convention |
|---------|---------------|------------|
| Syntax | `#field` | `_field` |
| Enforcement | Runtime error | Developer trust |
| Outside access | Not allowed | Allowed (bad practice) |
| Inheritance | Not inherited | Inherited |
| Reflection | Not accessible | Accessible |
| Use case | Sensitive data | Internal use hint |
