# Methods

## Definition

A method is a function that is associated with an object. Methods define the behavior of objects and can access and modify the object's data. In JavaScript, methods can be defined in objects, classes, prototypes, and even as standalone functions that operate on data.

## Code Examples

### Object Methods

```javascript
const calculator = {
  result: 0,

  add(n) {
    this.result += n;
    return this; // Enable chaining
  },

  subtract(n) {
    this.result -= n;
    return this;
  },

  reset() {
    this.result = 0;
    return this;
  },

  getResult() {
    return this.result;
  },
};

const value = calculator.add(10).subtract(3).getResult();
console.log(value); // 7
```

### Class Methods

```javascript
class BankAccount {
  #balance;
  #owner;

  constructor(owner, balance = 0) {
    this.#owner = owner;
    this.#balance = balance;
  }

  // Instance method
  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return this;
    }
    throw new Error("Invalid deposit amount");
  }

  // Instance method
  withdraw(amount) {
    if (amount > 0 && amount <= this.#balance) {
      this.#balance -= amount;
      return this;
    }
    throw new Error("Insufficient funds");
  }

  // Getter method
  get balance() {
    return this.#balance;
  }

  // Static method
  static transfer(from, to, amount) {
    from.withdraw(amount);
    to.deposit(amount);
  }
}

const alice = new BankAccount("Alice", 1000);
const bob = new BankAccount("Bob", 500);

BankAccount.transfer(alice, bob, 200);
console.log(alice.balance); // 800
console.log(bob.balance);   // 700
```

### Prototype Methods

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}

User.prototype.greet = function () {
  return `Hello, I'm ${this.name}`;
};

User.prototype.updateEmail = function (newEmail) {
  this.email = newEmail;
  return this;
};

const user = new User("Alice", "alice@example.com");
console.log(user.greet()); // "Hello, I'm Alice"
user.updateEmail("new@example.com");
```

### Method Chaining

```javascript
class QueryBuilder {
  #table;
  #conditions;
  #orderBy;
  #limit;

  constructor(table) {
    this.#table = table;
    this.#conditions = [];
    this.#orderBy = null;
    this.#limit = null;
  }

  where(condition) {
    this.#conditions.push(condition);
    return this;
  }

  orderBy(field, direction = "ASC") {
    this.#orderBy = `${field} ${direction}`;
    return this;
  }

  take(n) {
    this.#limit = n;
    return this;
  }

  build() {
    let query = `SELECT * FROM ${this.#table}`;

    if (this.#conditions.length) {
      query += ` WHERE ${this.#conditions.join(" AND ")}`;
    }
    if (this.#orderBy) {
      query += ` ORDER BY ${this.#orderBy}`;
    }
    if (this.#limit) {
      query += ` LIMIT ${this.#limit}`;
    }

    return query;
  }
}

const query = new QueryBuilder("users")
  .where("age > 18")
  .where("status = 'active'")
  .orderBy("name")
  .take(10)
  .build();

console.log(query);
// "SELECT * FROM users WHERE age > 18 AND status = 'active' ORDER BY name ASC LIMIT 10"
```

### Array Methods (Built-in)

```javascript
const numbers = [1, 2, 3, 4, 5];

// map - transform each element
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - keep elements matching condition
const evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4]

// reduce - accumulate into single value
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15

// find - get first matching element
const found = numbers.find((n) => n > 3);
console.log(found); // 4

// some - check if any element matches
const hasBig = numbers.some((n) => n > 4);
console.log(hasBig); // true

// every - check if all elements match
const allPositive = numbers.every((n) => n > 0);
console.log(allPositive); // true
```

### Getter and Setter Methods

```javascript
class Temperature {
  #celsius;

  constructor(celsius) {
    this.#celsius = celsius;
  }

  get fahrenheit() {
    return this.#celsius * 1.8 + 32;
  }

  set fahrenheit(f) {
    this.#celsius = (f - 32) / 1.8;
  }

  get display() {
    return `${this.#celsius}°C / ${this.fahrenheit}°F`;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // 212
temp.fahrenheit = 32;
console.log(temp.celsius);    // 0
```

## Common Use Cases

- **Data encapsulation** — Bundle data with behavior
- **API design** — Create fluent interfaces with chaining
- **Utility functions** — Organize reusable operations
- **State management** — Modify object state safely
- **Data transformation** — Process collections with array methods

## Common Mistakes

```javascript
// Mistake 1: Losing 'this' in callbacks
class Timer {
  constructor() {
    this.seconds = 0;
  }

  start() {
    // Wrong: 'this' is lost in setInterval
    // setInterval(function() {
    //   this.seconds++;
    // }, 1000);

    // Correct: arrow function preserves 'this'
    setInterval(() => {
      this.seconds++;
    }, 1000);
  }
}

// Mistake 2: Using arrow functions for object methods
const obj = {
  value: 10,
  // Wrong: arrow function doesn't have own 'this'
  getValue: () => this.value,
  // Correct: method shorthand
  getValue() {
    return this.value;
  },
};

// Mistake 3: Not returning 'this' for chaining
class Builder {
  #data = [];

  addItem(item) {
    this.#data.push(item);
    // Missing return this; - breaks chaining
    return this;
  }
}

// Mistake 4: Confusing methods with properties
const user = {
  name: "Alice",
  greet() {
    return `Hello, ${this.name}`;
  },
};

console.log(user.greet);  // [Function: greet]
console.log(user.greet()); // "Hello, Alice"
```

## Related Topics

- [[Define-Objects]]
- [[Classes]]
- [[This-Keyword]]
- [[Arrow-Functions]]
- [[Prototype-Chain]]
- [[Array-Methods]]
- [[Getters-Setters]]

## Quick Revision

| Type | Syntax | Context |
|------|--------|---------|
| Object method | `obj.method()` | `this` is `obj` |
| Class method | `class.method()` | `this` is instance |
| Static method | `Class.method()` | `this` is class |
| Prototype | `obj.__proto__.method()` | `this` is `obj` |
| Arrow | `() => {}` | Inherits `this` |
| Getter | `get prop()` | Called like property |
| Setter | `set prop(v)` | Called on assignment |

| Pattern | Description |
|---------|-------------|
| Method chaining | Return `this` from methods |
| Builder pattern | Chain method calls to construct |
| Fluent API | Readable method chaining |
