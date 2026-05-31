# What is a Constructor?

A **constructor** is a special method in a JavaScript class that is automatically called when a new instance of the class is created. It is used to initialize the object's properties with default or user-provided values.

## Definition

In JavaScript, the `constructor()` method is a built-in method that:
- Is called once when the object is instantiated with the `new` keyword
- Initializes the properties of the new object
- Can accept parameters to configure the object
- Must only be defined once per class (a class cannot have multiple constructors)

## Syntax

```javascript
class ClassName {
  constructor(parameter1, parameter2) {
    this.parameter1 = parameter1;
    this.parameter2 = parameter2;
  }
}

const instance = new ClassName(value1, value2);
```

## Code Example

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const person1 = new Person("Alice", 30);
console.log(person1.name); // Output: Alice
console.log(person1.age);  // Output: 30
```

## Constructor with Default Values

Constructors can provide default values for parameters:

```javascript
class User {
  constructor(name = "Guest", role = "viewer") {
    this.name = name;
    this.role = role;
  }
}

const user1 = new User();
console.log(user1.name); // Output: Guest
console.log(user1.role); // Output: viewer

const user2 = new User("Admin", "admin");
console.log(user2.name); // Output: Admin
console.log(user2.role); // Output: admin
```

## Constructor with Computed Properties

Constructors can compute values based on parameters:

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
    this.area = width * height;
  }
}

const rect = new Rectangle(5, 10);
console.log(rect.area); // Output: 50
```

## Common Use Cases

- Initializing object state
- Setting default property values
- Validating input data before assignment
- Creating computed properties
- Connecting to databases or external services
- Setting up event listeners

## Common Mistakes

1. **Forgetting to use `this` keyword**
   ```javascript
   // Wrong
   class User {
     constructor(name) {
       name = name; // This creates a local variable, not a property
     }
   }

   // Correct
   class User {
     constructor(name) {
       this.name = name;
     }
   }
   ```

2. **Defining multiple constructors** — JavaScript only allows one constructor per class

3. **Returning an object from constructor** — This overrides the `new` operator behavior

4. **Using arrow functions for constructors** — Constructors must be regular functions

## Related Topics

- [[Use-Constructor]]
- [[What-is-Super]]
- [[Call-Parent]]
- [[What-is-Static]]
- [[What-is-Private]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Purpose | Initialize new object instances |
| Syntax | `constructor() { ... }` |
| Called | Automatically with `new` keyword |
| Parameters | Optional, used to set initial values |
| `this` | Refers to the new object being created |
| Limitation | Only one constructor per class |
| Default params | `constructor(name = "default")` |
