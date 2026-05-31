# What is Method Chaining?

Method chaining is a technique where multiple methods are called on the same object in a single statement. Each method returns the object itself (`this`), allowing the next method to be called on it.

## Definition

Method chaining works when:
1. Each method returns `this` (the current object)
2. The next method can be called on the returned object
3. The chain continues until a terminating method is called

## Syntax

```javascript
class Builder {
  method1() {
    // Do something
    return this; // Enable chaining
  }

  method2() {
    // Do something
    return this; // Enable chaining
  }

  build() {
    // Return final result
    return this.result;
  }
}

// Chained calls
new Builder()
  .method1()
  .method2()
  .method3()
  .build();
```

## Basic Example

```javascript
class QueryBuilder {
  constructor() {
    this.table = "";
    this.conditions = [];
    this.orderField = null;
    this.limitValue = null;
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
    this.orderField = field;
    return this;
  }

  limit(n) {
    this.limitValue = n;
    return this;
  }

  build() {
    let query = `SELECT * FROM ${this.table}`;
    if (this.conditions.length > 0) {
      query += ` WHERE ${this.conditions.join(" AND ")}`;
    }
    if (this.orderField) {
      query += ` ORDER BY ${this.orderField}`;
    }
    if (this.limitValue) {
      query += ` LIMIT ${this.limitValue}`;
    }
    return query;
  }
}

const query = new QueryBuilder()
  .from("users")
  .where("age > 18")
  .where("active = true")
  .orderBy("name")
  .limit(10)
  .build();

console.log(query);
// Output: SELECT * FROM users WHERE age > 18 AND active = true ORDER BY name LIMIT 10
```

## Chaining with Array-like Methods

```javascript
class DataPipeline {
  constructor(data) {
    this.data = [...data];
  }

  filter(predicate) {
    this.data = this.data.filter(predicate);
    return this;
  }

  map(transform) {
    this.data = this.data.map(transform);
    return this;
  }

  sort(comparator) {
    this.data = this.data.sort(comparator);
    return this;
  }

  take(n) {
    this.data = this.data.slice(0, n);
    return this;
  }

  result() {
    return [...this.data];
  }
}

const result = new DataPipeline([5, 3, 8, 1, 9, 2, 7])
  .filter(x => x > 3)
  .sort((a, b) => a - b)
  .take(3)
  .result();

console.log(result); // Output: [5, 7, 8]
```

## Builder Pattern

```javascript
class Pizza {
  constructor() {
    this.size = "medium";
    this.crust = "regular";
    this.toppings = [];
    this.sauce = "tomato";
    this.cheese = "mozzarella";
  }

  setSize(size) {
    this.size = size;
    return this;
  }

  setCrust(crust) {
    this.crust = crust;
    return this;
  }

  addTopping(topping) {
    this.toppings.push(topping);
    return this;
  }

  setSauce(sauce) {
    this.sauce = sauce;
    return this;
  }

  setCheese(cheese) {
    this.cheese = cheese;
    return this;
  }

  build() {
    return {
      size: this.size,
      crust: this.crust,
      toppings: this.toppings,
      sauce: this.sauce,
      cheese: this.cheese
    };
  }
}

const pizza = new Pizza()
  .setSize("large")
  .setCrust("thin")
  .addTopping("pepperoni")
  .addTopping("mushrooms")
  .setSauce("garlic")
  .setCheese("parmesan")
  .build();

console.log(pizza);
// Output: { size: 'large', crust: 'thin', toppings: ['pepperoni', 'mushrooms'], ... }
```

## Fluent Interface

```javascript
class StringBuilder {
  #parts = [];

  append(text) {
    this.#parts.push(text);
    return this;
  }

  prepend(text) {
    this.#parts.unshift(text);
    return this;
  }

  replace(search, replace) {
    this.#parts = this.#parts.map(part =>
      part.split(search).join(replace)
    );
    return this;
  }

  uppercase() {
    this.#parts = this.#parts.map(part => part.toUpperCase());
    return this;
  }

  build() {
    return this.#parts.join("");
  }
}

const result = new StringBuilder()
  .append("Hello")
  .append(" ")
  .append("World")
  .replace("World", "JavaScript")
  .uppercase()
  .build();

console.log(result); // Output: HELLO JAVASCRIPT
```

## Chaining with Validation

```javascript
class FormValidator {
  #errors = [];
  #data = {};

  set(field, value) {
    this.#data[field] = value;
    return this;
  }

  required(field, message) {
    if (!this.#data[field]) {
      this.#errors.push({ field, message: message || `${field} is required` });
    }
    return this;
  }

  minLength(field, min, message) {
    if (this.#data[field] && this.#data[field].length < min) {
      this.#errors.push({
        field,
        message: message || `${field} must be at least ${min} characters`
      });
    }
    return this;
  }

  validate() {
    return {
      isValid: this.#errors.length === 0,
      errors: [...this.#errors],
      data: { ...this.#data }
    };
  }
}

const result = new FormValidator()
  .set("username", "ab")
  .set("email", "invalid")
  .required("username")
  .minLength("username", 3)
  .required("email")
  .validate();

console.log(result);
// Output: { isValid: false, errors: [...], data: { username: 'ab', email: 'invalid' } }
```

## Common Use Cases

- Query builders
- Form validators
- Configuration builders
- Data pipelines
- String builders
- DOM manipulation (jQuery style)
- Promise chains (similar pattern)

## Common Mistakes

1. **Forgetting to return `this`**
   ```javascript
   // Wrong
   class Bad {
     method1() {
       this.value = 1;
       // No return - breaks chain
     }
   }

   // Correct
   class Good {
     method1() {
       this.value = 1;
       return this;
     }
   }
   ```

2. **Returning something other than `this`**
   ```javascript
   // Wrong
   method1() {
     return this.value; // Returns value, not object
   }
   ```

3. **Calling chain-terminating methods too early**
   ```javascript
   // Wrong - build() terminates chain
   new Builder().method1().build().method2(); // Error

   // Correct
   new Builder().method1().method2().build();
   ```

4. **Chaining void methods**
   ```javascript
   // Wrong - push returns new length, not array
   arr.push(1).push(2); // Error
   ```

## Related Topics

- [[What-is-Constructor]]
- [[What-is-GetSet]]
- [[Use-GetSet]]
- [[What-is-Static]]
- [[What-is-Private]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Purpose | Call multiple methods in one statement |
| Mechanism | Each method returns `this` |
| Terminating | Final method returns result, not `this` |
| Use case | Builder pattern, fluent interfaces |
| Benefit | Readable, concise code |
| Anti-pattern | Returning values before chain ends |
