# How to Create Regular Expressions

## Definition

Regular expressions (regex) are patterns used to match character combinations in strings. In JavaScript, regex can be created using either a literal notation or the `RegExp` constructor.

**Syntax:**
```javascript
// Literal notation
const regex = /pattern/flags;

// Constructor notation
const regex = new RegExp("pattern", "flags");
```

## Code Examples

### Literal Notation
```javascript
// Simple pattern matching
const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
const phonePattern = /\d{3}-\d{3}-\d{4}/;
const wordBoundary = /\bword\b/;

// Test strings
console.log(emailPattern.test("user@example.com"));  // true
console.log(phonePattern.test("123-456-7890"));      // true
```

### Constructor Notation
```javascript
// Dynamic pattern from user input
function createSearchRegex(userInput) {
    const escaped = userInput.replace(/[.*+?^${}()|[\]\\]/g, "\\$&");
    return new RegExp(escaped, "gi");
}

const searchText = "hello";
const regex = createSearchRegex(searchText);
console.log(regex.test("Hello World"));  // true
```

### Common Patterns
```javascript
// Email validation
const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;

// Phone number (US format)
const phoneRegex = /^\(\d{3}\)\s?\d{3}-\d{4}$/;

// URL pattern
const urlRegex = /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/;

// Password strength (min 8 chars, 1 uppercase, 1 lowercase, 1 number)
const strongPasswordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/;

// Test patterns
console.log(emailRegex.test("user@example.com"));           // true
console.log(phoneRegex.test("(123) 456-7890"));             // true
console.log(urlRegex.test("https://www.example.com"));      // true
console.log(strongPasswordRegex.test("Password123"));       // true
```

### Special Characters
```javascript
// \d - digit (0-9)
const digitPattern = /\d+/;

// \w - word character (a-z, A-Z, 0-9, _)
const wordPattern = /\w+/;

// \s - whitespace
const spacePattern = /\s+/;

// . - any character (except newline)
const anyCharPattern = /h.llo/;

// * - zero or more occurrences
const zeroOrMore = /ab*c/;

// + - one or more occurrences
const oneOrMore = /ab+c/;

// ? - zero or one occurrence
const optional = /colou?r/;
```

## Common Use Cases

1. **Form validation** (email, phone, password strength)
2. **Search and replace** operations in text
3. **Data extraction** from strings
4. **Input sanitization** and cleaning
5. **Pattern matching** in log files or data

## Common Mistakes

1. **Forgetting to escape special characters** in user input
2. **Not using `^` and `$`** for complete string matching
3. **Performance issues** with poorly written patterns (catastrophic backtracking)
4. **Confusing literal vs constructor notation** for dynamic patterns

## Related Topics

- [[Use-RegexFlags]]
- [[Match-Patterns]]
- [[Replace-Text]]
- [[Test-Regex]]

## Quick Revision Summary

| Syntax | Description |
|--------|-------------|
| `/pattern/flags` | Literal notation |
| `new RegExp("pattern", "flags")` | Constructor notation |
| `\d` | Match any digit |
| `\w` | Match any word character |
| `+` | One or more occurrences |
| `*` | Zero or more occurrences |
| `^` | Start of string |
| `$` | End of string |
