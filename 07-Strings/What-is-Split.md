# What is String Split in JavaScript?

The **split()** method divides a string into an array of substrings and returns that array.

## Definition

`split()` splits a string into an array based on a specified separator (delimiter).

## Basic Syntax

```javascript
string.split(separator);
string.split(separator, limit);
```

## How Split Works

### Splitting by Character

```javascript
let str = "Hello";
let result = str.split("");
console.log(result);  // ["H", "e", "l", "l", "o"]
```

### Splitting by String

```javascript
let str = "Hello World";
let result = str.split(" ");
console.log(result);  // ["Hello", "World"]
```

### Splitting by Comma

```javascript
let str = "apple,banana,cherry";
let result = str.split(",");
console.log(result);  // ["apple", "banana", "cherry"]
```

## Using Limit Parameter

```javascript
let str = "Hello World JavaScript";
let result = str.split(" ", 2);
console.log(result);  // ["Hello", "World"]

let result2 = str.split(" ", 1);
console.log(result2);  // ["Hello"]
```

## Splitting with Regex

```javascript
let str = "Hello1World2JavaScript3";
let result = str.split(/\d/);
console.log(result);  // ["Hello", "World", "JavaScript", ""]
```

## Common Use Cases

### 1. Converting String to Array

```javascript
let str = "Hello World";
let words = str.split(" ");
console.log(words);  // ["Hello", "World"]
```

### 2. Reversing a String

```javascript
let str = "Hello";
let reversed = str.split("").reverse().join("");
console.log(reversed);  // "olleH"
```

### 3. Getting File Extension

```javascript
let filename = "document.pdf";
let parts = filename.split(".");
let extension = parts[parts.length - 1];
console.log(extension);  // "pdf"
```

### 4. Parsing CSV Data

```javascript
let csv = "John,Doe,30,New York";
let fields = csv.split(",");
console.log(fields);  // ["John", "Doe", "30", "New York"]
```

### 5. Extracting Query Parameters

```javascript
let url = "https://example.com?name=John&age=30";
let queryString = url.split("?")[1];
let params = queryString.split("&");
let result = params.map(param => param.split("="));
console.log(result);  // [["name", "John"], ["age", "30"]]
```

### 6. Counting Words

```javascript
let text = "Hello World from JavaScript";
let wordCount = text.split(" ").length;
console.log(wordCount);  // 4
```

## Splitting with No Separator

```javascript
let str = "Hello";
let result = str.split();
console.log(result);  // ["Hello"]
```

## Splitting Empty String

```javascript
let str = "";
let result = str.split("");
console.log(result);  // []
```

## Common Mistakes to Avoid

1. **Not handling empty strings** - split can return empty strings in array
2. **Using split() for single character** - use bracket notation instead
3. **Forgetting limit parameter** - it limits the number of splits
4. **Not handling undefined separators** - split() without args returns full string

## Related Topics

- [[What-is-String]] - String basics
- [[What-is-Array]] - Array basics
- [[What-is-Methods]] - Other string methods
- [[What-is-Join]] - Join array to string
- [[What-is-Slice]] - Extract substrings

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| split() | Divides string into array |
| Separator | Character or string to split by |
| Limit | Maximum number of splits |
| No args | Returns full string in array |
| Empty string | Returns empty array |
| Regex | Can use patterns as separator |
| Return | Array of substrings |
