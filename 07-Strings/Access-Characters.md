# How to Access String Characters

**String characters** can be accessed by index, similar to arrays. Strings are zero-indexed.

## Bracket Notation

```javascript
const str = "Hello";

console.log(str[0]);  // "H"
console.log(str[1]);  // "e"
console.log(str[4]);  // "o"
console.log(str[10]); // undefined (out of bounds)
```

## charAt() Method

```javascript
const str = "Hello";

console.log(str.charAt(0));  // "H"
console.log(str.charAt(4));  // "o"
console.log(str.charAt(10)); // "" (empty string, not undefined)
```

## charCodeAt() Method

```javascript
const str = "Hello";

console.log(str.charCodeAt(0));  // 72 (H)
console.log(str.charCodeAt(4));  // 111 (o)
console.log(str.charCodeAt(0).toString(16)); // "48" (hex)
```

## codePointAt() (For Unicode)

```javascript
const emoji = "😀🎉";

console.log(emoji[0]);           // "�" (surrogate pair)
console.log(emoji.codePointAt(0)); // 128512 (correct Unicode code point)
console.log(emoji.codePointAt(1)); // 9193 (second surrogate)
console.log(emoji.codePointAt(2)); // 127881 (🎉)
```

## Accessing Last Character

```javascript
const str = "Hello";

// Method 1: Using length
console.log(str[str.length - 1]); // "o"

// Method 2: Using slice
console.log(str.slice(-1)); // "o"

// Method 3: Using at() (ES2022+)
console.log(str.at(-1)); // "o"
```

## Common Use Cases

- Validating input character by character
- Parsing strings
- Building character-based algorithms
- Working with Unicode characters

## Common Mistakes

```javascript
const str = "Hello";

// ❌ Strings are immutable
str[0] = "J"; // No error, but doesn't change the string
console.log(str); // "Hello" (unchanged)

// ✅ Create new string instead
const newStr = "J" + str.slice(1);
console.log(newStr); // "Jello"

// ❌ Using bracket notation on null/undefined
const empty = null;
// console.log(empty[0]); // TypeError

// ✅ Check before accessing
if (empty && empty[0]) {
  console.log(empty[0]);
}

// ❌ Confusing charAt() and bracket notation
const str2 = "Hi";
console.log(str2.charAt(5));  // ""
console.log(str2[5]);         // undefined
```

## Related Topics

- [[Create-String]]
- [[String-Methods]]
- [[Concatenate-Strings]]
- [[Array-Access]]
- [[Unicode]]

## Quick Revision

| Method | Syntax | Returns |
|--------|--------|---------|
| Bracket | `str[0]` | Character or `undefined` |
| `charAt()` | `str.charAt(0)` | Character or `""` |
| `charCodeAt()` | `str.charCodeAt(0)` | Number (UTF-16) |
| `codePointAt()` | `str.codePointAt(0)` | Number (Unicode) |
| `at()` | `str.at(-1)` | Character (supports negatives) |

**Key Point:** Use `at()` for negative indexing; remember strings are immutable.