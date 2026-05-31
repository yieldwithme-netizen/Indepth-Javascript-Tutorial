# What are Regex Flags in JavaScript?

**Regex flags** modify how a regular expression pattern behaves. They are added after the pattern to change matching behavior.

## Definition

Flags are optional parameters that alter the default behavior of regular expressions. They are placed after the closing `/` in literal notation.

## Common Flags

### g (Global) Flag

Matches all occurrences, not just the first:

```javascript
let str = "hello world hello";

// Without global flag
console.log(str.match(/hello/));  // ["hello"] (first match only)

// With global flag
console.log(str.match(/hello/g)); // ["hello", "hello"] (all matches)
```

### i (Case-Insensitive) Flag

Ignores case when matching:

```javascript
let str = "Hello World";

// Without case-insensitive flag
console.log(str.match(/hello/));    // null

// With case-insensitive flag
console.log(str.match(/hello/i));   // ["Hello"]
```

### m (Multiline) Flag

Treats `^` and `$` as matching start/end of each line:

```javascript
let str = "Line 1\nLine 2\nLine 3";

// Without multiline flag
console.log(str.match(/^Line/));   // ["Line"] (only first line)

// With multiline flag
console.log(str.match(/^Line/mg)); // ["Line", "Line", "Line"]
```

## Combining Flags

```javascript
// Case-insensitive and global
let str = "Hello World HELLO javascript";
console.log(str.match(/hello/ig)); // ["Hello", "HELLO"]

// All three flags
let str = "Line 1\nLine 2\nLine 3";
console.log(str.match(/^line/igm)); // ["Line", "Line", "Line"]
```

## Flag Behaviors in Different Methods

### match() Method

```javascript
let str = "hello world hello";

// Without g flag - returns first match with details
console.log(str.match(/hello/));
// ["hello", index: 0, input: "hello world hello"]

// With g flag - returns array of all matches
console.log(str.match(/hello/g));
// ["hello", "hello"]
```

### replace() Method

```javascript
let str = "Hello World Hello";

// Without g flag - replaces first match only
console.log(str.replace(/Hello/, "Hi"));
// "Hi World Hello"

// With g flag - replaces all matches
console.log(str.replace(/Hello/g, "Hi"));
// "Hi World Hi"
```

### search() Method

```javascript
let str = "Hello World";

// search() always returns first match index
console.log(str.search(/world/i));  // 6
console.log(str.search(/world/));   // -1 (case-sensitive)
```

## Special Flags

### s (Dotall) Flag

Makes `.` match newlines:

```javascript
let str = "Hello\nWorld";

// Without s flag
console.log(str.match(/Hello.World/));  // null

// With s flag
console.log(str.match(/Hello.World/s)); // ["Hello\nWorld"]
```

### u (Unicode) Flag

Enables Unicode matching:

```javascript
let str = "Hello 👋 World";

// Without u flag
console.log(str.match(/👋/));    // ["👋"]

// With u flag (better Unicode support)
console.log(str.match(/👋/u));   // ["👋"]
```

### y (Sticky) Flag

Matches only from lastIndex position:

```javascript
let str = "Hello World";
let regex = /World/y;
regex.lastIndex = 6;
console.log(regex.exec(str)); // ["World"]
```

## Common Use Cases

### 1. Case-Insensitive Search

```javascript
let email = "User@Example.COM";
let isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/i.test(email);
console.log(isValid);  // true
```

### 2. Global Replacement

```javascript
let text = "I love cats. Cats are cute.";
let result = text.replace(/cats/gi, "dogs");
console.log(result);  // "I love dogs. Dogs are cute."
```

### 3. Multi-line Validation

```javascript
let log = "Error: File not found\nError: Permission denied";
let errorCount = (log.match(/^Error/gm) || []).length;
console.log(errorCount);  // 2
```

## Common Mistakes to Avoid

1. **Forgetting global flag** - only replaces first occurrence
2. **Not understanding flag differences** - match() behaves differently with g flag
3. **Overusing global flag** - can cause performance issues
4. **Mixing up flags** - test each flag separately first

## Related Topics

- [[What-is-Regex]] - Regex basics
- [[What-is-Match]] - match() method
- [[What-is-Replace]] - replace() method
- [[What-is-String]] - String basics
- [[What-is-Test]] - test() method

## Quick Revision Summary

| Flag | Description | Example |
|------|-------------|---------|
| g | Global (all matches) | `/hello/g` |
| i | Case-insensitive | `/hello/i` |
| m | Multiline | `/^hello/m` |
| s | Dotall (dot matches newline) | `/hello.world/s` |
| u | Unicode | `/👋/u` |
| y | Sticky (from lastIndex) | `/hello/y` |
