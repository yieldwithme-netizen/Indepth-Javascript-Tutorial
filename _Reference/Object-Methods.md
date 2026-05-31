# Object Methods Overview

## Definition

Object methods are functions that are stored as object properties. They provide behavior associated with an object and can access and manipulate the object's data through the `this` keyword.

## Creating Object Methods

### Object Literal

```javascript
const calculator = {
  value: 0,

  add(num) {
    this.value += num;
    return this; // Enable chaining
  },

  subtract(num) {
    this.value -= num;
    return this;
  },

  getResult() {
    return this.value;
  }
};

calculator.add(5).subtract(2);
console.log(calculator.getResult()); // 3
```

### Constructor Function

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const john = new Person('John');
console.log(john.greet()); // "Hello, I'm John"
```

### Class Syntax

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
    return `${this.name} says Woof!`;
  }

  fetch(item) {
    return `${this.name} fetches the ${item}`;
  }
}

const rex = new Dog('Rex');
console.log(rex.bark()); // "Rex says Woof!"
```

## Built-in Object Methods

### `Object.keys()` - Get Property Names

```javascript
const user = { name: 'John', age: 30, email: 'john@example.com' };

const keys = Object.keys(user);
console.log(keys); // ["name", "age", "email"]
```

### `Object.values()` - Get Property Values

```javascript
const user = { name: 'John', age: 30, email: 'john@example.com' };

const values = Object.values(user);
console.log(values); // ["John", 30, "john@example.com"]
```

### `Object.entries()` - Get Key-Value Pairs

```javascript
const user = { name: 'John', age: 30 };

const entries = Object.entries(user);
console.log(entries);
// [["name", "John"], ["age", 30]]

// Useful for iteration
entries.forEach(([key, value]) => {
  console.log(`${key}: ${value}`);
});
```

### `Object.assign()` - Copy/Merge Objects

```javascript
const defaults = { color: 'red', size: 'medium' };
const custom = { color: 'blue', weight: 'heavy' };

const merged = Object.assign({}, defaults, custom);
console.log(merged);
// { color: 'blue', size: 'medium', weight: 'heavy' }

// Shorthand (modern)
const merged2 = { ...defaults, ...custom };
```

### `Object.freeze()` - Immutable Object

```javascript
const config = Object.freeze({
  apiUrl: 'https://api.example.com',
  timeout: 5000
});

config.apiUrl = 'https://other.com'; // Silently fails (or throws in strict mode)
console.log(config.apiUrl); // "https://api.example.com"
```

### `Object.seal()` - Prevent Add/Remove

```javascript
const user = Object.seal({
  name: 'John',
  age: 30
});

user.name = 'Jane'; // Works (modification allowed)
user.email = 'jane@example.com'; // Fails (can't add)
delete user.age; // Fails (can't delete)
```

## Method Patterns

### Method Chaining

```javascript
class QueryBuilder {
  constructor() {
    this.conditions = [];
    this.orderByField = null;
    this.limitValue = null;
  }

  where(condition) {
    this.conditions.push(condition);
    return this;
  }

  order(field) {
    this.orderByField = field;
    return this;
  }

  take(n) {
    this.limitValue = n;
    return this;
  }

  build() {
    return {
      where: this.conditions,
      orderBy: this.orderByField,
      limit: this.limitValue
    };
  }
}

const query = new QueryBuilder()
  .where('age > 18')
  .where('active = true')
  .order('name')
  .take(10)
  .build();
```

### Getters and Setters

```javascript
class Temperature {
  constructor(celsius) {
    this._celsius = celsius;
  }

  get fahrenheit() {
    return this._celsius * 9/5 + 32;
  }

  set fahrenheit(f) {
    this._celsius = (f - 32) * 5/9;
  }

  get kelvin() {
    return this._celsius + 273.15;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // 212
temp.fahrenheit = 32;
console.log(temp.celsius); // 0
```

### Static Methods

```javascript
class MathUtils {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }

  static clamp(value, min, max) {
    return Math.min(Math.max(value, min), max);
  }
}

console.log(MathUtils.add(5, 3)); // 8
console.log(MathUtils.clamp(15, 0, 10)); // 10
```

## Common Use Cases

### Data Transformation

```javascript
const users = [
  { name: 'John', age: 30 },
  { name: 'Jane', age: 25 },
  { name: 'Bob', age: 35 }
];

// Transform to lookup object
const usersByName = Object.fromEntries(
  users.map(user => [user.name, user])
);

console.log(usersByName.John); // { name: 'John', age: 30 }
```

### Object Comparison

```javascript
function shallowEqual(obj1, obj2) {
  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);

  if (keys1.length !== keys2.length) return false;

  return keys1.every(key => obj1[key] === obj2[key]);
}

console.log(shallowEqual({ a: 1, b: 2 }, { a: 1, b: 2 })); // true
```

### Pick/Omit Properties

```javascript
function pick(obj, keys) {
  return keys.reduce((result, key) => {
    if (obj[key] !== undefined) {
      result[key] = obj[key];
    }
    return result;
  }, {});
}

function omit(obj, keys) {
  return Object.keys(obj).reduce((result, key) => {
    if (!keys.includes(key)) {
      result[key] = obj[key];
    }
    return result;
  }, {});
}

const user = { name: 'John', age: 30, password: 'secret' };
console.log(pick(user, ['name', 'age'])); // { name: 'John', age: 30 }
console.log(omit(user, ['password'])); // { name: 'John', age: 30 }
```

## Common Mistakes

```javascript
const obj = {
  name: 'John',
  greet() {
    // ❌ Wrong: Arrow function doesn't bind 'this'
    return () => this.name; // 'this' won't work as expected
  }
};

// ✅ Correct: Regular function
const obj2 = {
  name: 'John',
  greet() {
    return this.name;
  }
};

// ❌ Wrong: Calling method without context
const greet = obj2.greet;
// console.log(greet()); // undefined (lost context)

// ✅ Correct: Bind or use arrow
const greet = obj2.greet.bind(obj2);
console.log(greet()); // "John"
```

## Related Topics

- [[this-keyword]] - Understanding context binding
- [[Prototypes]] - Method inheritance
- [[Spread-Operator]] - Object copying
- [[Destructuring]] - Extracting properties
- [[Classes]] - Modern OOP syntax
- [[Getters-Setters]] - Accessor properties

## Quick Revision

**Built-in Methods:**
| Method | Purpose |
|--------|---------|
| `Object.keys()` | Get property names |
| `Object.values()` | Get property values |
| `Object.entries()` | Get key-value pairs |
| `Object.assign()` | Merge/copy objects |
| `Object.freeze()` | Make immutable |
| `Object.seal()` | Prevent add/remove |

**Key Concepts:**
- Methods are functions stored in objects
- `this` refers to the containing object
- Method chaining requires returning `this`
- Static methods belong to the class, not instances
- Use `bind()` to preserve context