# How to Create Strings

**Strings** represent sequences of characters. JavaScript provides multiple ways to create strings.

## String Literals (Recommended)

```javascript
const single = 'Hello, World!';
const double = "Hello, World!";
const backtick = `Hello, World!`;

console.log(single); // "Hello, World!"
console.log(double); // "Hello, World!"
console.log(backtick); // "Hello, World!"
```

## String Constructor

```javascript
const str1 = new String("Hello");
const str2 = String("Hello");

console.log(typeof str1); // "object" (wrapper object)
console.log(typeof str2); // "string" (primitive)
```

## Template Literals (ES6+)

```javascript
const name = "John";
const age = 30;

// Variable interpolation
const greeting = `Hello, ${name}!`;
console.log(greeting); // "Hello, John!"

// Expressions
const message = `I am ${age} years old in ${2024 - age}`;
console.log(message); // "I am 30 years old in 1994"

// Multi-line strings
const multiLine = `
  This is
  a multi-line
  string
`;
console.log(multiLine);
```

## Escape Characters

```javascript
const escaped = "He said, \"Hello!\"";
console.log(escaped); // He said, "Hello!"

const newline = "Line 1\nLine 2";
console.log(newline);
// Line 1
// Line 2

const tab = "Column1\tColumn2";
console.log(tab); // Column1	Column2
```

## String from Character Codes

```javascript
const char = String.fromCharCode(65);
console.log(char); // "A"

const emoji = String.fromCodePoint(128512);
console.log(emoji); // 😀
```

## Common Use Cases

- Displaying text
- Storing user input
- Building HTML/CSS
- API communication

## Common Mistakes

```javascript
// ❌ Using String constructor for primitives
const str = String(123); // "123" (string, but unnecessary)
const better = 123.toString(); // "123"

// ❌ Confusing string and String object
const primitive = "hello";
const object = new String("hello");

console.log(primitive === object); // false (different types)
console.log(primitive == object);  // true (loose equality)

// ✅ Always use string literals
const correct = "hello";

// ❌ Not handling special characters
const path = "C:\new\folder"; // Error: \n and \f are escape sequences

// ✅ Use double backslash or raw strings
const correctPath = "C:\\new\\folder";
const rawPath = String.raw`C:\new\folder`;
```

## Related Topics

- [[Access-Characters]]
- [[Concatenate-Strings]]
- [[String-Methods]]
- [[Template-Literals]]
- [[Regular-Expressions]]

## Quick Revision

| Method | Syntax | Type |
|--------|--------|------|
| Single quotes | `'text'` | Primitive |
| Double quotes | `"text"` | Primitive |
| Backticks | `` `text` `` | Primitive |
| Template literal | `` `Hello ${name}` `` | Primitive |
| Constructor | `new String("text")` | Object (avoid) |

**Key Point:** Use string literals or template literals; avoid the `String` constructor for creating strings.