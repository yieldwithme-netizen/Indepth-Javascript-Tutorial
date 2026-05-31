# What are Getters and Setters?

Getters and setters are special methods in JavaScript classes that allow you to define custom behavior when reading or writing to properties. They provide a way to intercept property access and modification.

## Definition

- **Getter**: A method defined with `get` keyword that is called when you read a property
- **Setter**: A method defined with `set` keyword that is called when you assign a value to a property
- They allow you to add validation, computation, or side effects to property access

## Syntax

```javascript
class ClassName {
  constructor(value) {
    this._value = value;
  }

  get propertyName() {
    // Return computed value or transform
    return this._value;
  }

  set propertyName(newValue) {
    // Validate or transform before setting
    this._value = newValue;
  }
}
```

## Basic Example

```javascript
class Temperature {
  constructor(celsius) {
    this.celsius = celsius; // Uses setter
  }

  get fahrenheit() {
    return (this.celsius * 9/5) + 32;
  }

  set fahrenheit(f) {
    this.celsius = (f - 32) * 5/9;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // Output: 212

temp.fahrenheit = 32;
console.log(temp.celsius);    // Output: 0
```

## Getters for Computed Properties

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  get area() {
    return this.width * this.height;
  }

  get perimeter() {
    return 2 * (this.width + this.height);
  }

  get isSquare() {
    return this.width === this.height;
  }
}

const rect = new Rectangle(5, 10);
console.log(rect.area);      // Output: 50
console.log(rect.perimeter); // Output: 30
console.log(rect.isSquare);  // Output: false
```

## Setters with Validation

```javascript
class BankAccount {
  constructor(owner, balance = 0) {
    this.owner = owner;
    this._balance = balance;
  }

  get balance() {
    return this._balance;
  }

  set balance(value) {
    if (value < 0) {
      throw new Error("Balance cannot be negative");
    }
    this._balance = value;
  }
}

const account = new BankAccount("Alice", 1000);
account.balance = 500;   // Valid
console.log(account.balance); // Output: 500

// account.balance = -100; // Error: Balance cannot be negative
```

## Getters/Setters for Data Transformation

```javascript
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  set fullName(name) {
    const [first, ...rest] = name.split(" ");
    this.firstName = first;
    this.lastName = rest.join(" ");
  }
}

const user = new User("John", "Doe");
console.log(user.fullName); // Output: John Doe

user.fullName = "Jane Smith";
console.log(user.firstName); // Output: Jane
console.log(user.lastName);  // Output: Smith
```

## Getters/Setters with Private Fields

```javascript
class SecureData {
  #data;

  constructor(data) {
    this.#data = data;
  }

  get data() {
    return this.#data;
  }

  set data(value) {
    if (typeof value !== "string") {
      throw new TypeError("Data must be a string");
    }
    this.#data = value;
  }
}
```

## Chaining Getters and Setters

```javascript
class Pipeline {
  constructor() {
    this._value = 0;
    this._transformations = [];
  }

  get value() {
    return this._value;
  }

  set value(v) {
    this._value = v;
  }

  add(n) {
    this._value += n;
    return this; // Enable chaining
  }

  multiply(n) {
    this._value *= n;
    return this;
  }
}

const p = new Pipeline();
p.add(5).multiply(2).add(10);
console.log(p.value); // Output: 20
```

## Common Use Cases

- Data validation and sanitization
- Computed/derived properties
- Data transformation (e.g., unit conversion)
- Access control (read-only, write-only)
- Logging and debugging
- Proxy-like behavior
- Encapsulation of complex logic

## Common Mistakes

1. **Using getter that triggers setter recursion**
   ```javascript
   // Wrong
   class Bad {
     get name() {
       return this.name; // Infinite recursion!
     }
   }

   // Better
   class Good {
     get name() {
       return this._name; // Use backing field
     }
   }
   ```

2. **Defining getter and setter with different names**
   ```javascript
   // Wrong
   class Bad {
     get name() { return this._name; }
     set fullName(v) { this._name = v; } // Different name!
   }
   ```

3. **Forgetting getter/setter syntax in objects**
   ```javascript
   // Wrong
   const obj = {
     get value() { return 10; },
     set value(v) { /* ok */ }
   };

   // The get/set keywords are correct
   ```

4. **Using arrow functions for getters/setters**
   ```javascript
   // Wrong
   class Bad {
     get name = () => { return this._name; } // SyntaxError
   }
   ```

## Related Topics

- [[Use-GetSet]]
- [[What-is-Constructor]]
- [[What-is-Static]]
- [[What-is-Private]]
- [[What-is-Chaining]]

## Quick Revision

| Feature | Getter | Setter |
|---------|--------|--------|
| Syntax | `get prop()` | `set prop(v)` |
| Triggered | On read | On assignment |
| Returns | Computed value | Nothing (undefined) |
| Parameters | None | One (the value) |
| Can be read-only | Yes (omit setter) | N/A |
| Can be write-only | N/A | Yes (omit getter) |
| Validation | No | Yes |
