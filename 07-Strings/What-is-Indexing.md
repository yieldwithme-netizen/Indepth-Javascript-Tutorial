# What is String Indexing in JavaScript?

**Indexing** is how you access individual characters in a string. Each character has a position (index) starting from 0.

## Definition

String indexing allows you to retrieve or reference specific characters within a string using their position number.

## How Indexing Works

```javascript
let str = "Hello";
// Index:  01234

// Access first character
console.log(str[0]);    // "H"

// Access last character
console.log(str[4]);    // "o"

// Access middle character
console.log(str[2]);    // "l"
```

## Index Properties

### Zero-Based Indexing

```javascript
let text = "JavaScript";
// Index:  0123456789

console.log(text[0]);   // "J"
console.log(text[9]);   // "t"
console.log(text[10]);  // undefined (out of bounds)
```

### Last Index

```javascript
let str = "Hello";
let lastIndex = str.length - 1;
console.log(str[lastIndex]);  // "o"

// Alternative: using at() method (ES2022)
console.log(str.at(-1));      // "o"
console.log(str.at(-2));      // "l"
```

## Using charAt() Method

```javascript
let str = "World";

// Using bracket notation
console.log(str[0]);        // "W"

// Using charAt() method
console.log(str.charAt(0)); // "W"

// charAt() returns empty string for invalid index
console.log(str.charAt(10)); // ""
```

## Accessing Multiple Characters

```javascript
let str = "JavaScript";

// Using slice
console.log(str.slice(0, 4));  // "Java"

// Using substring
console.log(str.substring(0, 4)); // "Java"

// Using bracket notation in loop
for (let i = 0; i < 4; i++) {
    process.stdout.write(str[i]);
}
// Output: "Java"
```

## Common Use Cases

### 1. Validating First Character

```javascript
let username = "John123";
if (username[0] >= '0' && username[0] <= '9') {
    console.log("Username cannot start with a number");
}
```

### 2. Checking File Extension

```javascript
let filename = "document.pdf";
let lastDot = filename.lastIndexOf(".");
let extension = filename.slice(lastDot + 1);
console.log(extension);  // "pdf"
```

### 3. Capitalizing First Letter

```javascript
let word = "hello";
let capitalized = word[0].toUpperCase() + word.slice(1);
console.log(capitalized);  // "Hello"
```

## Common Mistakes to Avoid

1. **Forgetting zero-based indexing** - first index is 0, not 1
2. **Not checking string length** before accessing high indices
3. **Confusing `[]` with `charAt()`** - bracket returns undefined, charAt returns empty string
4. **Using negative indices** with bracket notation (use `at()` instead)

## Related Topics

- [[What-is-String]] - String basics
- [[What-is-Immutability]] - Why strings can't be changed
- [[What-is-Slice]] - Extract substrings
- [[What-is-Methods]] - Common string methods
- [[What-is-Loops]] - Looping through strings

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| Indexing | Access characters by position |
| Zero-based | First index is 0 |
| Bracket notation | `str[0]` |
| charAt() | `str.charAt(0)` |
| Last character | `str[str.length - 1]` or `str.at(-1)` |
| Out of bounds | `undefined` (bracket) or `""` (charAt) |
