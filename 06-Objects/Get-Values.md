# How to Use Object.values()

`Object.values()` returns an array of a given object's **own enumerable property values**.

## Basic Syntax

```javascript
Object.values(object);
```

## Basic Examples

```javascript
const person = {
  name: "John",
  age: 30,
  city: "New York"
};

const values = Object.values(person);
console.log(values); // ["John", 30, "New York"]
```

## Iterating Over Values

```javascript
const scores = {
  math: 95,
  science: 88,
  english: 92
};

// Using for...of
for (const value of Object.values(scores)) {
  console.log(value);
}

// Using forEach
Object.values(scores).forEach(value => {
  console.log(value);
});
```

## Summing Values

```javascript
const prices = {
  apple: 1.5,
  banana: 0.75,
  orange: 2.0
};

const total = Object.values(prices).reduce((sum, price) => sum + price, 0);
console.log(total); // 4.25
```

## Filtering Values

```javascript
const inventory = {
  apples: 50,
  bananas: 0,
  oranges: 25,
  grapes: 0
};

const inStock = Object.values(inventory).filter(count => count > 0);
console.log(inStock); // [50, 25]
```

## Common Use Cases

- Extracting all values from an object
- Performing calculations on values
- Filtering and transforming data
- Working with API responses

## Common Mistakes

```javascript
const obj = { a: 1, b: 2, c: 3 };

// ❌ Forgetting it returns an array
const values = Object.values(obj);
// console.log(values[0]); // 1 (works, but don't treat as object)

// ❌ Not handling nested objects
const nested = { x: { y: 1 } };
const flatValues = Object.values(nested);
console.log(flatValues); // [{ y: 1 }] (still an object, not flattened)

// ✅ Use flatMap or recursion for deep flattening

// ❌ Using on non-objects
// Object.values("string"); // TypeError
```

## Related Topics

- [[Get-Keys]]
- [[Iterate-Objects]]
- [[Define-Objects]]
- [[Array-Methods]]

## Quick Revision

| Method | Returns | Example |
|--------|---------|---------|
| `Object.values(obj)` | Array of values | `[1, 2, 3]` |
| `Object.keys(obj)` | Array of keys | `["a", "b", "c"]` |
| `Object.entries(obj)` | Array of pairs | `[["a", 1], ["b", 2]]` |

**Key Point:** `Object.values()` is perfect for extracting and working with all values in an object.