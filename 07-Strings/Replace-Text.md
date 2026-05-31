# How to Use the `replace()` Method

## Definition

The `replace()` method returns a new string with some or all matches of a pattern replaced by a replacement. The original string remains unchanged.

**Syntax:**
```javascript
string.replace(searchValue, replacement);
string.replace(regexp, replacement);
string.replace(regexp, function);
```

- **searchValue**: String or regex to search for
- **replacement**: String or function that returns the replacement

## Code Examples

### Basic String Replacement
```javascript
const text = "Hello World";

// Replace first occurrence
console.log(text.replace("World", "JavaScript"));  // "Hello JavaScript"

// Case-sensitive by default
console.log(text.replace("hello", "Hi"));  // "Hello World" (no match)
console.log(text.replace("Hello", "Hi"));  // "Hi World"
```

### Global Replacement with Regex
```javascript
const text = "cat bat rat";

// Replace first only
console.log(text.replace(/at/, "AT"));  // "cAT bat rat"

// Replace all with g flag
console.log(text.replace(/at/g, "AT"));  // "cAT bAT rAT"

// Case-insensitive replacement
const text2 = "Hello HELLO hello";
console.log(text2.replace(/hello/gi, "Hi"));  // "Hi Hi Hi"
```

### Function as Replacement
```javascript
// Convert to uppercase
const text = "hello world";
console.log(text.replace(/\w+/g, (match) => match.toUpperCase()));
// "HELLO WORLD"

// Double the length of each word
console.log(text.replace(/\w+/g, (match) => match + match));
// "hellohelloworld world"

// Add numbers to matches
const text2 = "a b c";
let counter = 1;
console.log(text2.replace(/\w/g, () => counter++));
// "1 2 3"
```

### Capture Groups in Replacement
```javascript
// Swap first and last name
const name = "John Smith";
console.log(name.replace(/(\w+) (\w+)/, "$2 $1"));  // "Smith John"

// Format date
const date = "2024-01-15";
console.log(date.replace(/(\d{4})-(\d{2})-(\d{2})/, "$2/$3/$1"));
// "01/15/2024"

// Named groups
const name2 = "John Smith";
console.log(name2.replace(/(?<first>\w+) (?<last>\w+)/, "$<last>, $<first>"));
// "Smith, John"
```

### Practical Examples

#### Clean User Input
```javascript
function cleanInput(input) {
    return input
        .replace(/^\s+/, "")           // Remove leading spaces
        .replace(/\s+$/, "")           // Remove trailing spaces
        .replace(/\s+/g, " ");         // Collapse multiple spaces
}

console.log(cleanInput("  Hello   World  "));  // "Hello World"
```

#### Extract Domain from Email
```javascript
function extractDomain(email) {
    return email.replace(/.*@(.+\..+)$/, "$1");
}

console.log(extractDomain("user@example.com"));  // "example.com"
console.log(extractDomain("test@company.org"));  // "company.org"
```

#### Mask Credit Card Number
```javascript
function maskCard(cardNumber) {
    return cardNumber.replace(/\d(?=\d{4})/g, "*");
}

console.log(maskCard("1234567890123456"));  // "************3456"
```

#### Replace Multiple Words
```javascript
const text = "I like cats and cats like me";
const replacements = {
    "cats": "dogs",
    "like": "love"
};

const result = text.replace(/\b(cats|like)\b/g, (match) => replacements[match]);
console.log(result);  // "I love dogs and dogs love me"
```

#### Add Commas to Numbers
```javascript
function addCommas(num) {
    return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}

console.log(addCommas(1234567));    // "1,234,567"
console.log(addCommas(1234567890)); // "1,234,567,890"
```

## Common Use Cases

1. **Text formatting** and cleaning
2. **Data transformation** (dates, numbers, names)
3. **Input sanitization** (removing unwanted characters)
4. **Template processing** (replacing placeholders)
5. **Search and replace** operations

## Common Mistakes

1. **Forgetting `g` flag**: Only replaces first occurrence
2. **Not returning replacement**: Function must return the replacement string
3. **Confusing `replace()` with `replaceAll()`**: `replaceAll()` requires `g` flag for regex
4. **Not escaping special characters**: In dynamic patterns

## Related Topics

- [[Match-Patterns]]
- [[Split-String]]
- [[Create-Regex]]
- [[Use-RegexFlags]]

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| `replace(search, replacement)` | Replace first match |
| `replace(/pattern/g, replacement)` | Replace all matches |
| `replace(/pattern/, function)` | Dynamic replacement |
| `$1`, `$2` | Reference capture groups |
| `$<name>` | Reference named groups |
| Original string | Remains unchanged |
