# String Methods Overview

## Definition

JavaScript provides numerous built-in methods for manipulating and working with strings. This comprehensive guide covers all essential string methods with examples and use cases.

## Searching Methods

### indexOf() / lastIndexOf()

```javascript
const str = "Hello, World! Hello, JavaScript!";

// indexOf - Find first occurrence
console.log(str.indexOf("Hello"));      // 0
console.log(str.indexOf("Hello", 5));   // 14 (start from index 5)
console.log(str.indexOf("xyz"));        // -1 (not found)

// lastIndexOf - Find last occurrence
console.log(str.lastIndexOf("Hello"));  // 14
console.log(str.lastIndexOf("o"));      // 8
```

### includes()

```javascript
const str = "Hello, World!";

console.log(str.includes("World"));     // true
console.log(str.includes("world"));     // false (case-sensitive)
console.log(str.includes("World", 8));  // true (start position)
```

### startsWith() / endsWith()

```javascript
const filename = "document.pdf";

console.log(filename.startsWith("doc"));  // true
console.log(filename.endsWith(".pdf"));   // true
console.log(filename.endsWith(".doc"));   // false
```

### search()

```javascript
const str = "Hello, World! 123";

console.log(str.search(/\d+/));          // 14
console.log(str.search(/hello/i));       // 0 (case-insensitive)
```

## Extraction Methods

### slice()

```javascript
const str = "Hello, World!";

console.log(str.slice(0, 5));           // "Hello"
console.log(str.slice(7));              // "World!"
console.log(str.slice(-6));             // "orld!"
console.log(str.slice(-6, -1));         // "orld"
```

### substring()

```javascript
const str = "Hello, World!";

console.log(str.substring(0, 5));       // "Hello"
console.log(str.substring(7));          // "World!"
console.log(str.substring(5, 0));       // "Hello" (swaps if start > end)
```

### charAt() / charCodeAt()

```javascript
const str = "Hello";

console.log(str.charAt(0));            // "H"
console.log(str.charCodeAt(0));        // 72 (ASCII value)
console.log(str[0]);                   // "H" (bracket notation)
```

## Transform Methods

### toUpperCase() / toLowerCase()

```javascript
const str = "Hello World";

console.log(str.toUpperCase());        // "HELLO WORLD"
console.log(str.toLowerCase());        // "hello world"
```

### trim() / trimStart() / trimEnd()

```javascript
const str = "   Hello, World!   ";

console.log(str.trim());               // "Hello, World!"
console.log(str.trimStart());          // "Hello, World!   "
console.log(str.trimEnd());            // "   Hello, World!"
```

### replace() / replaceAll()

```javascript
const str = "Hello World, Hello JavaScript";

console.log(str.replace("Hello", "Hi"));
// "Hi World, Hello JavaScript" (first occurrence only)

console.log(str.replaceAll("Hello", "Hi"));
// "Hi World, Hi JavaScript" (all occurrences)

// Using regex
console.log(str.replace(/Hello/g, "Hi"));
// "Hi World, Hi JavaScript"
```

### padStart() / padEnd()

```javascript
const num = "5";

console.log(num.padStart(3, "0"));     // "005"
console.log(num.padEnd(3, "0"));       // "500"
console.log(num.padStart(3));          // "  5" (default: space)
```

### repeat()

```javascript
const str = "Ha";

console.log(str.repeat(3));            // "HaHaHa"
console.log(str.repeat(0));            // ""
console.log(str.repeat(-1));           // RangeError
```

### normalize()

```javascript
const str1 = "\u0041\u0301";  // A + combining acute accent
const str2 = "\u00C1";        // Á (precomposed)

console.log(str1.normalize() === str2.normalize()); // true
```

## Splitting and Joining

### split()

```javascript
const str = "Hello, World, JavaScript";

console.log(str.split(", "));          // ["Hello", "World", "JavaScript"]
console.log(str.split(""));            // ["H","e","l","l","o",","," ",...]
console.log(str.split(", ", 2));       // ["Hello", "World"] (limit)
```

### Array.join() (Inverse of split)

```javascript
const arr = ["Hello", "World", "JavaScript"];

console.log(arr.join(" "));            // "Hello World JavaScript"
console.log(arr.join(", "));           // "Hello, World, JavaScript"
console.log(arr.join(""));             // "HelloWorldJavaScript"
```

## Conversion Methods

### toString()

```javascript
const num = 42;
const bool = true;

console.log(num.toString());          // "42"
console.log(bool.toString());         // "true"
console.log((123).toString(2));       // "1111011" (binary)
console.log((255).toString(16));      // "ff" (hex)
```

### Number to String

```javascript
const num = 1234567;

console.log(String(num));             // "1234567"
console.log(num + "");                // "1234567"
console.log(`${num}`);                // "1234567"
console.log(num.toLocaleString());    // "1,234,567"
```

### String to Number

```javascript
const str = "42";

console.log(Number(str));             // 42
console.log(parseInt(str));           // 42
console.log(parseFloat("3.14"));      // 3.14
console.log(+str);                    // 42 (unary plus)
console.log(str - 0);                 // 42
```

## Comparison Methods

### localeCompare()

```javascript
const a = "apple";
const b = "banana";

console.log(a.localeCompare(b));      // -1 (a before b)
console.log(b.localeCompare(a));      // 1 (b after a)
console.log(a.localeCompare(a));      // 0 (equal)

// Sorting with localeCompare
const fruits = ["banana", "Apple", "cherry"];
fruits.sort((a, b) => a.localeCompare(b, undefined, { sensitivity: 'base' }));
console.log(fruits); // ["Apple", "banana", "cherry"]
```

### String Comparison

```javascript
console.log("apple" < "banana");     // true
console.log("A" < "a");             // true (uppercase first in ASCII)
console.log("10" < "9");            // true (string comparison!)
```

## Encoding/Decoding

### encodeURIComponent() / decodeURIComponent()

```javascript
const url = "https://example.com/path?name=John&age=30";

const encoded = encodeURIComponent(url);
console.log(encoded);
// "https%3A%2F%2Fexample.com%2Fpath%3Fname%3DJohn%26age%3D30"

console.log(decodeURIComponent(encoded));
// "https://example.com/path?name=John&age=30"
```

### btoa() / atob() (Base64)

```javascript
const str = "Hello, World!";

const encoded = btoa(str);
console.log(encoded);                 // "SGVsbG8sIFdvcmxkIQ=="

console.log(atob(encoded));           // "Hello, World!"
```

## Pattern Matching

### match()

```javascript
const str = "Hello World 123";

console.log(str.match(/\d+/));        // ["123"]
console.log(str.match(/\d+/g));       // ["123"]
console.log(str.match(/(\w+)\s(\w+)/));
// ["Hello World", "Hello", "World", index: 0, input: "Hello World 123"]
```

### matchAll()

```javascript
const str = "test1 test2 test3";
const matches = [...str.matchAll(/test(\d)/g)];

matches.forEach(match => {
  console.log(match[0]);              // "test1", "test2", "test3"
  console.log(match[1]);              // "1", "2", "3"
  console.log(match.index);           // 0, 6, 12
});
```

## Quick Reference Table

| Method | Purpose | Returns |
|--------|---------|---------|
| `indexOf()` | Find first occurrence | Number |
| `lastIndexOf()` | Find last occurrence | Number |
| `includes()` | Check if contains | Boolean |
| `startsWith()` | Check beginning | Boolean |
| `endsWith()` | Check ending | Boolean |
| `search()` | Find pattern | Number |
| `slice()` | Extract section | String |
| `substring()` | Extract section | String |
| `charAt()` | Get character | String |
| `toUpperCase()` | Convert to upper | String |
| `toLowerCase()` | Convert to lower | String |
| `trim()` | Remove whitespace | String |
| `replace()` | Replace first match | String |
| `replaceAll()` | Replace all matches | String |
| `padStart()` | Pad beginning | String |
| `padEnd()` | Pad ending | String |
| `repeat()` | Repeat string | String |
| `split()` | String to array | Array |
| `match()` | Pattern matching | Array |
| `matchAll()` | All pattern matches | Iterator |
| `normalize()` | Unicode normalization | String |
| `localeCompare()` | Compare strings | Number |
| `toString()` | Convert to string | String |

## Common Use Cases

### Data Validation

```javascript
function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

function validatePhone(phone) {
  return /^\+?[\d\s-]{10,}$/.test(phone);
}
```

### Text Processing

```javascript
// Word count
function wordCount(text) {
  return text.trim().split(/\s+/).length;
}

// Truncate text
function truncate(text, maxLength, suffix = '...') {
  if (text.length <= maxLength) return text;
  return text.slice(0, maxLength - suffix.length) + suffix;
}
```

### Formatting

```javascript
// Slug generation
function slugify(text) {
  return text
    .toLowerCase()
    .replace(/[^\w\s-]/g, '')
    .replace(/[\s_-]+/g, '-')
    .replace(/^-+|-+$/g, '');
}

// Title case
function titleCase(text) {
  return text.replace(/\b\w/g, char => char.toUpperCase());
}
```

## Related Topics

- [[string]] - String fundamentals
- [[Regular-Expressions]] - Pattern matching
- [[Template-Literals]] - Template strings
- [[Array-Methods]] - Array manipulation
- [[JSON]] - Data serialization
- [[Encoding]] - Character encoding

## Quick Revision

**Top 10 Most Used Methods:**
1. `slice()` - Extract substrings
2. `includes()` - Check existence
3. `trim()` - Remove whitespace
4. `replace()/replaceAll()` - Find & replace
5. `split()` - Convert to array
6. `toUpperCase()/toLowerCase()` - Case conversion
7. `indexOf()` - Find position
8. `startsWith()/endsWith()` - Check boundaries
9. `repeat()` - Repeat strings
10. `padStart()/padEnd()` - Padding

**Key Takeaway:** Strings are immutable - all methods return new strings without modifying the original.