# How to Use Getters and Setters

This guide covers practical patterns for using getters and setters in JavaScript classes, including validation, computed properties, and real-world examples.

## Basic Usage

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
```

## Validation with Setters

```javascript
class Person {
  #name;
  #age;

  constructor(name, age) {
    this.name = name;  // Uses setter
    this.age = age;    // Uses setter
  }

  get name() {
    return this.#name;
  }

  set name(value) {
    if (typeof value !== "string" || value.length === 0) {
      throw new Error("Name must be a non-empty string");
    }
    this.#name = value.trim();
  }

  get age() {
    return this.#age;
  }

  set age(value) {
    if (typeof value !== "number" || value < 0 || value > 150) {
      throw new Error("Age must be a number between 0 and 150");
    }
    this.#age = Math.floor(value);
  }
}

const person = new Person("Alice", 30);
// person.age = -5;  // Error: Age must be a number between 0 and 150
// person.name = ""; // Error: Name must be a non-empty string
```

## Computed Properties

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

  get diagonal() {
    return Math.sqrt(this.width ** 2 + this.height ** 2);
  }
}

const rect = new Rectangle(3, 4);
console.log(rect.area);      // Output: 12
console.log(rect.perimeter); // Output: 14
console.log(rect.diagonal);  // Output: 5
```

## Unit Conversion with Getters/Setters

```javascript
class Temperature {
  #celsius;

  constructor(celsius) {
    this.#celsius = celsius;
  }

  get celsius() {
    return this.#celsius;
  }

  set celsius(c) {
    this.#celsius = c;
  }

  get fahrenheit() {
    return (this.#celsius * 9/5) + 32;
  }

  set fahrenheit(f) {
    this.#celsius = (f - 32) * 5/9;
  }

  get kelvin() {
    return this.#celsius + 273.15;
  }

  set kelvin(k) {
    this.#celsius = k - 273.15;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // Output: 212
console.log(temp.kelvin);     // Output: 373.15

temp.fahrenheit = 32;
console.log(temp.celsius);    // Output: 0
```

## Read-Only Properties

```javascript
class BankAccount {
  #balance;
  #owner;
  #transactions = [];

  constructor(owner, initialBalance = 0) {
    this.#owner = owner;
    this.#balance = initialBalance;
  }

  get owner() {
    return this.#owner;
  }

  get balance() {
    return this.#balance;
  }

  // No setter for balance - use methods to modify
  deposit(amount) {
    if (amount <= 0) throw new Error("Deposit must be positive");
    this.#balance += amount;
    this.#transactions.push({ type: "deposit", amount, date: new Date() });
  }

  withdraw(amount) {
    if (amount <= 0) throw new Error("Withdrawal must be positive");
    if (amount > this.#balance) throw new Error("Insufficient funds");
    this.#balance -= amount;
    this.#transactions.push({ type: "withdrawal", amount, date: new Date() });
  }

  get transactions() {
    return [...this.#transactions]; // Return copy
  }
}
```

## Chaining with Getters/Setters

```javascript
class QueryBuilder {
  #table;
  #conditions = [];
  #orderBy = null;
  #limit = null;

  constructor(table) {
    this.#table = table;
  }

  get query() {
    let sql = `SELECT * FROM ${this.#table}`;
    if (this.#conditions.length > 0) {
      sql += ` WHERE ${this.#conditions.join(" AND ")}`;
    }
    if (this.#orderBy) {
      sql += ` ORDER BY ${this.#orderBy}`;
    }
    if (this.#limit) {
      sql += ` LIMIT ${this.#limit}`;
    }
    return sql;
  }

  where(condition) {
    this.#conditions.push(condition);
    return this; // Enable chaining
  }

  orderBy(field) {
    this.#orderBy = field;
    return this;
  }

  limit(n) {
    this.#limit = n;
    return this;
  }
}

const q = new QueryBuilder("users")
  .where("age > 18")
  .where("active = true")
  .orderBy("name ASC")
  .limit(10);

console.log(q.query);
// Output: SELECT * FROM users WHERE age > 18 AND active = true ORDER BY name ASC LIMIT 10
```

## Common Use Cases

- Form validation
- Currency/price formatting
- Date formatting
- Data transformation
- API response normalization
- Configuration management
- Encapsulation with validation

## Common Mistakes

1. **Triggering infinite recursion**
   ```javascript
   // Wrong
   class Bad {
     get name() {
       return this.name; // Infinite loop!
     }
   }

   // Correct
   class Good {
     get name() {
       return this._name;
     }
   }
   ```

2. **Inconsistent getter/setter types**
   ```javascript
   // Wrong
   class Bad {
     get value() {
       return this._value.toString(); // Returns string
     }
     set value(v) {
       this._value = Number(v); // Expects number
     }
   }
   ```

3. **Forgetting setters are void**
   ```javascript
   // Wrong
   set name(v) {
     return this._name = v; // Misleading - returns value but setter ignores it
   }
   ```

4. **Using arrow functions**
   ```javascript
   // Wrong
   class Bad {
     get name = () => this._name; // SyntaxError
   }
   ```

## Related Topics

- [[What-is-GetSet]]
- [[What-is-Private]]
- [[What-is-Constructor]]
- [[What-is-Static]]
- [[What-is-Chaining]]

## Quick Revision

| Pattern | Example |
|---------|---------|
| Basic getter | `get name() { return this._name; }` |
| Basic setter | `set name(v) { this._name = v; }` |
| Computed | `get area() { return w * h; }` |
| Validation | `set age(v) { if (v < 0) throw Error; }` |
| Read-only | Omit the setter method |
| Write-only | Omit the getter method |
| Data transform | `set fullName(n) { [first, last] = n.split(" "); }` |
