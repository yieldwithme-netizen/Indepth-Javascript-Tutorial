# What are Common String Methods in JavaScript?

JavaScript provides many built-in methods for manipulating strings. These methods return new strings (strings are immutable).

## Definition

String methods are built-in functions that perform operations on strings and return new strings without modifying the original.

## Case Methods

### toUpperCase()

Converts string to uppercase:

```javascript
let str = "Hello World";
console.log(str.toUpperCase());  // "HELLO WORLD"
```

### toLowerCase()

Converts string to lowercase:

```javascript
let str = "Hello World";
console.log(str.toLowerCase());  // "hello world"
```

### toLocaleUpperCase()

Converts to uppercase considering locale:

```javascript
let str = "hello";
console.log(str.toLocaleUpperCase());  // "HELLO"
```

### toLocaleLowerCase()

Converts to lowercase considering locale:

```javascript
let str = "HELLO";
console.log(str.toLocaleLowerCase());  // "hello"
```

## Trimming Methods

### trim()

Removes whitespace from both ends:

```javascript
let str = "  Hello World  ";
console.log(str.trim());       // "Hello World"
console.log(str.trimStart());  // "Hello World  "
console.log(str.trimEnd());    // "  Hello World"
```

## Searching Methods

### indexOf()

Returns index of first occurrence, or -1:

```javascript
let str = "Hello World";
console.log(str.indexOf("World"));   // 6
console.log(str.indexOf("JavaScript")); // -1
console.log(str.indexOf("o", 5));    // 7 (start from index 5)
```

### lastIndexOf()

Returns index of last occurrence:

```javascript
let str = "Hello World Hello";
console.log(str.lastIndexOf("Hello"));  // 12
console.log(str.lastIndexOf("o"));      // 14
```

### includes()

Checks if string contains substring:

```javascript
let str = "Hello World";
console.log(str.includes("World"));     // true
console.log(str.includes("JavaScript")); // false
console.log(str.includes("world", 6));   // true (case-sensitive)
```

### startsWith()

Checks if string starts with substring:

```javascript
let str = "Hello World";
console.log(str.startsWith("Hello"));   // true
console.log(str.startsWith("World"));   // false
console.log(str.startsWith("lo", 3));   // true
```

### endsWith()

Checks if string ends with substring:

```javascript
let str = "Hello World";
console.log(str.endsWith("World"));     // true
console.log(str.endsWith("Hello"));     // false
console.log(str.endsWith("llo", 5));    // true
```

### search()

Searches for match using regex:

```javascript
let str = "Hello World";
console.log(str.search(/World/));  // 6
console.log(str.search(/world/));  // -1 (case-sensitive)
```

## Extraction Methods

### slice()

Extracts section of string:

```javascript
let str = "Hello World";
console.log(str.slice(0, 5));    // "Hello"
console.log(str.slice(6));       // "World"
console.log(str.slice(-5));      // "World"
```

### substring()

Similar to slice but doesn't accept negative indices:

```javascript
let str = "Hello World";
console.log(str.substring(0, 5));  // "Hello"
console.log(str.substring(6));     // "World"
console.log(str.substring(-5));    // "Hello World" (treats -5 as 0)
```

### substr()

Extracts characters (deprecated):

```javascript
let str = "Hello World";
console.log(str.substr(0, 5));  // "Hello"
console.log(str.substr(6));     // "World"
```

## Conversion Methods

### charAt()

Returns character at index:

```javascript
let str = "Hello";
console.log(str.charAt(0));  // "H"
console.log(str.charAt(4));  // "o"
console.log(str.charAt(10)); // ""
```

### charCodeAt()

Returns Unicode of character at index:

```javascript
let str = "Hello";
console.log(str.charCodeAt(0));  // 72 (Unicode for 'H')
console.log(str.charCodeAt(4));  // 111 (Unicode for 'o')
```

### at()

Returns character at index (ES2022):

```javascript
let str = "Hello";
console.log(str.at(0));    // "H"
console.log(str.at(-1));   // "o"
console.log(str.at(-5));   // "H"
```

## Repeat and Pad Methods

### repeat()

Repeats string n times:

```javascript
let str = "Hello";
console.log(str.repeat(3));  // "HelloHelloHello"
console.log(str.repeat(0));  // ""
```

### padStart()

Pads string from start:

```javascript
let str = "5";
console.log(str.padStart(3, "0"));  // "005"
console.log(str.padStart(10, "*")); // "*********5"
```

### padEnd()

Pads string from end:

```javascript
let str = "5";
console.log(str.padEnd(3, "0"));  // "500"
console.log(str.padEnd(10, "*")); // "5*********"
```

## Common Use Cases

### 1. Title Case

```javascript
let str = "hello world";
let titleCase = str.split(" ").map(word => 
    word.charAt(0).toUpperCase() + word.slice(1)
).join(" ");
console.log(titleCase);  // "Hello World"
```

### 2. Slug Generation

```javascript
let title = "Hello World JavaScript";
let slug = title.toLowerCase().replace(/\s+/g, "-");
console.log(slug);  // "hello-world-javascript"
```

### 3. Truncating Text

```javascript
let text = "This is a long text that needs to be truncated";
let maxLength = 20;
let truncated = text.length > maxLength ? 
    text.slice(0, maxLength) + "..." : text;
console.log(truncated);  // "This is a long text ..."
```

### 4. Masking Email

```javascript
let email = "user@example.com";
let [name, domain] = email.split("@");
let masked = name.charAt(0) + "*".repeat(name.length - 1) + "@" + domain;
console.log(masked);  // "u******@example.com"
```

## Common Mistakes to Avoid

1. **Expecting original to change** - all methods return new strings
2. **Confusing slice and substring** - they handle negative indices differently
3. **Using deprecated methods** - avoid substr()
4. **Not handling case sensitivity** - use toLowerCase() for comparisons

## Related Topics

- [[What-is-String]] - String basics
- [[What-is-Indexing]] - Accessing characters
- [[What-is-Immutability]] - Strings can't be changed
- [[What-is-Slice]] - slice() method
- [[What-is-Methods]] - This topic

## Quick Revision Summary

| Category | Methods |
|----------|---------|
| Case | toUpperCase, toLowerCase, toLocaleUpperCase, toLocaleLowerCase |
| Trim | trim, trimStart, trimEnd |
| Search | indexOf, lastIndexOf, includes, startsWith, endsWith, search |
| Extract | slice, substring, substr (deprecated) |
| Convert | charAt, charCodeAt, at |
| Pad | repeat, padStart, padEnd |
| Immutability | All methods return new strings |
