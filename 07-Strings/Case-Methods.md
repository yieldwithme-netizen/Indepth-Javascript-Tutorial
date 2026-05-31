# How to Use `toUpperCase()` and `toLowerCase()`

## Definition

The `toUpperCase()` and `toLowerCase()` methods return a string converted to uppercase or lowercase. They do not modify the original string.

**Syntax:**
```javascript
string.toUpperCase();
string.toLowerCase();
```

## Code Examples

### Basic Usage
```javascript
const text = "Hello World";

console.log(text.toUpperCase());  // "HELLO WORLD"
console.log(text.toLowerCase());  // "hello world"

// Original unchanged
console.log(text);  // "Hello World"
```

### Practical Examples

#### Normalize User Input
```javascript
function normalizeInput(input) {
    return input.toLowerCase().trim();
}

const email = "  User@Example.COM  ";
console.log(normalizeEmail(email));  // "user@example.com"
```

#### Case-Insensitive Comparison
```javascript
function equalsIgnoreCase(str1, str2) {
    return str1.toLowerCase() === str2.toLowerCase();
}

console.log(equalsIgnoreCase("Hello", "hello"));    // true
console.log(equalsIgnoreCase("JavaScript", "JAVASCRIPT"));  // true
```

#### Capitalize First Letter
```javascript
function capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

console.log(capitalize("hello"));  // "Hello"
console.log(capitalize("jAVASCRIPT"));  // "Javascript"
```

#### Title Case
```javascript
function toTitleCase(str) {
    return str
        .toLowerCase()
        .split(" ")
        .map(word => word.charAt(0).toUpperCase() + word.slice(1))
        .join(" ");
}

console.log(toTitleCase("hello world"));  // "Hello World"
console.log(toTitleCase("the quick brown fox"));  // "The Quick Brown Fox"
```

#### Sentence Case
```javascript
function toSentenceCase(str) {
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

console.log(toSentenceCase("hello world."));  // "Hello world."
```

#### Toggle Case
```javascript
function toggleCase(str) {
    return str
        .split("")
        .map(char =>
            char === char.toUpperCase()
                ? char.toLowerCase()
                : char.toUpperCase()
        )
        .join("");
}

console.log(toggleCase("Hello World"));  // "hELLO wORLD"
console.log(toggleCase("JavaScript"));   // "jAVAsCRIPT"
```

#### Check Case Properties
```javascript
function isUpperCase(str) {
    return str === str.toUpperCase() && str !== str.toLowerCase();
}

function isLowerCase(str) {
    return str === str.toLowerCase() && str !== str.toUpperCase();
}

console.log(isUpperCase("HELLO"));   // true
console.log(isLowerCase("hello"));   // true
console.log(isUpperCase("Hello"));   // false
console.log(isLowerCase("Hello"));   // false
```

#### Search Filter
```javascript
function filterByCase(items, searchTerm) {
    const lowerSearch = searchTerm.toLowerCase();
    return items.filter(item =>
        item.toLowerCase().includes(lowerSearch)
    );
}

const fruits = ["Apple", "Banana", "avocado", "cherry"];
console.log(filterByCase(fruits, "apple"));  // ["Apple"]
console.log(filterByCase(fruits, "a"));      // ["Apple", "Banana", "avocado"]
```

## Common Use Cases

1. **Input normalization** for validation
2. **Case-insensitive comparisons**
3. **Formatting text** for display
4. **Search functionality** with case-insensitive matching
5. **Styling text** in user interfaces

## Common Mistakes

1. **Expecting original string modification**: Methods return new strings
2. **Not handling special characters**: Some characters may not change case
3. **Using for locale-specific cases**: Not suitable for Turkish (İ, ı)
4. **Performance issues**: Creating many new strings in loops

## Related Topics

- [[Extract-Substring]]
- [[Match-Patterns]]
- [[Replace-Text]]
- [[String-Methods-Overview]]

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| `toUpperCase()` | Convert to uppercase |
| `toLowerCase()` | Convert to lowercase |
| Original string | Remains unchanged |
| Special characters | May not change case |
| Locale | Not locale-aware |
