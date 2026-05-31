# What is a String in JavaScript?

A **string** is a sequence of characters used to represent text. In JavaScript, strings are one of the most commonly used data types.

## Definition

A string is a primitive data type that stores textual data. Strings can contain letters, numbers, spaces, symbols, and special characters.

## Creating Strings

There are three ways to create strings in JavaScript:

### 1. String Literal (Recommended)

```javascript
let name = "John";
let greeting = 'Hello World';
let sentence = `This is a template literal`;
```

### 2. String Constructor

```javascript
let str = new String("Hello");
let numStr = new String(123);
```

### 3. Template Literals (ES6)

```javascript
let name = "Alice";
let message = `Hello, ${name}!`;
let multiLine = `
  This is line 1
  This is line 2
`;
```

## String Properties

### Length Property

```javascript
let text = "Hello World";
console.log(text.length);  // 11

let empty = "";
console.log(empty.length); // 0
```

## Immutability

Strings in JavaScript are **immutable** - once created, they cannot be changed.

```javascript
let str = "Hello";
str[0] = "J";        // This won't work
console.log(str);     // "Hello" (unchanged)

// To "change" a string, create a new one
str = "J" + str.slice(1);
console.log(str);     // "Jello"
```

## String vs String Object

```javascript
// Primitive string
let primitive = "Hello";
let primitive2 = String("Hello");

// String object
let obj = new String("Hello");

// Type checking
typeof primitive;   // "string"
typeof obj;         // "object"

// Comparison
primitive == obj;   // true (value comparison)
primitive === obj;  // false (different types)
```

## Common Use Cases

1. **Displaying text** on web pages
2. **Storing user input** like names, emails
3. **Data validation** and formatting
4. **URL and path manipulation**
5. **Template generation** for dynamic content

## Common Mistakes to Avoid

1. **Using `new String()` unnecessarily** - always prefer string literals
2. **Confusing `==` and `===`** when comparing strings
3. **Forgetting string immutability** - trying to modify strings directly
4. **Not handling special characters** properly in strings

## Related Topics

- [[What-is-Indexing]] - Access characters in strings
- [[What-is-Immutability]] - Why strings can't be changed
- [[What-is-Methods]] - Common string methods
- [[What-is-Slice]] - Extract substrings
- [[What-is-Template-Literals]] - Template literals (ES6)

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| String | Sequence of characters representing text |
| Creation | Use literals `"text"` or `'text'` |
| Length | `string.length` property |
| Immutability | Cannot be changed after creation |
| Template Literals | Use backticks for interpolation |
| Avoid | Using `new String()` constructor |
| Comparison | Use `===` for strict comparison |
