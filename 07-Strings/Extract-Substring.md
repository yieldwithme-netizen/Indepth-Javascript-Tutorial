# How to Extract Substrings with `slice()`

## Definition

The `slice()` method extracts a section of a string and returns it as a new string without modifying the original string. It takes two arguments: the start index and optionally the end index.

**Syntax:**
```javascript
string.slice(startIndex, endIndex);
```

- **startIndex**: The position to begin extraction (inclusive). If negative, counts from the end.
- **endIndex**: The position to end extraction (exclusive). If omitted, extracts to the end.

## Code Examples

### Basic Usage
```javascript
const message = "Hello, World!";

// Extract from index 0 to 5
console.log(message.slice(0, 5));    // "Hello"

// Extract from index 7 to end
console.log(message.slice(7));       // "World!"

// Extract last 6 characters
console.log(message.slice(-6));      // "orld!"

// Extract from index -6 to -1
console.log(message.slice(-6, -1));  // "orld"
```

### Practical Examples
```javascript
// Get file extension
const filename = "document.pdf";
const extension = filename.slice(filename.lastIndexOf(".") + 1);
console.log(extension);  // "pdf"

// Truncate text with ellipsis
function truncate(text, maxLength) {
    if (text.length <= maxLength) return text;
    return text.slice(0, maxLength) + "...";
}

console.log(truncate("This is a long sentence", 10));  // "This is a l..."

// Extract year from date string
const date = "2024-01-15";
const year = date.slice(0, 4);
console.log(year);  // "2024"
```

### Negative Index Examples
```javascript
const str = "JavaScript";

console.log(str.slice(-4));      // "Script"
console.log(str.slice(-4, -1));  // "Scri"
console.log(str.slice(0, -4));   // "Java"
```

## Common Use Cases

1. **Extracting file extensions** from filenames
2. **Truncating long text** for display purposes
3. **Parsing date strings** to extract components
4. **Creating substrings** for comparison or validation
5. **Extracting URLs or domains** from full URLs

## Common Mistakes

1. **Confusing `slice()` with `substring()`**: `slice()` accepts negative indices; `substring()` does not.
2. **Forgetting endIndex is exclusive**: The character at endIndex is not included.
3. **Expecting original string modification**: `slice()` returns a new string; original remains unchanged.

## Related Topics

- [[Split-String]]
- [[Case-Methods]]
- [[Replace-Text]]
- [[String-Methods-Overview]]

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| `slice(start, end)` | Extracts section from start to end (exclusive) |
| Negative indices | Count from end of string |
| Original string | Remains unchanged |
| `slice(start)` | Extracts from start to end of string |
