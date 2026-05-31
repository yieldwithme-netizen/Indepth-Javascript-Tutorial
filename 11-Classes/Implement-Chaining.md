# How to Implement Method Chaining in JavaScript Classes

## Definition

Method chaining is a design pattern where multiple methods are called on the same object consecutively, with each method returning `this` (the object itself) to allow the next method call. This creates a fluent interface that makes code more readable and concise.

## Syntax

```javascript
class ClassName {
  method1() {
    // Do something
    return this; // Return the instance
  }

  method2() {
    // Do something
    return this; // Return the instance
  }
}

// Chaining
const obj = new ClassName();
obj.method1().method2().method1();
```

## Code Examples

### Basic Method Chaining

```javascript
class Calculator {
  constructor(value = 0) {
    this.value = value;
  }

  add(n) {
    this.value += n;
    return this;
  }

  subtract(n) {
    this.value -= n;
    return this;
  }

  multiply(n) {
    this.value *= n;
    return this;
  }

  result() {
    return this.value;
  }
}

const calc = new Calculator(10);
const finalValue = calc.add(5).multiply(2).subtract(3).result();
console.log(finalValue); // 27
```

### Builder Pattern with Chaining

```javascript
class QueryBuilder {
  constructor() {
    this.table = '';
    this.conditions = [];
    this.orderByField = '';
    this.limitCount = null;
  }

  from(table) {
    this.table = table;
    return this;
  }

  where(condition) {
    this.conditions.push(condition);
    return this;
  }

  orderBy(field) {
    this.orderByField = field;
    return this;
  }

  limit(count) {
    this.limitCount = count;
    return this;
  }

  build() {
    let query = `SELECT * FROM ${this.table}`;
    if (this.conditions.length > 0) {
      query += ` WHERE ${this.conditions.join(' AND ')}`;
    }
    if (this.orderByField) {
      query += ` ORDER BY ${this.orderByField}`;
    }
    if (this.limitCount) {
      query += ` LIMIT ${this.limitCount}`;
    }
    return query;
  }
}

const query = new QueryBuilder()
  .from('users')
  .where('age > 18')
  .where('active = true')
  .orderBy('name')
  .limit(10)
  .build();

console.log(query);
// "SELECT * FROM users WHERE age > 18 AND active = true ORDER BY name LIMIT 10"
```

### Object Configuration with Chaining

```javascript
class ConfigBuilder {
  constructor() {
    this.config = {};
  }

  set(key, value) {
    this.config[key] = value;
    return this;
  }

  setMultiple(obj) {
    Object.assign(this.config, obj);
    return this;
  }

  remove(key) {
    delete this.config[key];
    return this;
  }

  reset() {
    this.config = {};
    return this;
  }

  get(key) {
    return this.config[key];
  }

  build() {
    return { ...this.config };
  }
}

const config = new ConfigBuilder()
  .set('theme', 'dark')
  .set('language', 'en')
  .setMultiple({ fontSize: 14, fontFamily: 'Arial' })
  .set('debug', false)
  .remove('debug')
  .build();

console.log(config); // { theme: 'dark', language: 'en', fontSize: 14, fontFamily: 'Arial' }
```

### Array-like Chaining

```javascript
class DataProcessor {
  constructor(data) {
    this.data = data;
  }

  filter(predicate) {
    this.data = this.data.filter(predicate);
    return this;
  }

  map(transform) {
    this.data = this.data.map(transform);
    return this;
  }

  sort(compareFn) {
    this.data = this.data.sort(compareFn);
    return this;
  }

  take(n) {
    this.data = this.data.slice(0, n);
    return this;
  }

  toArray() {
    return [...this.data];
  }
}

const numbers = [5, 3, 8, 1, 9, 2, 7, 4, 6];

const result = new DataProcessor(numbers)
  .filter(n => n > 3)
  .sort((a, b) => a - b)
  .map(n => n * 2)
  .take(3)
  .toArray();

console.log(result); // [8, 10, 12]
```

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| Builder Pattern | Constructing complex objects step-by-step |
| Fluent APIs | Database query builders, form builders |
| Configuration | Setting multiple options on an object |
| Data Processing | Filter, map, sort operations |
| DOM Manipulation | jQuery-style chaining |
| Test Assertions | Chaining assertion methods |

## Common Mistakes

```javascript
// ❌ Wrong: Forgetting to return this
class Builder {
  constructor() {
    this.items = [];
  }

  addItem(item) {
    this.items.push(item);
    // Missing return this;
  }
}

// const builder = new Builder();
// builder.addItem('a').addItem('b'); // TypeError: builder.addItem is not a function

// ✅ Correct: Always return this
class CorrectBuilder {
  constructor() {
    this.items = [];
  }

  addItem(item) {
    this.items.push(item);
    return this;
  }
}

// ❌ Wrong: Returning a new object instead of this
class WrongBuilder {
  addItem(item) {
    return new WrongBuilder(); // Creates new instance
  }
}

// ✅ Correct: Return the same instance
class CorrectBuilder2 {
  addItem(item) {
    this.items.push(item);
    return this; // Returns same instance
  }
}
```

## Related Topics

- [[Create-Class]] - Creating classes in JavaScript
- [[What-is-Encapsulation]] - Encapsulation principles
- [[Implement-Encapsulation]] - Implementing encapsulation
- [[What-is-Polymorphism]] - Polymorphism concepts
- [[Override-Methods]] - Overriding class methods

## Quick Revision

| Concept | Key Point |
|---------|-----------|
| Return Value | Methods must return `this` for chaining |
| Fluent Interface | Creates readable, chainable method calls |
| Builder Pattern | Common use case for method chaining |
| Immutability | Consider if chaining should modify original object |
| Return Type | Always return the same instance, not a new one |
| Termination | Provide a method to get the final result |
