# What is Regular Expression (Regex) in JavaScript?

A **Regular Expression (Regex)** is a pattern used to match character combinations in strings. It's a powerful tool for text search and manipulation.

## Definition

A regular expression is an object that describes a pattern of characters. Regex patterns can be used to search, edit, and manipulate text.

## Creating Regular Expressions

### 1. RegExp Literal (Recommended)

```javascript
let pattern = /hello/;
let caseInsensitive = /hello/i;
```

### 2. RegExp Constructor

```javascript
let pattern = new RegExp("hello");
let caseInsensitive = new RegExp("hello", "i");
```

## Basic Pattern Matching

```javascript
let str = "Hello World";

// Test if pattern exists
console.log(/Hello/.test(str));      // true
console.log(/hello/.test(str));      // false (case-sensitive)
console.log(/hello/i.test(str));     // true (case-insensitive)
```

## Special Characters

### Dot (.) - Any Character

```javascript
let pattern = /h.t/;  // Matches "hat", "hot", "hut"
console.log(pattern.test("hat"));  // true
console.log(pattern.test("hot"));  // true
console.log(pattern.test("hut"));  // true
console.log(pattern.test("hut"));  // true
console.log(pattern.test("ht"));   // false (need a character in between)
```

### Asterisk (*) - Zero or More

```javascript
let pattern = /ab*c/;  // Matches "ac", "abc", "abbc"
console.log(pattern.test("ac"));    // true
console.log(pattern.test("abc"));   // true
console.log(pattern.test("abbc"));  // true
console.log(pattern.test("ab"));    // false
```

### Plus (+) - One or More

```javascript
let pattern = /ab+c/;  // Matches "abc", "abbc" but not "ac"
console.log(pattern.test("abc"));   // true
console.log(pattern.test("abbc"));  // true
console.log(pattern.test("ac"));    // false
```

### Question Mark (?) - Zero or One

```javascript
let pattern = /colou?r/;  // Matches "color" or "colour"
console.log(pattern.test("color"));  // true
console.log(pattern.test("colour")); // true
console.log(pattern.test("colouur")); // false
```

## Character Classes

```javascript
// \d - Any digit [0-9]
/\d+/.test("123")    // true

// \w - Word character [a-zA-Z0-9_]
/\w+/.test("hello")  // true

// \s - Whitespace
/\s+/.test("hello world")  // true

// [abc] - Any of a, b, or c
/[aeiou]/.test("hello")  // true (contains 'e')

// [^abc] - Not a, b, or c
/[^aeiou]/.test("hello")  // true (contains 'h')
```

## Quantifiers

```javascript
// {n} - Exactly n times
/\d{3}/.test("123")   // true
/\d{3}/.test("12")    // false

// {n,} - n or more times
/\d{2,}/.test("1")    // false
/\d{2,}/.test("12")   // true
/\d{2,}/.test("123")  // true

// {n,m} - Between n and m times
/\d{2,4}/.test("1")    // false
/\d{2,4}/.test("12")   // true
/\d{2,4}/.test("1234") // true
/\d{2,4}/.test("12345") // false
```

## Common Use Cases

### 1. Email Validation

```javascript
let emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(emailPattern.test("user@example.com"));  // true
console.log(emailPattern.test("invalid-email"));     // false
```

### 2. Phone Number Validation

```javascript
let phonePattern = /^\d{3}-\d{3}-\d{4}$/;
console.log(phonePattern.test("123-456-7890"));  // true
console.log(phonePattern.test("1234567890"));    // false
```

### 3. Password Strength

```javascript
let strongPassword = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;
console.log(strongPassword.test("Password123!"));  // true
```

## Common Mistakes to Avoid

1. **Forgetting to escape special characters** - use `\` for literal dots, stars, etc.
2. **Not using anchors** - `^` and `$` for start/end of string
3. **Overusing greedy quantifiers** - prefer non-greedy `*?`, `+?`
4. **Not testing patterns** - always test regex before using

## Related Topics

- [[What-is-RegexFlags]] - Flags for regex behavior
- [[What-is-Match]] - Using match() with regex
- [[What-is-Replace]] - Replacing with regex patterns
- [[What-is-String]] - String basics
- [[What-is-Test]] - test() method

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| Regex | Pattern for matching characters |
| Creation | `/pattern/` or `new RegExp()` |
| test() | Returns true/false if match found |
| Special chars | `.`, `*`, `+`, `?`, `^`, `$` |
| Character classes | `\d`, `\w`, `\s`, `[abc]` |
| Quantifiers | `{n}`, `{n,}`, `{n,m}` |
| Escaping | Use `\` for literal special chars |
