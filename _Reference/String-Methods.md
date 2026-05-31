# String Methods

## Definition
String methods are built-in functions in JavaScript that manipulate and work with string values. They allow you to search, extract, transform, and format text data.

## Creating Strings
```javascript
const str1 = "Hello";          // String literal
const str2 = 'World';          // Single quotes
const str3 = `Template`;       // Template literal
const str4 = new String("Hi"); // String object (avoid)
```

## Common String Methods

### Case Methods
```javascript
const str = "Hello World";

console.log(str.toUpperCase());  // "HELLO WORLD"
console.log(str.toLowerCase());  // "hello world"

// Convert to title case
const titleCase = str.split(' ')
  .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
  .join(' ');
console.log(titleCase);  // "Hello World"
```

### Search Methods
```javascript
const str = "Hello World, Hello JavaScript";

console.log(str.indexOf("Hello"));        // 0
console.log(str.indexOf("Hello", 1));     // 13
console.log(str.lastIndexOf("Hello"));    // 13
console.log(str.includes("World"));       // true
console.log(str.startsWith("Hello"));     // true
console.log(str.endsWith("JavaScript"));  // true
console.log(str.search(/world/i));        // 6
console.log(str.match(/Hello/g));         // ["Hello", "Hello"]
console.log(str.matchAll(/Hello/g));      // Iterator with matches
```

### Extract Methods
```javascript
const str = "Hello World";

console.log(str.slice(0, 5));      // "Hello"
console.log(str.slice(-5));        // "World"
console.log(str.substring(0, 5));  // "Hello"
console.log(str.substr(0, 5));     // "Hello" (deprecated)
console.log(str.charAt(0));        // "H"
console.log(str.charCodeAt(0));    // 72
console.log(str.at(-1));           // "d"
```

### Trim Methods
```javascript
const str = "  Hello World  ";

console.log(str.trim());       // "Hello World"
console.log(str.trimStart());  // "Hello World  "
console.log(str.trimEnd());    // "  Hello World"
```

### Replace Methods
```javascript
const str = "Hello World";

console.log(str.replace("World", "JS"));      // "Hello JS"
console.log(str.replaceAll("l", "L"));         // "HeLLo WorLd"
console.log(str.replace(/l/g, "L"));           // "HeLLo WorLd"
```

### Split and Join
```javascript
const str = "Hello World";

// Split string to array
const words = str.split(" ");
console.log(words);  // ["Hello", "World"]

// Join array to string
const joined = words.join("-");
console.log(joined);  // "Hello-World"
```

### Repeat and Pad
```javascript
console.log("Ha".repeat(3));           // "HaHaHa"
console.log("5".padStart(3, "0"));     // "005"
console.log("5".padEnd(3, "0"));       // "500"
console.log("Hello".padStart(10, ".")); // ".....Hello"
```

### Template Literals
```javascript
const name = "John";
const age = 25;

// String interpolation
const greeting = `Hello, ${name}! You are ${age} years old.`;

// Multiline strings
const multiLine = `
  First line
  Second line
  Third line
`;

// Expressions in templates
const calc = `Sum: ${2 + 3}`;
```

## Common Use Cases
- Data validation (email, phone, etc.)
- Formatting user input
- Parsing URL parameters
- Displaying formatted content
- Data transformation

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Strings are immutable | Methods return new strings |
| Confusing indexOf/find | indexOf returns position |
| Using replace with string | Use regex for global replace |
| Not handling case sensitivity | Use toLowerCase()/toUpperCase() |

## Quick Revision Summary
- Strings are immutable; methods return new strings
- Use template literals for interpolation and multiline
- Search with indexOf, includes, search, match
- Extract with slice, substring, charAt
- Transform with replace, split, join, trim
- Pad with padStart and padEnd

## Related Topics
- [[Template-Literals]]
- [[Regular-Expressions]]
- [[Arrays]]
- [[Loops]]
- [[Type-Conversion]]
- [[Template-Strings]]
