# Mutable vs Immutable in JavaScript

## Definition

**Mutable** values can be changed after creation, while **immutable** values cannot. In JavaScript, objects and arrays are mutable, while primitives (strings, numbers, booleans, null, undefined, symbols, BigInt) are immutable. Understanding mutability is crucial for avoiding unintended side effects and writing predictable code.

---

## Primitives are Immutable

```javascript
// Strings are immutable
let name = "John";
name.toUpperCase(); // Returns "JOHN" but doesn't change original
console.log(name); // "John"

// Numbers are immutable
let num = 5;
num.toFixed(2); // Returns "5.00" but doesn't change original
console.log(num); // 5

// Booleans are immutable
let flag = true;
// No method can change the value of true/false

// Template literals create new strings
let greeting = `Hello ${name}`; // New string, original unchanged
```

---

## Objects are Mutable

```javascript
// Objects can be modified after creation
const person = { name: "Alice", age: 30 };
person.age = 31; // Modifies existing object
person.city = "NYC"; // Adds new property
console.log(person); // { name: "Alice", age: 31, city: "NYC" }

// Delete properties
delete person.city;
console.log(person); // { name: "Alice", age: 31 }

// Methods modify the object
const arr = [1, 2, 3];
arr.push(4); // Modifies original array
console.log(arr); // [1, 2, 3, 4]
```

---

## Arrays are Mutable

```javascript
const numbers = [1, 2, 3];

// These methods mutate the original array
numbers.push(4); // Adds to end
numbers.pop(); // Removes from end
numbers.unshift(0); // Adds to beginning
numbers.shift(); // Removes from beginning
numbers.splice(1, 1); // Removes at index
numbers.sort(); // Sorts in place
numbers.reverse(); // Reverses in place

console.log(numbers); // Modified array
```

---

## Creating Immutable Copies

### Objects

```javascript
// Spread operator (shallow copy)
const original = { a: 1, b: { c: 2 } };
const copy = { ...original };

copy.a = 10; // Only changes copy
console.log(original.a); // 1 (unchanged)

// Note: nested objects are still mutable references
copy.b.c = 20;
console.log(original.b.c); // 20 (shared reference!)

// Deep copy
const deepCopy = JSON.parse(JSON.stringify(original));
// or structuredClone(original) in modern environments
```

### Arrays

```javascript
const original = [1, 2, 3, 4];

// Spread operator
const copy1 = [...original];

// Slice
const copy2 = original.slice();

// Array.from
const copy3 = Array.from(original);

// These don't mutate original
copy1.push(5);
copy2.push(6);
copy3.push(7);

console.log(original); // [1, 2, 3, 4]
```

---

## Object.freeze() - Shallow Immutability

```javascript
const user = { name: "Bob", address: { city: "Boston" } };
Object.freeze(user);

user.name = "Alice"; // Silently fails (or throws in strict mode)
user.age = 25; // Silently fails
console.log(user); // { name: "Bob", address: { city: "Boston" } }

// Note: freeze is shallow!
user.address.city = "NYC"; // This works!
console.log(user.address.city); // "NYC"

// Deep freeze
function deepFreeze(obj) {
  Object.freeze(obj);
  Object.keys(obj).forEach(key => {
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
  return obj;
}
```

---

## Object.seal() - No Adding/Deleting

```javascript
const car = { make: "Toyota", model: "Camry" };
Object.seal(car);

car.year = 2024; // Silently fails (can't add)
delete car.make; // Silently fails (can't delete)
car.model = "Corolla"; // Works (can modify existing)
console.log(car); // { make: "Toyota", model: "Corolla" }
```

---

## Spread Operator for Immutability

```javascript
// Updating objects immutably
const state = { count: 0, items: [] };

const newState = { ...state, count: state.count + 1 };
console.log(newState.count); // 1
console.log(state.count); // 0

// Updating arrays immutably
const addItem = (arr, item) => [...arr, item];
const removeItem = (arr, index) => arr.filter((_, i) => i !== index);
const updateItem = (arr, index, newItem) =>
  arr.map((item, i) => (i === index ? newItem : item));
```

---

## Common Use Cases

### React State Updates

```javascript
// Wrong: mutates state directly
state.items.push(newItem);
state.count++;

// Correct: immutable updates
setState({
  ...state,
  items: [...state.items, newItem],
  count: state.count + 1
});
```

### Avoiding Side Effects

```javascript
// Function with side effect (mutates input)
function addItemBad(arr, item) {
  arr.push(item); // Mutates original!
  return arr;
}

// Function without side effect (returns new array)
function addItemGood(arr, item) {
  return [...arr, item]; // New array
}

const original = [1, 2, 3];
addItemBad(original, 4); // original is now [1, 2, 3, 4]
addItemGood(original, 5); // original unchanged
```

### Immutable Data Structures

```javascript
// Using Map for immutable updates
const map1 = new Map([["key1", "value1"]]);
const map2 = map1.set("key2", "value2"); // Returns new Map
console.log(map1.size); // 1
console.log(map2.size); // 2

// Using Set for immutable updates
const set1 = new Set([1, 2, 3]);
const set2 = set1.add(4); // Returns new Set
console.log(set1.size); // 3
console.log(set2.size); // 4
```

---

## Common Mistakes

### Mistake 1: Assuming Primitives are Mutable

```javascript
let str = "hello";
str[0] = "H"; // No error, but doesn't work
console.log(str); // "hello"

// Must create new string
str = "H" + str.slice(1); // "Hello"
```

### Mistake 2: Shallow Copy of Nested Objects

```javascript
const original = { nested: { value: 1 } };
const copy = { ...original };

copy.nested.value = 99;
console.log(original.nested.value); // 99 (shared reference!)

// Solution: deep copy
const deepCopy = structuredClone(original);
```

### Mistake 3: Forgetting Array Methods Mutate

```javascript
const arr = [3, 1, 2];
arr.sort(); // Mutates original!
console.log(arr); // [1, 2, 3]

// Solution: copy first
const sorted = [...arr].sort((a, b) => a - b);
```

### Mistake 4: Using const with Mutable Objects

```javascript
const obj = { a: 1 };
obj = { b: 2 }; // Error: can't reassign const

// But you CAN modify the object's properties
obj.a = 10; // Works! const only prevents reassignment
```

---

## Performance Considerations

```javascript
// Mutating is faster (no new memory allocation)
const arr1 = [];
for (let i = 0; i < 1000000; i++) {
  arr1.push(i); // Fast
}

// Immutable copying can be expensive
let arr2 = [];
for (let i = 0; i < 1000000; i++) {
  arr2 = [...arr2, i]; // Very slow! Creates new array each time
}

// Better immutable pattern
const arr3 = [];
for (let i = 0; i < 1000000; i++) {
  arr3.push(i);
}
const immutableCopy = [...arr3]; // Copy once at the end
```

---

## Quick Revision Summary

| Type | Mutable? | Example |
|------|----------|---------|
| String | No | `"hello"` |
| Number | No | `42` |
| Boolean | No | `true` |
| Object | Yes | `{ a: 1 }` |
| Array | Yes | `[1, 2, 3]` |
| Map/Set | Yes | `new Map()` |

| Method | Effect |
|--------|--------|
| `Object.freeze()` | Shallow immutability |
| `Object.seal()` | Can't add/delete properties |
| `{ ...obj }` | Shallow copy |
| `structuredClone()` | Deep copy |
| `[...arr]` | Array copy |

---

## Related Topics

- [[Object]] - Working with mutable objects
- [[Array-Access]] - Modifying array elements
- [[const]] - const doesn't make objects immutable
- [[Spread-Operator]] - Creating immutable copies
- [[Object-Destructuring]] - Immutable destructuring patterns
- [[Closures]] - Capturing mutable state
- [[this]] - Mutable `this` context