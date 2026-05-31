# How to Use Regex Flags (g, i, m)

## Definition

Regex flags modify how patterns are matched. They are added after the closing `/` in literal notation or as the second argument in the `RegExp` constructor.

**Syntax:**
```javascript
const regex = /pattern/flags;
const regex = new RegExp("pattern", "flags");
```

**Common Flags:**
- `g` - Global: Match all occurrences
- `i` - Case-insensitive: Ignore case
- `m` - Multiline: Match line beginnings/endings

## Code Examples

### The `g` (Global) Flag
```javascript
const text = "cat bat rat";

// Without g flag - matches first occurrence only
console.log(text.match(/at/));       // ["at"]
console.log(text.replace(/at/, "AT"));  // "cAT bat rat"

// With g flag - matches all occurrences
console.log(text.match(/at/g));      // ["at", "at", "at"]
console.log(text.replace(/at/g, "AT"));  // "cAT bAT rAT"

// Count occurrences
function countOccurrences(str, pattern) {
    const matches = str.match(new RegExp(pattern, "g"));
    return matches ? matches.length : 0;
}

console.log(countOccurrences("hello hello hello", /hello/g));  // 3
```

### The `i` (Case-Insensitive) Flag
```javascript
const text = "Hello World";

// Without i flag - case-sensitive
console.log(text.match(/hello/));    // null
console.log(text.indexOf("hello"));  // -1

// With i flag - case-insensitive
console.log(text.match(/hello/i));   // ["Hello"]
console.log(text.toLowerCase().indexOf("hello"));  // 0

// Case-insensitive search function
function caseInsensitiveSearch(text, searchTerm) {
    return text.toLowerCase().includes(searchTerm.toLowerCase());
}

console.log(caseInsensitiveSearch("Hello World", "hello"));  // true
```

### The `m` (Multiline) Flag
```javascript
const multilineText = `First line
Second line
Third line`;

// Without m flag - ^ and $ match start/end of string
console.log(/^Second/.test(multilineText));  // false
console.log(/Third$/.test(multilineText));   // false

// With m flag - ^ and $ match start/end of each line
console.log(/^Second/m.test(multilineText));  // true
console.log(/Third$/m.test(multilineText));   // true

// Extract all lines starting with capital letter
const lines = multilineText.match(/^[A-Z].*$/gm);
console.log(lines);  // ["First line", "Second line", "Third line"]
```

### Combining Flags
```javascript
const text = `Hello World
hello earth
HELLO UNIVERSE`;

// g + i: Find all occurrences case-insensitively
console.log(text.match(/hello/gi));  // ["Hello", "hello", "HELLO"]

// g + m: Replace at start of each line
const replaced = text.replace(/^hello/gi, "Hi");
console.log(replaced);
// "Hi World
// Hi earth
// Hi UNIVERSE"

// g + i + m: All three flags combined
const result = text.match(/^hello/gim);
console.log(result);  // ["Hello", "hello", "HELLO"]
```

### Practical Examples
```javascript
// Remove all HTML tags
const html = "<p>Hello</p><br><b>World</b>";
console.log(html.replace(/<[^>]*>/g, ""));  // "HelloWorld"

// Validate email (case-insensitive, global not needed)
const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/i;

// Extract all hashtags
const tweet = "Love #JavaScript and #coding! #webdev is fun!";
console.log(tweet.match(/#\w+/g));  // ["#JavaScript", "#coding", "#webdev"]

// Replace multiple spaces with single space
const messy = "Hello    World   Test";
console.log(messy.replace(/\s+/g, " "));  // "Hello World Test"
```

## Common Use Cases

1. **Global replacement**: Replace all occurrences in a string
2. **Case-insensitive search**: Find text regardless of case
3. **Multiline parsing**: Process text line by line
4. **Data extraction**: Extract all matches from text
5. **Text cleaning**: Remove or replace patterns throughout text

## Common Mistakes

1. **Forgetting `g` flag**: Only replaces first occurrence
2. **Overusing `g` flag**: May cause performance issues with large strings
3. **Not considering multiline**: `^` and `$` behave differently with `m`
4. **Case sensitivity issues**: Forgetting `i` flag when needed

## Related Topics

- [[Create-Regex]]
- [[Match-Patterns]]
- [[Replace-Text]]
- [[Test-Regex]]

## Quick Revision Summary

| Flag | Name | Description |
|------|------|-------------|
| `g` | Global | Match all occurrences |
| `i` | Case-insensitive | Ignore case when matching |
| `m` | Multiline | `^` and `$` match line beginnings/endings |
| `gi` | Combined | All occurrences, case-insensitive |
| `gm` | Combined | All occurrences, multiline |
| `gim` | Combined | All flags together |
