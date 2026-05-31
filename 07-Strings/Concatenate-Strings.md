# How to Concatenate Strings

**String concatenation** is combining two or more strings into one. JavaScript provides multiple ways to do this.

## Using + Operator

```javascript
const first = "Hello";
const second = "World";
const result = first + " " + second;
console.log(result); // "Hello World"
```

## Using += Operator

```javascript
let message = "Hello";
message += " ";
message += "World";
console.log(message); // "Hello World"
```

## Using Template Literals (Recommended)

```javascript
const name = "John";
const age = 30;
const greeting = `Hello, ${name}! You are ${age} years old.`;
console.log(greeting); // "Hello, John! You are 30 years old."
```

## Using concat() Method

```javascript
const str1 = "Hello";
const str2 = " ";
const str3 = "World";

const result = str1.concat(str2, str3);
console.log(result); // "Hello World"

// With arrays
const words = ["Hello", " ", "World"];
console.log(words.join("")); // "Hello World"
```

## Joining Arrays

```javascript
const words = ["JavaScript", "is", "fun"];
const sentence = words.join(" ");
console.log(sentence); // "JavaScript is fun"

// With custom separator
const csv = ["apple", "banana", "orange"].join(", ");
console.log(csv); // "apple, banana, orange"
```

## Common Use Cases

- Building user messages
- Creating file paths
- Generating HTML content
- Logging and debugging

## Common Mistakes

```javascript
// ❌ String concatenation with numbers
const num = 42;
const result1 = "The answer is " + num; // "The answer is 42" (works)
const result2 = "The answer is " + num + "!"; // "The answer is 42!"

// ⚠️ Unexpected behavior with + operator
const result3 = "5" + 3;   // "53" (string)
const result4 = "5" - 3;   // 2 (number, subtraction)
const result5 = "5" * 2;   // 10 (number, multiplication)

// ✅ Use template literals for clarity
const result6 = `The answer is ${num}!`;

// ❌ Performance issue with loop concatenation
let longString = "";
for (let i = 0; i < 10000; i++) {
  longString += "a"; // Creates new string each iteration
}

// ✅ Use array join for large concatenations
const parts = [];
for (let i = 0; i < 10000; i++) {
  parts.push("a");
}
const efficient = parts.join(""); // More efficient

// ❌ Forgetting spaces
const a = "Hello";
const b = "World";
console.log(a + b); // "HelloWorld" (missing space)

// ✅ Add explicit spaces
console.log(a + " " + b); // "Hello World"
```

## Related Topics

- [[Create-String]]
- [[Access-Characters]]
- [[String-Methods]]
- [[Template-Literals]]
- [[Array-Methods]]

## Quick Revision

| Method | Syntax | Best For |
|--------|--------|----------|
| `+` operator | `a + b` | Simple concatenation |
| `+=` operator | `a += b` | Building strings |
| Template literals | `` `${a} ${b}` `` | Complex interpolation |
| `concat()` | `a.concat(b)` | Multiple strings |
| `join()` | `arr.join("")` | Array to string |

**Key Point:** Use template literals for readable interpolation; use `join()` for efficient array concatenation.