# What is String Replace in JavaScript?

The **replace()** and **replaceAll()** methods return a new string with some or all matches of a pattern replaced.

## Definition

`replace()` replaces the first match, while `replaceAll()` replaces all matches of a pattern in a string.

## Basic Syntax

```javascript
string.replace(searchValue, replaceValue);
string.replaceAll(searchValue, replaceValue);
```

## replace() Method

### Replacing First Match

```javascript
let str = "Hello World Hello";
let result = str.replace("Hello", "Hi");
console.log(result);  // "Hi World Hello"
```

### Using Regex Without g Flag

```javascript
let str = "Hello World Hello";
let result = str.replace(/Hello/, "Hi");
console.log(result);  // "Hi World Hello"
```

### Using Regex With g Flag

```javascript
let str = "Hello World Hello";
let result = str.replace(/Hello/g, "Hi");
console.log(result);  // "Hi World Hi"
```

## replaceAll() Method (ES2021)

### Replacing All Occurrences

```javascript
let str = "Hello World Hello";
let result = str.replaceAll("Hello", "Hi");
console.log(result);  // "Hi World Hi"
```

### Without Global Flag

```javascript
let str = "Hello World Hello";
let result = str.replaceAll(/Hello/, "Hi");
console.log(result);  // "Hi World Hi" (replaceAll doesn't need g flag)
```

## Using Replacement Patterns

### $1, $2 - Capture Groups

```javascript
let str = "John Doe";
let result = str.replace(/(\w+) (\w+)/, "$2 $1");
console.log(result);  // "Doe John"
```

### $& - Matched Text

```javascript
let str = "Hello World";
let result = str.replace(/World/, "($&)");
console.log(result);  // "Hello (World)"
```

### $` - Text Before Match

```javascript
let str = "Hello World";
let result = str.replace(/World/, "$`");
console.log(result);  // "Hello Hello "
```

### $' - Text After Match

```javascript
let str = "Hello World";
let result = str.replace(/Hello/, "$'");
console.log(result);  // " World World"
```

## Common Use Cases

### 1. Replacing All Spaces

```javascript
let str = "Hello World JavaScript";
let result = str.replace(/ /g, "-");
console.log(result);  // "Hello-World-JavaScript"
```

### 2. Formatting Phone Numbers

```javascript
let phone = "1234567890";
let formatted = phone.replace(/(\d{3})(\d{3})(\d{4})", "($1) $2-$3");
console.log(formatted);  // "(123) 456-7890"
```

### 3. Capitalizing Words

```javascript
let str = "hello world";
let result = str.replace(/\b\w/g, (char) => char.toUpperCase());
console.log(result);  // "Hello World"
```

### 4. Removing HTML Tags

```javascript
let html = "<p>Hello <b>World</b></p>";
let text = html.replace(/<[^>]*>/g, "");
console.log(text);  // "Hello World"
```

### 5. Sanitizing User Input

```javascript
let input = "  Hello   World  ";
let cleaned = input.replace(/\s+/g, " ").trim();
console.log(cleaned);  // "Hello World"
```

## Using Function as Replacement

```javascript
let str = "hello world";
let result = str.replace(/\w+/g, (match) => {
    return match.charAt(0).toUpperCase() + match.slice(1);
});
console.log(result);  // "Hello World"
```

## Common Mistakes to Avoid

1. **Using replace() without g flag** - only replaces first occurrence
2. **Forgetting replaceAll() is ES2021** - check browser support
3. **Not escaping special characters** - use `\\` for literal dots, stars
4. **Not capturing groups** - use parentheses in regex for patterns

## Related Topics

- [[What-is-Regex]] - Regex basics
- [[What-is-RegexFlags]] - Using flags with replace
- [[What-is-Match]] - match() method
- [[What-is-String]] - String basics
- [[What-is-Split]] - Splitting strings

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| replace() | Replaces first match |
| replace() + g | Replaces all matches |
| replaceAll() | Replaces all occurrences (ES2021) |
| $1, $2 | Capture group references |
| $& | Matched text |
| Function | Custom replacement logic |
