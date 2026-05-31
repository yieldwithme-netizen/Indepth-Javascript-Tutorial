# split

## Definition

`split()` divides a string into an **array of substrings** based on a separator.

## Basic Usage

```javascript
const str = "Hello World";

str.split(" ");    // ["Hello", "World"]
str.split("");     // ["H", "e", "l", "l", "o", " ", "W", "o", "r", "l", "d"]
```

## With Limit

```javascript
const str = "a,b,c,d,e";

str.split(",", 2); // ["a", "b"]
str.split(",", 3); // ["a", "b", "c"]
```

## Common Patterns

```javascript
// Split by space
"Hello World".split(" "); // ["Hello", "World"]

// Split by comma
"a,b,c".split(","); // ["a", "b", "c"]

// Split by regex
"Hello1World2".split(/\d/); // ["Hello", "World"]

// Reverse string
str.split("").reverse().join("");

// Check palindrome
const isPalindrome = (str) => str === str.split("").reverse().join("");
```

## Quick Revision

- `split(separator)` divides string
- Returns array of substrings
- Optional limit parameter
- Common: split by space, comma, regex
- Use for: string manipulation

---

## Related Topics

- [[What-is-Split]] - [[What-is-Split|Split]] overview
- [[Split-String]] - [[Split-String|Splitting strings]]
- [[What-is-String]] - [[What-is-String|Strings]]
- [[String-Methods]] - [[String-Methods|String methods]]
