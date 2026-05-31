# How to Access Object Properties

**Object properties** can be accessed using two methods: **dot notation** and **bracket notation**.

## Dot Notation

```javascript
const person = {
  name: "John",
  age: 30
};

console.log(person.name); // "John"
console.log(person.age);  // 30
```

## Bracket Notation

```javascript
const person = {
  name: "John",
  age: 30
};

console.log(person["name"]); // "John"
console.log(person["age"]);  // 30
```

## When to Use Bracket Notation

```javascript
const user = {
  "first-name": "John",
  "last-name": "Doe",
  123: "number key"
};

// ❌ Dot notation won't work with special characters
// user.first-name; // SyntaxError

// ✅ Bracket notation works
console.log(user["first-name"]); // "John"

// ✅ Dynamic property access
const key = "name";
console.log(person[key]); // undefined (no "name" property directly)

const prop = "age";
console.log(person[prop]); // 30
```

## Accessing Nested Properties

```javascript
const company = {
  name: "TechCorp",
  address: {
    city: "New York",
    zip: "10001"
  }
};

console.log(company.address.city);     // "New York"
console.log(company["address"]["zip"]); // "10001"

// Mixed notation
console.log(company.address["city"]);  // "New York"
```

## Common Use Cases

- Dynamic property names
- Properties with special characters
- Accessing computed keys
- Working with API responses

## Common Mistakes

```javascript
const obj = { key: "value" };

// ❌ Accessing non-existent property
console.log(obj.nonExistent); // undefined

// ✅ Check if property exists
console.log("key" in obj);            // true
console.log(obj.hasOwnProperty("key")); // true

// ❌ Using dot notation with variables
const propName = "key";
// console.log(obj.propName); // undefined (looks for "propName" property)

// ✅ Use bracket notation with variables
console.log(obj[propName]); // "value"
```

## Related Topics

- [[Define-Objects]]
- [[Object-Methods]]
- [[Get-Keys]]
- [[Get-Values]]
- [[Iterate-Objects]]

## Quick Revision

| Method | Syntax | Use Case |
|--------|--------|----------|
| Dot | `obj.property` | Known, valid identifier |
| Bracket | `obj["property"]` | Dynamic or special characters |

**Key Point:** Use dot notation for simple access; bracket notation for dynamic or special property names.