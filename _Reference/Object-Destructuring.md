# Object Destructuring

## Definition

Object destructuring is an ES6 feature that allows you to extract values from objects and bind them to variables in a single, concise statement. It provides a clean way to access object properties without repeatedly referencing the object name.

## Syntax

```javascript
// Basic syntax
const { property1, property2 } = object;

// With renaming
const { property1: newName1, property2: newName2 } = object;

// With default values
const { property1 = defaultValue1 } = object;

// Nested destructuring
const { nested: { property } } = object;

// Rest properties
const { prop1, ...rest } = object;
```

## Code Examples

### Basic Destructuring

```javascript
const person = {
  name: "Alice",
  age: 25,
  city: "Seattle"
};

// Without destructuring
const name1 = person.name;
const age1 = person.age;

// With destructuring
const { name, age, city } = person;

console.log(name);  // "Alice"
console.log(age);   // 25
console.log(city);  // "Seattle"
```

### Renaming Variables

```javascript
const user = {
  name: "Bob",
  age: 30,
  email: "bob@example.com"
};

const { name: userName, age: userAge, email: userEmail } = user;

console.log(userName);   // "Bob"
console.log(userAge);    // 30
console.log(userEmail);  // "bob@example.com"
```

### Default Values

```javascript
const config = {
  host: "localhost",
  port: 3000
};

const { host, port, protocol = "http" } = config;

console.log(host);      // "localhost"
console.log(port);      // 3000
console.log(protocol);  // "http" (default value)
```

### Nested Destructuring

```javascript
const user = {
  name: "Charlie",
  address: {
    street: "123 Main St",
    city: "Portland",
    state: "OR",
    zip: "97201"
  }
};

const { name, address: { street, city, state } } = user;

console.log(name);    // "Charlie"
console.log(street);  // "123 Main St"
console.log(city);    // "Portland"
console.log(state);   // "OR"
```

### Function Parameters

```javascript
// Destructuring in function parameters
function greet({ name, age, greeting = "Hello" }) {
  return `${greeting}, ${name}! You are ${age} years old.`;
}

const person = { name: "Dave", age: 28 };
console.log(greet(person)); // "Hello, Dave! You are 28 years old."

// With renaming
function processUser({ name: userName, age: userAge }) {
  console.log(`Processing ${userName}, age ${userAge}`);
}
```

### Rest Properties

```javascript
const user = {
  name: "Eve",
  age: 35,
  email: "eve@example.com",
  city: "Austin",
  country: "USA"
};

const { name, age, ...contactInfo } = user;

console.log(name);         // "Eve"
console.log(age);          // 35
console.log(contactInfo);  // { email: "eve@example.com", city: "Austin", country: "USA" }
```

### Practical Examples

```javascript
// API response handling
function processApiResponse({ data, status, message }) {
  if (status === 200) {
    return data;
  }
  throw new Error(message);
}

const response = {
  data: { users: [] },
  status: 200,
  message: "Success"
};

const users = processApiResponse(response);

// Swapping variables
let a = 1;
let b = 2;
[a, b] = [b, a]; // a = 2, b = 1

// Array of objects
const users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 }
];

const [{ name: firstUser }, { name: secondUser }] = users;
console.log(firstUser);   // "Alice"
console.log(secondUser);  // "Bob"
```

### Deep Nested Destructuring

```javascript
const company = {
  name: "TechCorp",
  departments: {
    engineering: {
      teams: {
        frontend: {
          lead: "Alice",
          members: 5
        },
        backend: {
          lead: "Bob",
          members: 8
        }
      }
    }
  }
};

const {
  departments: {
    engineering: {
      teams: {
        frontend: { lead: frontendLead },
        backend: { lead: backendLead }
      }
    }
  }
} = company;

console.log(frontendLead);  // "Alice"
console.log(backendLead);   // "Bob"
```

### Conditional Destructuring

```javascript
const data = {
  user: {
    name: "Charlie",
    settings: {
      theme: "dark",
      notifications: true
    }
  }
};

// Safe destructuring with defaults
const {
  user: { name, settings: { theme = "light", notifications = false } = {} } = {}
} = data;

console.log(name);           // "Charlie"
console.log(theme);          // "dark"
console.log(notifications);  // true
```

## Common Use Cases

1. **Function parameters** - Clean, self-documenting function signatures
2. **API responses** - Extract specific data from responses
3. **Configuration objects** - Access settings with defaults
4. **State management** - Extract state in React components
5. **Import/export** - Selective module imports

## Common Mistakes

1. **Accessing non-existent properties** - Results in `undefined`
2. **Forgetting to declare variables** - Must use `const`/`let`/`var`
3. **Not handling missing properties** - Use default values
4. **Over-nesting** - Keep destructuring shallow for readability

```javascript
// WRONG: Missing property
const { missing } = { name: "Alice" };
console.log(missing); // undefined

// RIGHT: With default value
const { missing = "default" } = { name: "Alice" };
console.log(missing); // "default"
```

## Quick Revision Summary

- Extracts properties from objects into variables
- Supports renaming, defaults, and nested destructuring
- Rest properties collect remaining properties
- Improves code readability and reduces repetition
- Use default values for optional properties
- Excellent for function parameters and API responses

## Related Topics

- [[Spread-Operator]]
- [[Rest-Parameters]]
- [[ES6-Features]]
- [[Array-Destructuring]]
- [[Default-Parameters]]
- [[Arrow-Functions]]
