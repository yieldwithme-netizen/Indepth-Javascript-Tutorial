# String Manipulation

## Definition

String manipulation involves **modifying and transforming strings** in JavaScript.

## Concatenation

```javascript
// Using +
const greeting = "Hello" + " " + "World";

// Using template literals
const name = "John";
const greeting = `Hello, ${name}!`;

// Using concat
const str1 = "Hello";
const str2 = "World";
const greeting = str1.concat(" ", str2);
```

## Extracting

```javascript
const str = "Hello World";

// slice
str.slice(0, 5);    // "Hello"
str.slice(-5);      // "World"

// substring
str.substring(0, 5); // "Hello"

// split
str.split(" ");      // ["Hello", "World"]
```

## Transforming

```javascript
const str = "Hello World";

str.toUpperCase();   // "HELLO WORLD"
str.toLowerCase();   // "hello world"
str.trim();          // "Hello World"
str.replace("World", "JS"); // "Hello JS"
str.replaceAll("l", "L");  // "HeLLo WorLd"
```

## Searching

```javascript
const str = "Hello World";

str.includes("World");  // true
str.startsWith("Hello"); // true
str.endsWith("World");   // true
str.indexOf("World");    // 6
str.lastIndexOf("l");    // 9
```

## Quick Revision

- Concatenation: +, template literals, concat()
- Extracting: slice(), substring(), split()
- Transforming: toUpperCase, toLowerCase, trim, replace
- Searching: includes, startsWith, endsWith, indexOf

---

## Related Topics

- [[What-is-String]] - [[What-is-String|Strings]]
- [[String-Methods]] - [[String-Methods|String methods]]
- [[String-Methods-Overview]] - [[String-Methods-Overview|Methods overview]]
- [[Concatenate-Strings]] - [[Concatenate-Strings|Concatenation]]
