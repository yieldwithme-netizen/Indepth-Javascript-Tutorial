# How to Use the `match()` Method

## Definition

The `match()` method searches a string for a match against a regular expression and returns the results as an array, or `null` if no match is found.

**Syntax:**
```javascript
string.match(regexp);
```

**Return Values:**
- Without `g` flag: Returns array with match details (index, groups)
- With `g` flag: Returns array of all matches
- No match: Returns `null`

## Code Examples

### Basic Usage
```javascript
const text = "Hello World, hello JavaScript!";

// Simple match
const result = text.match(/hello/);
console.log(result);  // ["hello"]
console.log(result.index);  // 13 (position of match)

// Case-insensitive match
const result2 = text.match(/hello/i);
console.log(result2);  // ["Hello"]
console.log(result2.index);  // 0
```

### Global Match (with `g` flag)
```javascript
const text = "cat bat rat mat";

// Find all matches
const matches = text.match(/at/g);
console.log(matches);  // ["at", "at", "at", "at"]
console.log(matches.length);  // 4

// Extract all words starting with 'c'
const words = text.match(/\bc\w+/g);
console.log(words);  // ["cat"]
```

### Capture Groups
```javascript
const text = "2024-01-15";

// Capture groups in parentheses
const result = text.match(/(\d{4})-(\d{2})-(\d{2})/);
console.log(result);
// ["2024-01-15", "2024", "01", "15"]
//  [full match,  year,   month, day]

console.log(result[1]);  // "2024" (year)
console.log(result[2]);  // "01" (month)
console.log(result[3]);  // "15" (day)
```

### Named Capture Groups
```javascript
const text = "John Smith";

const result = text.match(/(?<first>\w+)\s(?<last>\w+)/);
console.log(result.groups.first);  // "John"
console.log(result.groups.last);   // "Smith"
```

### Practical Examples

#### Extract Email Parts
```javascript
const email = "user@example.com";
const result = email.match(/^(\w+)@(\w+)\.(\w+)$/);

if (result) {
    console.log("Username:", result[1]);  // "user"
    console.log("Domain:", result[2]);    // "example"
    console.log("Extension:", result[3]); // "com"
}
```

#### Find All Numbers
```javascript
const text = "I have 3 cats, 5 dogs, and 12 fish";
const numbers = text.match(/\d+/g);
console.log(numbers);  // ["3", "5", "12"]
console.log(numbers.map(Number));  // [3, 5, 12]
```

#### Validate Phone Number
```javascript
function validatePhone(phone) {
    const regex = /^\(?(\d{3})\)?[-. ]?(\d{3})[-. ]?(\d{4})$/;
    const match = phone.match(regex);

    if (match) {
        return {
            valid: true,
            area: match[1],
            prefix: match[2],
            line: match[3]
        };
    }
    return { valid: false };
}

console.log(validatePhone("(123) 456-7890"));
// { valid: true, area: "123", prefix: "456", line: "7890" }
```

#### Extract Hashtags
```javascript
const tweet = "Love #JavaScript and #coding! #webdev is fun!";
const hashtags = tweet.match(/#\w+/g);
console.log(hashtags);  // ["#JavaScript", "#coding", "#webdev"]
```

## Common Use Cases

1. **Pattern matching** in strings
2. **Data extraction** (emails, phone numbers, dates)
3. **Input validation** (form fields)
4. **Parsing structured text** (CSV, log files)
5. **Search functionality** in applications

## Common Mistakes

1. **Forgetting `g` flag**: Only returns first match
2. **Not checking for `null`**: Match can return null if no match found
3. **Confusing index property**: Index is only available without `g` flag
4. **Not handling capture groups**: Groups are only available without `g` flag

## Related Topics

- [[Create-Regex]]
- [[Use-RegexFlags]]
- [[Replace-Text]]
- [[Test-Regex]]

## Quick Revision Summary

| Scenario | Return Value |
|----------|--------------|
| No match | `null` |
| Without `g` flag | Array with match details |
| With `g` flag | Array of all matches |
| `result[0]` | Full matched string |
| `result[1]`, `result[2]`, etc. | Capture group values |
| `result.index` | Position of match (without `g`) |
| `result.groups` | Named capture groups |
