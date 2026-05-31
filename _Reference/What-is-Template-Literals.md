# Template Literals

## Definition

Template literals are string literals enclosed by backticks (`` ` ``) that allow embedded expressions, multi-line strings, and string interpolation using `${expression}` syntax. Introduced in ES6, they provide a more powerful and readable alternative to string concatenation.

## Syntax

```
`string text`
`string text ${expression} text`
```

## Code Examples

### Basic String Interpolation

```javascript
const name = "Alice";
const age = 25;

// Old way - string concatenation
const oldWay = "Hello, " + name + "! You are " + age + " years old.";

// New way - template literal
const newWay = `Hello, ${name}! You are ${age} years old.`;

console.log(newWay); // "Hello, Alice! You are 25 years old."
```

### Expressions Inside Template Literals

```javascript
const a = 10;
const b = 20;

console.log(`The sum is: ${a + b}`);          // "The sum is: 30"
console.log(`The product is: ${a * b}`);       // "The product is: 200"
console.log(`Is a greater? ${a > b ? 'Yes' : 'No'}`); // "Is a greater? No"
```

### Multi-Line Strings

```javascript
// Without template literals
const multiLineOld = "Line 1\n" +
  "Line 2\n" +
  "Line 3";

// With template literals
const multiLineNew = `
Line 1
Line 2
Line 3`;

console.log(multiLineNew);
```

### Nested Template Literals

```javascript
const users = ["Alice", "Bob", "Charlie"];

const html = `
<ul>
  ${users.map(user => `<li>${user}</li>`).join("")}
</ul>`;

console.log(html);
```

### Tagged Templates

```javascript
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => {
    const value = values[i] ? `<strong>${values[i]}</strong>` : "";
    return result + str + value;
  }, "");
}

const name = "Alice";
const msg = highlight`Hello ${name}, welcome!`;
console.log(msg); // "Hello <strong>Alice</strong>, welcome!"
```

### Escaping Backticks

```javascript
const code = `This is a backtick: \``;
console.log(code); // "This is a backtick: `"
```

## Common Use Cases

- **String interpolation** — Embed variables and expressions directly in strings
- **Multi-line strings** — Create formatted strings without `\n`
- **HTML templating** — Generate HTML markup dynamically
- **SQL query building** — Construct queries with parameters
- **Logging and debugging** — Create descriptive log messages
- **Internationalization** — Build translatable string patterns

## Common Mistakes

```javascript
// Mistake 1: Using single/double quotes instead of backticks
const msg = `Hello ${name}`;  // Correct
const msg = 'Hello ${name}';  // Wrong - will not interpolate

// Mistake 2: Forgetting backticks for multi-line
// This will cause a syntax error:
// const str = "Line 1
// Line 2";

// Mistake 3: Complex expressions should be kept readable
// Hard to read:
const x = `${a > b ? 'greater' : 'less'} than ${c + d / e * f}`;

// Better: extract to a variable or function
const comparison = a > b ? 'greater' : 'less';
const result = `${comparison} than ${calculateValue(c, d, e, f)}`;
```

## Related Topics

- [[String-Methods]]
- [[String-Manipulation]]
- [[Arrow-Functions]]
- [[Destructuring]]
- [[ES6-Features]]
- [[Spread-Operator]]

## Quick Revision

| Feature | Description |
|---------|-------------|
| Backticks | `` `string` `` defines a template literal |
| Interpolation | `${expression}` embeds values |
| Multi-line | No need for `\n` characters |
| Expressions | Any JS expression works inside `${}` |
| Tagged | `tag`template`` passes strings to a function |
| ES6 | Introduced in ECMAScript 2015 |
