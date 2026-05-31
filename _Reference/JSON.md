# JSON in JavaScript

## Definition

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate. It is based on JavaScript object syntax but is language-independent.

## Syntax

### Data Types

```javascript
// JSON supports these types:
{
  "string": "Hello, World!",      // Strings (double quotes required)
  "number": 42,                   // Numbers
  "float": 3.14,                  // Floating point
  "boolean": true,                // Booleans (lowercase)
  "null": null,                   // Null
  "array": [1, 2, 3],            // Arrays
  "object": {                     // Objects
    "key": "value"
  }
}
```

## JSON Methods

### `JSON.parse()` - String to Object

```javascript
const jsonString = '{"name": "John", "age": 30}';
const user = JSON.parse(jsonString);

console.log(user.name); // "John"
console.log(user.age);  // 30
```

### `JSON.stringify()` - Object to String

```javascript
const user = { name: 'John', age: 30 };
const jsonString = JSON.stringify(user);

console.log(jsonString); // '{"name":"John","age":30}'
console.log(typeof jsonString); // "string"
```

## Advanced Usage

### Pretty Printing

```javascript
const data = { name: 'John', age: 30, address: { city: 'NYC' } };

// Default (compact)
console.log(JSON.stringify(data));
// '{"name":"John","age":30,"address":{"city":"NYC"}}'

// Pretty printed with 2-space indent
console.log(JSON.stringify(data, null, 2));
// {
//   "name": "John",
//   "age": 30,
//   "address": {
//     "city": "NYC"
//   }
// }

// Pretty printed with 4-space indent
console.log(JSON.stringify(data, null, 4));
```

### Custom Serialization

```javascript
const user = {
  name: 'John',
  age: 30,
  password: 'secret',
  date: new Date()
};

// Filter out sensitive fields
const filtered = JSON.stringify(user, (key, value) => {
  if (key === 'password') return undefined;
  if (value instanceof Date) return value.toISOString();
  return value;
});

console.log(filtered);
// '{"name":"John","age":30,"date":"2024-01-15T..."}'
```

### Replacer Array

```javascript
const user = { name: 'John', age: 30, email: 'john@example.com' };

// Only include specified keys
const result = JSON.stringify(user, ['name', 'email']);
console.log(result); // '{"name":"John","email":"john@example.com"}'
```

## Common Use Cases

### API Communication

```javascript
// Sending data
const response = await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'John', age: 30 })
});

// Receiving data
const data = await response.json();
```

### Local Storage

```javascript
// Store object
const settings = { theme: 'dark', lang: 'en' };
localStorage.setItem('settings', JSON.stringify(settings));

// Retrieve object
const stored = JSON.parse(localStorage.getItem('settings'));
```

### Deep Clone (Simple Objects)

```javascript
const original = { a: 1, b: { c: 2 } };

// Simple deep clone
const clone = JSON.parse(JSON.stringify(original));

clone.b.c = 3;
console.log(original.b.c); // 2 (unchanged)
```

### URL Parameters

```javascript
// Encode object for URL
const params = { search: 'hello', page: 1, limit: 10 };
const queryString = encodeURIComponent(JSON.stringify(params));

// Decode from URL
const decoded = JSON.parse(decodeURIComponent(queryString));
```

## Parsing Options

### Reviver Function

```javascript
const jsonString = '{"name":"John","birth":"1990-01-15"}';

// Convert date string to Date object
const user = JSON.parse(jsonString, (key, value) => {
  if (key === 'birth') return new Date(value);
  return value;
});

console.log(user.birth instanceof Date); // true
```

## Common Mistakes

```javascript
// ❌ Wrong: Single quotes
// '{"name": "John"}'; // SyntaxError

// ✅ Correct: Double quotes
'{"name": "John"}';

// ❌ Wrong: Trailing commas
// '{"name": "John",}'; // SyntaxError

// ✅ Correct: No trailing commas
'{"name": "John"}';

// ❌ Wrong: Undefined values
JSON.stringify({ a: undefined }); // '{}' (key omitted)

// ❌ Wrong: Functions
JSON.stringify({ fn: () => {} }); // '{}' (function omitted)

// ✅ Correct: Convert undefined
JSON.stringify({ a: undefined }, (k, v) => v === undefined ? null : v);
// '{"a":null}'
```

## Error Handling

```javascript
try {
  const data = JSON.parse('invalid json');
} catch (error) {
  console.error('Parse error:', error.message);
  // "Parse error: Unexpected token i in JSON at position 0"
}

try {
  const circular = {};
  circular.self = circular;
  JSON.stringify(circular);
} catch (error) {
  console.error('Stringify error:', error.message);
  // "Converting circular structure to JSON"
}
```

## JSON vs JavaScript Objects

| Feature | JSON | JS Object |
|---------|------|-----------|
| Keys | Double-quoted strings | Strings, symbols, numbers |
| Values | String, number, boolean, null, array, object | Any JS value |
| Methods | Not allowed | Allowed |
| `undefined` | Not allowed | Allowed |
| Comments | Not allowed | Allowed |

## Related Topics

- [[Fetch-API]] - Making HTTP requests with JSON
- [[Async-Await]] - Handling asynchronous JSON operations
- [[LocalStorage]] - Persisting JSON data
- [[Object-Methods]] - Working with JavaScript objects
- [[Spread-Operator]] - Object copying techniques
- [[Modules]] - Importing/exporting data

## Quick Revision

| Method | Purpose | Returns |
|--------|---------|---------|
| `JSON.parse()` | String → Object | JavaScript object |
| `JSON.stringify()` | Object → String | JSON string |

**Key Rules:**
- JSON keys must be double-quoted strings
- No trailing commas in JSON
- `undefined`, functions, symbols are omitted
- Circular references cause errors

**Common Use Cases:**
- API data exchange
- Local storage persistence
- Configuration files
- Data serialization