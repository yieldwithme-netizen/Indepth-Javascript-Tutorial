# What is a [[for]]...[[in]] [[loop]]?

## Definition

A [[for]]...[[in]] [[loop]] **iterates over [[object]] properties** (keys).

## Basic Syntax

```javascript
for (let key in object) {
    // code using key
}
```

## Examples

```javascript
// Loop through object properties
const person = {
    name: "John",
    age: 30,
    city: "New York"
};

for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}
// Output:
// name: John
// age: 30
// city: New York

// With hasOwnProperty (recommended)
for (let key in person) {
    if (person.hasOwnProperty(key)) {
        console.log(`${key}: ${person[key]}`);
    }
}

// Loop through array indices
const fruits = ["apple", "banana", "orange"];
for (let index in fruits) {
    console.log(`${index}: ${fruits[index]}`);
}
// Output:
// 0: apple
// 1: banana
// 2: orange
```

## [[for]]...[[in]] vs [[for]]...[[of]]

```javascript
// for...in: iterates over KEYS/INDICES
const arr = ["a", "b", "c"];
for (let x in arr) {
    console.log(x); // "0", "1", "2" (indices)
}

// for...of: iterates over VALUES
for (let x of arr) {
    console.log(x); // "a", "b", "c" (values)
}
```

## When to Use

```javascript
// ✅ Use for...in for:
// - Object properties
// - When you need the key
for (let key in obj) {
    console.log(key, obj[key]);
}

// ❌ Don't use for...in for:
// - Arrays (use for...of or forEach)
// - When order matters (for...in doesn't guarantee order)
```

## Quick Revision

- [[for]]...[[in]] iterates over [[object]] keys
- Use `obj[key]` to access values
- Use `hasOwnProperty()` to avoid inherited properties
- Don't use for [[array|array]] (use [[for]]...[[of]] instead)
- Order not guaranteed for [[object|objects]]

---

## Related Topics

- [[What-is-ForOf]] - [[for]]...[[of]] loop
- [[What-is-ForLoop]] - Traditional [[for]] loop
- [[Use-ForIn]] - Using [[for]]...[[in]]
- [[Iterate-Objects]] - [[object|Object]] iteration
