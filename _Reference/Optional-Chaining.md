# Optional Chaining

## Definition

Optional chaining (`?.`) is a modern JavaScript operator that allows you to safely access deeply nested properties of an object without worrying about whether intermediate properties exist. It short-circuits and returns `undefined` if any part of the chain is `null` or `undefined`, preventing runtime errors.

## Syntax

```javascript
obj?.prop
obj?.[expr]
obj?.method()
```

## Code Examples

### Basic Usage

```javascript
const user = {
  name: "Alice",
  address: {
    city: "Seattle",
    zip: "98101"
  }
};

// Without optional chaining
const city1 = user && user.address && user.address.city;

// With optional chaining
const city2 = user?.address?.city; // "Seattle"
const zip = user?.address?.zip; // "98101"
```

### Accessing Nested Properties

```javascript
const company = {
  name: "TechCorp",
  ceo: {
    name: "Bob",
    contact: {
      email: "bob@techcorp.com"
    }
  }
};

console.log(company?.ceo?.name); // "Bob"
console.log(company?.ceo?.contact?.email); // "bob@techcorp.com"
console.log(company?.cto?.name); // undefined (cto doesn't exist)
```

### Array Access

```javascript
const data = {
  items: ["apple", "banana", "cherry"]
};

console.log(data?.items?.[0]); // "apple"
console.log(data?.items?.[5]); // undefined
console.log(data?.prices?.[0]); // undefined (prices doesn't exist)
```

### Method Calls

```javascript
const user = {
  name: "Alice",
  greet() {
    return `Hello, ${this.name}`;
  }
};

console.log(user?.greet()); // "Hello, Alice"
console.log(user?.sayGoodbye()); // undefined (method doesn't exist)

// Safe function call
const fn = user?.nonExistentMethod?.();
console.log(fn); // undefined
```

### Dynamic Property Access

```javascript
const obj = {
  name: "Alice",
  age: 25
};

const propName = "name";
console.log(obj?.[propName]); // "Alice"
```

### Practical Examples

```javascript
// API response handling
function getUserCity(response) {
  return response?.data?.user?.address?.city;
}

const response = {
  data: {
    user: {
      address: {
        city: "Portland"
      }
    }
  }
};

console.log(getUserCity(response)); // "Portland"
console.log(getUserCity({})); // undefined
```

### Optional Chaining with Nullish Coalescing

```javascript
const settings = {
  theme: "dark",
  notifications: {
    email: true
  }
};

// Get value with default
const emailEnabled = settings?.notifications?.email ?? false; // true
const smsEnabled = settings?.notifications?.sms ?? false; // false
const language = settings?.language ?? "en"; // "en"
```

### Class Instances

```javascript
class User {
  constructor(name, address) {
    this.name = name;
    this.address = address;
  }
}

function getStreet(user) {
  return user?.address?.street;
}

const user1 = new User("Alice", { street: "123 Main St" });
const user2 = new User("Bob", null);

console.log(getStreet(user1)); // "123 Main St"
console.log(getStreet(user2)); // undefined
```

### DOM Element Access

```javascript
// Safe DOM access
const element = document?.getElementById("app");
const text = element?.textContent;

// Safe event handling
element?.addEventListener("click", () => {
  console.log("Clicked");
});
```

### Chaining Multiple Levels

```javascript
const database = {
  users: {
    1: {
      name: "Alice",
      posts: [
        { id: 1, title: "Hello" },
        { id: 2, title: "World" }
      ]
    }
  }
};

console.log(database?.users?.[1]?.posts?.[0]?.title); // "Hello"
console.log(database?.users?.[2]?.posts?.[0]?.title); // undefined
```

## Common Use Cases

1. **API responses** - Safely access nested response data
2. **Configuration objects** - Handle optional settings
3. **DOM manipulation** - Safe element access
4. **User input validation** - Handle incomplete data
5. **Chain method calls** - Avoid "Cannot read property" errors

## Common Mistakes

1. **Using when you expect values** - Optional chaining hides bugs if values should exist
2. **Overusing** - Not every property access needs optional chaining
3. **Confusing with logical AND** - `?.` is different from `&&`
4. **Not handling undefined** - Optional chaining returns `undefined`, not a default value

```javascript
// WRONG: Always using optional chaining
const name = user?.name?.first?.toString(); // Might hide bugs

// RIGHT: Use when values are optional
const name = user?.name?.first; // Only if name/first are optional
```

## Quick Revision Summary

- `?.` safely accesses nested properties
- Returns `undefined` if any part is null/undefined
- Works with properties, methods, and array access
- Combines well with nullish coalescing (`??`) for defaults
- Prevents "Cannot read property of undefined" errors
- Use when properties are genuinely optional

## Related Topics

- [[Nullish-Coalescing]]
- [[ES6-Features]]
- [[Null-Checking]]
- [[Error-Handling]]
- [[Array-Methods]]
- [[Object-Destructuring]]
