# How to Use the `split()` Method

## Definition

The `split()` method divides a string into an ordered list of substrings, puts them into an array, and returns the array. The original string remains unchanged.

**Syntax:**
```javascript
string.split(separator, limit);
```

- **separator**: String or regex used to split
- **limit**: Maximum number of array elements (optional)

## Code Examples

### Basic Usage
```javascript
const text = "Hello World";

// Split by space
console.log(text.split(" "));  // ["Hello", "World"]

// Split by comma
const csv = "apple,banana,cherry";
console.log(csv.split(","));  // ["apple", "banana", "cherry"]

// Split by character
console.log(text.split(""));  // ["H", "e", "l", "l", "o", " ", "W", "o", "r", "l", "d"]
```

### Using Limit
```javascript
const text = "one,two,three,four,five";

// Split with limit
console.log(text.split(",", 3));  // ["one", "two", "three"]

// Limit of 1
console.log(text.split(",", 1));  // ["one"]
```

### Split with Regex
```javascript
const text = "Hello123World456Test";

// Split by digits
console.log(text.split(/\d+/));  // ["Hello", "World", "Test"]

// Split by multiple delimiters
const text2 = "apple;banana,cherry orange";
console.log(text2.split(/[,;\s]+/));  // ["apple", "banana", "cherry", "orange"]

// Split by word boundaries
const sentence = "Hello World, how are you?";
console.log(sentence.split(/\b/));  // ["", "Hello", " ", "World", ", ", "how", " ", "are", " ", "you", "?", ""]
```

### Practical Examples

#### Split CSV with Quotes
```javascript
function parseCSV(line) {
    return line.split(",").map(field => field.trim());
}

console.log(parseCSV("John,Smith,30,New York"));
// ["John", "Smith", "30", "New York"]
```

#### Convert String to Array of Characters
```javascript
function reverseString(str) {
    return str.split("").reverse().join("");
}

console.log(reverseString("hello"));  // "olleh"
```

#### Extract Query Parameters
```javascript
function parseQueryParams(queryString) {
    const params = {};
    queryString
        .replace(/^\?/, "")
        .split("&")
        .forEach(param => {
            const [key, value] = param.split("=");
            params[decodeURIComponent(key)] = decodeURIComponent(value);
        });
    return params;
}

console.log(parseQueryParams("?name=John&age=30&city=NYC"));
// { name: "John", age: "30", city: "NYC" }
```

#### Split CamelCase
```javascript
function splitCamelCase(str) {
    return str.replace(/([a-z])([A-Z])/g, "$1 $2").split(" ");
}

console.log(splitCamelCase("camelCaseString"));  // ["camel", "Case", "String"]
console.log(splitCamelCase("HTMLElement"));       // ["HTML", "Element"]
```

#### Convert String to Words
```javascript
function getWords(text) {
    return text
        .toLowerCase()
        .split(/[^a-z0-9]+/)
        .filter(word => word.length > 0);
}

console.log(getWords("Hello, World! How are you?"));
// ["hello", "world", "how", "are", "you"]
```

## Common Use Cases

1. **Parsing CSV data** and tabular text
2. **Extracting words** from sentences
3. **Splitting file paths** into components
4. **Processing user input** (comma-separated values)
5. **Tokenizing text** for analysis

## Common Mistakes

1. **Forgetting to handle empty strings**: `"".split(",")` returns `[""]`
2. **Not escaping special characters**: In regex patterns
3. **Using `split("")` for characters**: Consider `Array.from()` for Unicode
4. **Ignoring limit parameter**: May cause unexpected results

## Related Topics

- [[Replace-Text]]
- [[Match-Patterns]]
- [[Extract-Substring]]
- [[Array-Methods]]

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| `split(",")` | Split by comma |
| `split("")` | Split into characters |
| `split(/\d+/)` | Split by digits |
| `split(",", 3)` | Split with limit of 3 |
| Original string | Remains unchanged |
| Empty string | Returns `[""]` |
