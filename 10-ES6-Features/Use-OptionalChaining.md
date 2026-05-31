# How to Use Optional Chaining `?.`

## Definition
Optional chaining (`?.`) is an error-proof way to access nested object properties. If any part of the chain is `null` or `undefined`, the expression short-circuits and returns `undefined` instead of throwing an error.

## Syntax

```javascript
// Property access
obj?.prop
obj?.prop?.nested

// Method calls
obj.method?.()

// Array access
obj?.[index]
```

## Basic Usage

```javascript
const user = {
  name: "John",
  address: {
    city: "NYC"
  }
};

// Without optional chaining
const street = user.address && user.address.street;
const zip = user.address ? user.address.zip : undefined;

// With optional chaining
const street2 = user.address?.street;  // undefined (no error)
const zip2 = user.address?.zip;        // undefined (no error)
```

## Deeply Nested Access

```javascript
const data = {
  company: {
    departments: [
      {
        employees: [
          { name: "Alice", role: "Developer" }
        ]
      }
    ]
  }
};

// Safe deep access
const firstEmployee = data.company?.departments?.[0]?.employees?.[0]?.name;
// "Alice"

const secondEmployee = data.company?.departments?.[1]?.employees?.[0]?.name;
// undefined (no error)
```

## Method Calls

```javascript
const user = {
  name: "John",
  greet() {
    return `Hello, ${this.name}`;
  }
};

// Without optional chaining
const greeting = user.greet && user.greet();

// With optional chaining
const greeting2 = user.greet?.();  // "Hello, John"
const result = user.nonExistent?.();  // undefined (no error)
```

## Array Access

```javascript
const items = [1, 2, 3];

const first = items?.[0];  // 1
const tenth = items?.[9];  // undefined (no error)
```

## Common Use Cases

### API Response Handling

```javascript
const response = {
  data: {
    user: {
      profile: {
        name: "John",
        avatar: null
      }
    }
  }
};

const avatar = response.data?.user?.profile?.avatar?.url;
// undefined (safe access)
```

### DOM Element Access

```javascript
const element = document.querySelector(".non-existent");
const text = element?.textContent;  // undefined (no error)
```

### Optional Chaining with Assignment

```javascript
// This throws an error
const obj = {};
obj?.prop = "value";  // SyntaxError

// Use with if statement instead
if (obj?.prop) {
  obj.prop = "value";
}
```

## Common Mistakes

```javascript
// Wrong: Trying to assign with optional chaining
obj?.prop = "value";  // SyntaxError

// Wrong: Using on non-existent root object
const obj = undefined;
obj?.prop;  // TypeError: obj is not defined

// Wrong: Short-circuiting assignment
const a = undefined;
a?.b = "value";  // SyntaxError
```

## Quick Revision

- `?.` prevents `TypeError` on null/undefined access
- Short-circuits and returns `undefined` if chain fails
- Works with properties, methods, and array access
- Cannot be used for assignments
- Replaces verbose `&&` chains and ternary checks

## Related Topics

- [[Use-Nullish]] - Nullish coalescing operator `??`
- [[Use-Spread]] - Spread syntax
- [[What-is-Null]] - Understanding null and undefined
- [[Access-Properties]] - Property access patterns
