# How to Iterate Over Objects

**Iterating over objects** means accessing each property (key-value pair) in an object. JavaScript provides several ways to do this.

## Using for...in Loop

```javascript
const person = {
  name: "John",
  age: 30,
  city: "New York"
};

for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}

// Output:
// name: John
// age: 30
// city: New York
```

## Using Object.keys()

```javascript
const car = {
  make: "Toyota",
  model: "Camry",
  year: 2020
};

Object.keys(car).forEach(key => {
  console.log(`${key}: ${car[key]}`);
});
```

## Using Object.values()

```javascript
const prices = {
  apple: 1.5,
  banana: 0.75,
  orange: 2.0
};

Object.values(prices).forEach(value => {
  console.log(value);
});
```

## Using Object.entries()

```javascript
const user = {
  name: "Alice",
  email: "alice@example.com",
  age: 25
};

for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
```

## Iterating with Array Methods

```javascript
const scores = { math: 95, science: 88, english: 92 };

// Filter
const highScores = Object.entries(scores).filter(([key, value]) => value > 90);
console.log(highScores); // [["math", 95], ["english", 92]]

// Map
const doubled = Object.values(scores).map(score => score * 2);
console.log(doubled); // [190, 176, 184]

// Reduce
const total = Object.values(scores).reduce((sum, score) => sum + score, 0);
console.log(total); // 275
```

## Common Use Cases

- Displaying object data
- Processing form data
- Working with API responses
- Data transformation

## Common Mistakes

```javascript
const obj = { a: 1, b: 2, c: 3 };

// ❌ Using for...in on arrays (use for...of instead)
const arr = [10, 20, 30];
for (const index in arr) {
  console.log(index); // "0", "1", "2" (strings, not numbers)
}

// ❌ Not checking for inherited properties
function Parent() {
  this.name = "parent";
}
Parent.prototype.inherited = "value";
const child = new Parent();

for (const key in child) {
  console.log(key); // "name", "inherited" (includes prototype)
}

// ✅ Use hasOwnProperty or Object.keys()
for (const key in child) {
  if (child.hasOwnProperty(key)) {
    console.log(key); // only "name"
  }
}
```

## Related Topics

- [[Get-Keys]]
- [[Get-Values]]
- [[Access-Properties]]
- [[Array-Methods]]
- [[For-Loop]]

## Quick Revision

| Method | Best For | Returns |
|--------|----------|---------|
| `for...in` | Key-based iteration | Keys (includes prototype) |
| `Object.keys()` | Array of keys | Array |
| `Object.values()` | Array of values | Array |
| `Object.entries()` | Key-value pairs | Array of arrays |

**Key Point:** Use `Object.entries()` for clean key-value iteration; use `for...in` with `hasOwnProperty` check.