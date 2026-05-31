# Regular Expressions in JavaScript

## Definition

**Regular expressions** (regex) are patterns used to match character combinations in strings. They provide a concise way to search, validate, and manipulate text. JavaScript supports regex through the `RegExp` object and pattern literals.

---

## Creating Regular Expressions

```javascript
// Regex literal (preferred)
const pattern = /hello/i;

// Constructor (for dynamic patterns)
const pattern2 = new RegExp("hello", "i");

// With variables
const searchTerm = "world";
const pattern3 = new RegExp(searchTerm, "g");
```

---

## Basic Patterns

```javascript
// Literal characters
/hello/ // Matches "hello" exactly

// Special characters
/\d/ // Digit: 0-9
/\D/ // Non-digit
/\w/ // Word character: a-z, A-Z, 0-9, _
/\W/ // Non-word character
/\s/ // Whitespace
/\S/ // Non-whitespace

// Quantifiers
/a+/ // One or more 'a'
/a*/ // Zero or more 'a'
/a?/ // Zero or one 'a'
/a{3}/ // Exactly 3 'a's
/a{2,4}/ // 2 to 4 'a's

// Anchors
/^hello/ // Starts with "hello"
/hello$/ // Ends with "hello"
/^hello$/ // Exactly "hello"
```

---

## Common Patterns

```javascript
// Email validation
const email = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;

// Phone number (US)
const phone = /^\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})$/;

// URL
const url = /^(https?:\/\/)?([\da-z.-]+)\.([a-z.]{2,6})([/\w .-]*)*\/?$/;

// IP address
const ip = /^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;

// Date (YYYY-MM-DD)
const date = /^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$/;

// Strong password
const password = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;
```

---

## Methods

###.test() - Check for Match

```javascript
const pattern = /hello/i;

console.log(pattern.test("Hello World")); // true
console.log(pattern.test("Hi there")); // false
console.log(pattern.test("say hello")); // true
```

### .exec() - Get Match Details

```javascript
const pattern = /(\d{3})-(\d{3})-(\d{4})/;
const result = pattern.exec("Phone: 123-456-7890");

console.log(result[0]); // "123-456-7890" (full match)
console.log(result[1]); // "123" (first group)
console.log(result[2]); // "456" (second group)
console.log(result[3]); // "7890" (third group)
console.log(result.index); // 7 (start position)
```

### String Methods with Regex

```javascript
const str = "Hello World, hello Universe";

// match - returns matches
console.log(str.match(/hello/gi)); // ["Hello", "hello"]

// matchAll - returns iterator with groups
for (const match of str.matchAll(/hello/gi)) {
  console.log(match[0], match.index);
}

// replace - replace matches
console.log(str.replace(/hello/gi, "Hi")); // "Hi World, Hi Universe"

// replaceAll - replace all occurrences
console.log(str.replaceAll("hello", "Hi")); // "Hi World, Hi Universe"

// search - find index of first match
console.log(str.search(/world/i)); // 6

// split - split by pattern
console.log(str.split(/[\s,]+/)); // ["Hello", "World", "hello", "Universe"]
```

---

## Flags

```javascript
// g - Global (match all occurrences)
"hello hello".match(/hello/); // ["hello"] (first only)
"hello hello".match(/hello/g); // ["hello", "hello"]

// i - Case insensitive
"Hello".match(/hello/); // null
"Hello".match(/hello/i); // ["Hello"]

// m - Multiline (^ and $ match line boundaries)
"line1\nline2".match(/^line2/m); // ["line2"]

// s - Dotall (. matches newline)
"hello\nworld".match(/hello.world/); // null
"hello\nworld".match(/hello.world/s); // ["hello\nworld"]

// u - Unicode support
"😊".match(/./u); // ["😊"]

// y - Sticky (match from lastIndex)
const str = "test test";
const regex = /test/gy;
console.log(regex.exec(str)); // ["test"]
console.log(regex.exec(str)); // ["test"]
console.log(regex.exec(str)); // null
```

---

## Common Use Cases

### Form Validation

```javascript
function validateForm(data) {
  const errors = {};
  
  if (!/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/.test(data.email)) {
    errors.email = "Invalid email";
  }
  
  if (data.password.length < 8) {
    errors.password = "Password too short";
  } else if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(data.password)) {
    errors.password = "Password must include uppercase, lowercase, and number";
  }
  
  if (!/^\d{3}-\d{3}-\d{4}$/.test(data.phone)) {
    errors.phone = "Invalid phone format (XXX-XXX-XXXX)";
  }
  
  return errors;
}
```

### Text Processing

```javascript
// Extract all URLs from text
function extractUrls(text) {
  const urlPattern = /https?:\/\/[^\s]+/g;
  return text.match(urlPattern) || [];
}

// Extract emails
function extractEmails(text) {
  const emailPattern = /[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g;
  return text.match(emailPattern) || [];
}

// Clean whitespace
function cleanWhitespace(text) {
  return text.replace(/\s+/g, " ").trim();
}

// Convert to slug
function toSlug(text) {
  return text
    .toLowerCase()
    .replace(/[^\w\s-]/g, "")
    .replace(/[\s_]+/g, "-")
    .replace(/^-+|-+$/g, "");
}
```

### Search and Replace

```javascript
// Highlight search terms
function highlight(text, term) {
  const pattern = new RegExp(`(${term})`, "gi");
  return text.replace(pattern, '<mark>$1</mark>');
}

// Redact sensitive data
function redact(text) {
  return text
    .replace(/\b\d{3}-\d{2}-\d{4}\b/g, "***-**-****") // SSN
    .replace(/\b\d{16}\b/g, "****-****-****-****") // Credit card
    .replace(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g, "***@***.***"); // Email
}
```

### Parsing

```javascript
// Parse query string
function parseQuery(query) {
  const params = {};
  const pairs = query.slice(1).split("&");
  
  for (const pair of pairs) {
    const [key, value] = pair.split("=");
    params[decodeURIComponent(key)] = decodeURIComponent(value || "");
  }
  
  return params;
}

// Parse CSV
function parseCSV(text) {
  const lines = text.split("\n");
  return lines.map(line => {
    const matches = line.match(/(".*?"|[^,]+)/g);
    return matches.map(m => m.replace(/^"|"$/g, ""));
  });
}
```

---

## Common Mistakes

### Mistake 1: Forgetting to Escape Special Characters

```javascript
// Wrong: . matches any character
"file.txt".match(/file.txt/); // true
"filetxt".match(/file.txt/); // true

// Correct: escape the dot
"file.txt".match(/file\.txt/); // true
"filetxt".match(/file\.txt/); // false
```

### Mistake 2: Not Using Anchors

```javascript
// Wrong: matches anywhere in string
"hello world".match(/hello/); // true
"say hello".match(/hello/); // true

// Correct: use anchors for exact match
"hello world".match(/^hello$/); // false
"hello".match(/^hello$/); // true
```

### Mistake 3: Greedy vs Lazy

```javascript
// Greedy (default) - matches as much as possible
"<div>hello</div>".match(/<.*>/); // ["<div>hello</div>"]

// Lazy - matches as little as possible
"<div>hello</div>".match(/<.*?>/); // ["<div>"]

// Common: extracting content between tags
const content = "<p>paragraph</p><p>another</p>";
const matches = content.match(/<p>(.*?)<\/p>/g);
// ["<p>paragraph</p>", "<p>another</p>"]
```

### Mistake 4: Global Flag and lastIndex

```javascript
const pattern = /hello/g;
const str = "hello hello";

console.log(pattern.test(str)); // true
console.log(pattern.test(str)); // true (starts from lastIndex)

// Reset lastIndex
pattern.lastIndex = 0;
console.log(pattern.test(str)); // true
```

---

## Quick Revision Summary

| Pattern | Matches |
|---------|---------|
| `\d` | Digit (0-9) |
| `\D` | Non-digit |
| `\w` | Word character |
| `\W` | Non-word |
| `\s` | Whitespace |
| `\S` | Non-whitespace |
| `.` | Any character |
| `^` | Start of string |
| `$` | End of string |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n}` | Exactly n |
| `{n,m}` | n to m |

---

## Related Topics

- [[string]] - String methods
- [[String-Methods]] - String manipulation
- [[expression]] - Expressions
- [[condition]] - Conditional logic
- [[loop]] - Iteration with regex
- [[Fetch-API]] - Using regex with fetched data
- [[JSON]] - Parsing JSON with regex