# How to Parse and Stringify JSON

**JSON (JavaScript Object Notation)** is a lightweight data format. Use `JSON.parse()` to convert JSON strings to objects and `JSON.stringify()` to convert objects to JSON strings.

## JSON.stringify() - Object to String

```javascript
const person = {
  name: "John",
  age: 30,
  hobbies: ["reading", "gaming"]
};

const jsonString = JSON.stringify(person);
console.log(jsonString);
// '{"name":"John","age":30,"hobbies":["reading","gaming"]}'
```

## JSON.parse() - String to Object

```javascript
const jsonString = '{"name":"John","age":30,"hobbies":["reading","gaming"]}';

const person = JSON.parse(jsonString);
console.log(person.name); // "John"
console.log(person.age);  // 30
```

## Pretty Printing

```javascript
const data = { name: "John", age: 30, city: "New York" };

// Compact (default)
console.log(JSON.stringify(data));
// '{"name":"John","age":30,"city":"New York"}'

// Pretty (with indentation)
console.log(JSON.stringify(data, null, 2));
// {
//   "name": "John",
//   "age": 30,
//   "city": "New York"
// }
```

## Handling Complex Data

```javascript
const complexData = {
  users: [
    { id: 1, name: "Alice", active: true },
    { id: 2, name: "Bob", active: false }
  ],
  total: 2
};

// Stringify
const json = JSON.stringify(complexData, null, 2);
console.log(json);

// Parse back
const parsed = JSON.parse(json);
console.log(parsed.users[0].name); // "Alice"
```

## Custom Replacer and Reviver

```javascript
// Replacer - filter/transform during stringify
const obj = { name: "John", password: "secret", age: 30 };
const safe = JSON.stringify(obj, (key, value) => {
  if (key === "password") return undefined;
  return value;
});
console.log(safe); // '{"name":"John","age":30}'

// Reviver - transform during parse
const json = '{"name":"John","birth":"1990-01-01"}';
const withDate = JSON.parse(json, (key, value) => {
  if (key === "birth") return new Date(value);
  return value;
});
console.log(withDate.birth instanceof Date); // true
```

## Common Use Cases

- API communication
- Local storage
- Configuration files
- Data serialization

## Common Mistakes

```javascript
// ❌ Trying to stringify functions
const obj = { greet: () => "hello" };
console.log(JSON.stringify(obj)); // '{}' (functions removed)

// ❌ Trying to stringify undefined
console.log(JSON.stringify({ a: undefined })); // '{}' (undefined removed)

// ❌ Parsing invalid JSON
try {
  JSON.parse("not valid json");
} catch (e) {
  console.log("Invalid JSON"); // This runs
}

// ❌ Not handling circular references
const circular = {};
circular.self = circular;
// JSON.stringify(circular); // TypeError: Converting circular structure to JSON

// ✅ Handle circular references
const seen = new WeakSet();
const safeStringify = (obj) => {
  return JSON.stringify(obj, (key, value) => {
    if (typeof value === 'object' && value !== null) {
      if (seen.has(value)) return '[Circular]';
      seen.add(value);
    }
    return value;
  });
};
```

## Related Topics

- [[Define-Objects]]
- [[Access-Properties]]
- [[Fetch-API]]
- [[Local-Storage]]
- [[Async-Await]]

## Quick Revision

| Method | Purpose | Example |
|--------|---------|---------|
| `JSON.stringify(obj)` | Object → String | `'{"name":"John"}'` |
| `JSON.parse(str)` | String → Object | `{ name: "John" }` |

**Key Point:** Always wrap `JSON.parse()` in try-catch; remember that functions and `undefined` are lost during stringify.