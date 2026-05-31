# Strings in JavaScript

## Definition

Strings are sequences of characters used to represent text. In JavaScript, strings are immutable primitives enclosed in single quotes (`'`), double quotes (`"`), or backticks (`` ` `` for template literals).

## Creating Strings

```javascript
// Different ways to create strings
const single = 'Hello';
const double = "Hello";
const template = `Hello`;

// From characters
const fromChars = String.fromCharCode(72, 101, 108, 108, 111); // "Hello"

// String object (avoid using)
const obj = new String('Hello'); // typeof returns "object"
```

## Template Literals

```javascript
const name = 'John';
const age = 30;

// String interpolation
const greeting = `Hello, my name is ${name} and I'm ${age} years old.`;

// Multi-line strings
const multiline = `
  This is line 1
  This is line 2
  This is line 3
`;

// Expressions
const price = 10;
const quantity = 3;
const total = `Total: $${price * quantity}`;

// Nested template literals
const html = `
  <div>
    <h1>${name}</h1>
    <p>Age: ${age}</p>
  </div>
`;
```

## String Properties

```javascript
const str = "Hello, World!";

console.log(str.length); // 13
console.log(str[0]); // "H"
console.log(str[str.length - 1]); // "!"
```

## Case Methods

```javascript
const str = "Hello World";

console.log(str.toUpperCase()); // "HELLO WORLD"
console.log(str.toLowerCase()); // "hello world"

// Case conversion
const name = "johN dOE";
console.log(name.charAt(0).toUpperCase() + name.slice(1).toLowerCase());
// "John doe"
```

## Search Methods

```javascript
const str = "Hello, World! Hello, JavaScript!";

// indexOf - Returns index or -1
console.log(str.indexOf("Hello")); // 0
console.log(str.indexOf("Hello", 1)); // 14 (start from index 1)
console.log(str.indexOf("xyz")); // -1

// lastIndexOf - Returns last occurrence
console.log(str.lastIndexOf("Hello")); // 14

// includes - Returns boolean
console.log(str.includes("World")); // true
console.log(str.includes("world")); // false (case-sensitive)

// startsWith / endsWith
console.log(str.startsWith("Hello")); // true
console.log(str.endsWith("!")); // true
```

## Extraction Methods

```javascript
const str = "Hello, World!";

// slice - Extracts section
console.log(str.slice(0, 5)); // "Hello"
console.log(str.slice(7)); // "World!"
console.log(str.slice(-6)); // "orld!"

// substring - Similar to slice
console.log(str.substring(0, 5)); // "Hello"

// substr (deprecated)
console.log(str.substr(7, 5)); // "World"
```

## Modify Methods

```javascript
const str = "  Hello, World!  ";

// trim - Remove whitespace
console.log(str.trim()); // "Hello, World!"
console.log(str.trimStart()); // "Hello, World!  "
console.log(str.trimEnd()); // "  Hello, World!"

// replace - Replace first occurrence
console.log("Hello World".replace("World", "JavaScript"));
// "Hello JavaScript"

// replaceAll - Replace all occurrences
console.log("Hello World".replaceAll("l", "L"));
// "HeLLo WorLd"

// padStart / padEnd
console.log("5".padStart(3, "0")); // "005"
console.log("5".padEnd(3, "0")); // "500"

// repeat
console.log("Ha".repeat(3)); // "HaHaHa"
```

## Split and Join

```javascript
// split - String to array
const str = "Hello, World, JavaScript";
const words = str.split(", ");
console.log(words); // ["Hello", "World", "JavaScript"]

// join (from array) - Array to string
const joined = words.join(" & ");
console.log(joined); // "Hello & World & JavaScript"

// Common patterns
const csv = "name,age,city";
const fields = csv.split(",");
console.log(fields); // ["name", "age", "city"]
```

## Comparison Methods

```javascript
const str1 = "apple";
const str2 = "Banana";

// localeCompare - Returns -1, 0, or 1
console.log(str1.localeCompare(str2)); // -1 (comes before)
console.log(str2.localeCompare(str1)); // 1 (comes after)

// Sorting strings
const fruits = ["banana", "apple", "cherry"];
fruits.sort((a, b) => a.localeCompare(b));
console.log(fruits); // ["apple", "banana", "cherry"]
```

## Regular Expressions

```javascript
const str = "Hello, World! 123";

// test - Check pattern match
console.log(/\d+/.test(str)); // true (has digits)

// match - Get matches
console.log(str.match(/\d+/)); // ["123"]

// matchAll - Get all matches with details
const str2 = "test1 test2 test3";
const matches = [...str2.matchAll(/test(\d)/g)];
console.log(matches.length); // 3

// search - Find index of pattern
console.log(str.search(/\d+/)); // 14
```

## Common Use Cases

### URL Manipulation

```javascript
const url = "https://example.com/path?name=John&age=30";

// Extract query parameters
const queryString = url.split("?")[1];
const params = new URLSearchParams(queryString);
console.log(params.get("name")); // "John"

// Modify URL
const newUrl = url.replace("https", "http");
```

### String Validation

```javascript
function validateEmail(email) {
  const pattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return pattern.test(email);
}

console.log(validateEmail("user@example.com")); // true
console.log(validateEmail("invalid-email")); // false
```

### Formatting

```javascript
// Capitalize first letter
function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

// Truncate with ellipsis
function truncate(str, maxLength) {
  if (str.length <= maxLength) return str;
  return str.slice(0, maxLength - 3) + "...";
}

console.log(capitalize("hello")); // "Hello"
console.log(truncate("This is a long string", 10)); // "This is..."
```

## Common Mistakes

```javascript
const str = "Hello";

// ❌ Wrong: Strings are immutable
// str[0] = "h"; // No error but doesn't work

// ✅ Correct: Create new string
const newStr = "h" + str.slice(1);

// ❌ Wrong: Confusing slice parameters
console.log(str.slice(1, 4)); // "ell" (not "elle")

// ❌ Wrong: Not handling case sensitivity
console.log("Hello".indexOf("hello")); // -1

// ✅ Correct: Convert case first
console.log("Hello".toLowerCase().indexOf("hello")); // 0

// ❌ Wrong: Assuming replace changes original
const original = "Hello World";
const replaced = original.replace("World", "JavaScript");
console.log(original); // "Hello World" (unchanged)
```

## String vs Array

```javascript
const str = "Hello";
const arr = ['H', 'e', 'l', 'l', 'o'];

// Similarities
console.log(str.length); // 5
console.log(arr.length); // 5

console.log(str[0]); // "H"
console.log(arr[0]); // "H"

// Key difference: Strings are immutable
arr[0] = 'h'; // Works
// str[0] = 'h'; // No effect

// Convert between them
const strToArray = str.split('');
const arrToString = arr.join('');
```

## Related Topics

- [[String-Methods-Overview]] - Complete method reference
- [[Template-Literals]] - Advanced template strings
- [[Regular-Expressions]] - Pattern matching
- [[Array-Methods]] - Array manipulation
- [[JSON]] - Data serialization
- [[String-Manipulation]] - Common operations

## Quick Revision

**Common Methods:**
| Method | Purpose |
|--------|---------|
| `toUpperCase()/toLowerCase()` | Case conversion |
| `indexOf()/includes()` | Search |
| `slice()/substring()` | Extract |
| `trim()` | Remove whitespace |
| `replace()/replaceAll()` | Find & replace |
| `split()/join()` | Array conversion |
| `repeat()` | Repeat string |
| `padStart()/padEnd()` | Padding |

**Key Points:**
- Strings are immutable primitives
- Template literals enable interpolation
- `slice()` is generally preferred over `substring()`
- `replaceAll()` is modern alternative to regex replace
- Always consider case sensitivity in searches