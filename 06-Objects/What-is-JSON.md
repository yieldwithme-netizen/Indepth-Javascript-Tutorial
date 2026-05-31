# What is [[JSON]]?

## Definition

[[JSON]] (JavaScript Object Notation) is a **lightweight data format** for storing and exchanging data.

## [[JSON]] Syntax

```javascript
// Object
{
    "name": "John",
    "age": 30,
    "isStudent": false
}

// Array
[1, 2, 3, "hello"]

// Nested
{
    "name": "John",
    "address": {
        "street": "123 Main St",
        "city": "NYC"
    },
    "hobbies": ["reading", "gaming"]
}
```

## [[JSON]] vs JavaScript Object

```javascript
// JavaScript Object
const obj = {
    name: "John",      // keys don't need quotes
    age: 30,
    isStudent: false,
    greet() {}         // can have methods
};

// JSON (string format)
const json = '{
    "name": "John",   // keys MUST have quotes
    "age": 30,
    "isStudent": false
}';
```

## Parsing and Stringifying

```javascript
// Stringify (object → JSON string)
const obj = { name: "John", age: 30 };
const jsonString = JSON.stringify(obj);
console.log(jsonString); // '{"name":"John","age":30}'

// Parse (JSON string → object)
const jsonString = '{"name":"John","age":30}';
const obj = JSON.parse(jsonString);
console.log(obj.name); // "John"
```

## Common Use Cases

```javascript
// Store in localStorage
localStorage.setItem("user", JSON.stringify(user));

// Retrieve from localStorage
const user = JSON.parse(localStorage.getItem("user"));

// Send to server
fetch("/api/user", {
    method: "POST",
    body: JSON.stringify(user),
    headers: { "Content-Type": "application/json" }
});

// Receive from server
const data = await response.json();
```

## Quick Revision

- [[JSON]] = data exchange format
- Keys must be strings (double quotes)
- No functions, no comments, no trailing commas
- `JSON.stringify()` = object to string
- `JSON.parse()` = string to object
- Use for: storage, API, data transfer

---

## Related Topics

- [[What-is-Object]] - Objects overview
- [[Parse-JSON]] - [[JSON]] parsing
- [[Create-Object]] - Creating objects
- [[What-is-API]] - APIs
- [[Make-HTTP]] - HTTP requests
