# What is String match() in JavaScript?

The **match()** method retrieves the result of matching a string against a regular expression.

## Definition

`match()` searches a string for a match against a regex pattern and returns the matches as an array.

## Basic Syntax

```javascript
string.match(regexp);
```

## How match() Works

### Without Global Flag (g)

Returns first match with details:

```javascript
let str = "Hello World";
let result = str.match(/Hello/);
console.log(result);
// ["Hello", index: 0, input: "Hello World", groups: undefined]
```

### With Global Flag (g)

Returns array of all matches:

```javascript
let str = "hello world hello";
let result = str.match(/hello/g);
console.log(result);
// ["hello", "hello"]
```

## Return Values

### When Match Found

```javascript
// Without g flag - detailed match
let str = "Hello World";
let result = str.match(/Hello/);
console.log(result[0]);      // "Hello" (matched text)
console.log(result.index);   // 0 (position)
console.log(result.input);   // "Hello World" (original string)

// With g flag - simple array
let result2 = str.match(/l/g);
console.log(result2);        // ["l", "l", "l"]
```

### When No Match Found

```javascript
let str = "Hello World";
let result = str.match(/xyz/);
console.log(result);  // null
```

## Common Use Cases

### 1. Extracting Numbers

```javascript
let text = "I have 2 cats and 3 dogs";
let numbers = text.match(/\d+/g);
console.log(numbers);  // ["2", "3"]
```

### 2. Validating Email Format

```javascript
let email = "user@example.com";
let isValid = email.match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/);
console.log(isValid !== null);  // true
```

### 3. Extracting Domain from Email

```javascript
let email = "user@example.com";
let domain = email.match(/@(.+)$/);
console.log(domain[1]);  // "example.com"
```

### 4. Finding All Words

```javascript
let text = "Hello World from JavaScript";
let words = text.match(/\w+/g);
console.log(words);  // ["Hello", "World", "from", "JavaScript"]
```

### 5. Checking for Special Characters

```javascript
let username = "user123";
let hasSpecial = username.match(/[!@#$%^&*(),.?":{}|<>]/);
console.log(hasSpecial !== null);  // false
```

## match() vs exec()

```javascript
let str = "Hello World Hello";

// match() with g flag - returns all matches
let matchResult = str.match(/Hello/g);
console.log(matchResult);  // ["Hello", "Hello"]

// exec() with g flag - returns first match, use lastIndex
let regex = /Hello/g;
let execResult;
while ((execResult = regex.exec(str)) !== null) {
    console.log(execResult[0], execResult.index);
}
// Hello 0
// Hello 12
```

## Common Mistakes to Avoid

1. **Forgetting global flag** - only returns first match
2. **Not checking for null** - match() returns null if no match
3. **Confusing match() and exec()** - they behave differently with g flag
4. **Using match() for simple checks** - use test() instead for boolean results

## Related Topics

- [[What-is-Regex]] - Regex basics
- [[What-is-RegexFlags]] - Using flags with match()
- [[What-is-String]] - String basics
- [[What-is-Replace]] - Replacing matched text
- [[What-is-Test]] - test() for boolean checks

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| match() | Retrieves match results |
| Without g | Returns first match with details |
| With g | Returns array of all matches |
| No match | Returns null |
| Result includes | Matched text, index, input |
| Use test() | For simple true/false checks |
