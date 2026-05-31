# Higher-Order Functions

## Definition
A higher-order function is a function that either takes one or more functions as arguments, or returns a function as its result. They are a fundamental concept in functional programming.

## Basic Examples

### Function as Argument
```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

function processUser(name, callback) {
  return callback(name);
}

console.log(processUser('John', greet)); // 'Hello, John!'
```

### Function as Return Value
```javascript
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

## Built-in Higher-Order Functions

### Array Methods
```javascript
const numbers = [1, 2, 3, 4, 5];

// map - transforms each element
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - selects elements meeting condition
const evens = numbers.filter(n => n % 2 === 0);
console.log(evens); // [2, 4]

// reduce - accumulates into single value
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15

// forEach - executes function for each element
numbers.forEach(n => console.log(n));

// find - returns first matching element
const found = numbers.find(n => n > 3);
console.log(found); // 4

// some - checks if any element matches
const hasEven = numbers.some(n => n % 2 === 0);
console.log(hasEven); // true

// every - checks if all elements match
const allPositive = numbers.every(n => n > 0);
console.log(allPositive); // true
```

### Practical Examples

### Chaining Operations
```javascript
const users = [
  { name: 'John', age: 25, active: true },
  { name: 'Jane', age: 30, active: false },
  { name: 'Bob', age: 35, active: true }
];

const activeUserNames = users
  .filter(user => user.active)
  .map(user => user.name)
  .join(', ');

console.log(activeUserNames); // 'John, Bob'
```

### Custom Sorting
```javascript
const fruits = ['banana', 'apple', 'cherry', 'date'];

// Sort by length
const byLength = fruits.sort((a, b) => a.length - b.length);
console.log(byLength); // ['date', 'apple', 'banana', 'cherry']

// Sort objects
const people = [
  { name: 'John', age: 30 },
  { name: 'Jane', age: 25 },
  { name: 'Bob', age: 35 }
];

const sortedByAge = people.sort((a, b) => a.age - b.age);
```

## Common Use Cases
- Data transformation and manipulation
- Event handling and callbacks
- Array processing and filtering
- Creating function factories
- Implementing decorators/middleware
- Functional composition

## Common Mistakes
- Not returning values in callback functions
- Side effects in pure higher-order functions
- Forgetting to chain return values
- Using wrong methods (e.g., map vs forEach)
- Not handling undefined/null values

## Quick Revision Summary
- Higher-order functions take or return functions
- Built-in: map, filter, reduce, forEach, find, some, every
- Enable functional programming patterns
- Great for data transformation pipelines
- Improve code readability and reusability

## Related Topics
- [[Callbacks]]
- [[Functions]]
- [[Arrays]]
- [[Closures-and-Scope]]
- [[Arrow-Functions]]
