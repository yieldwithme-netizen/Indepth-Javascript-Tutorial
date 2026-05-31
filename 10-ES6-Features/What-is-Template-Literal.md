# What is Template Literal?

## Definition

Template literals use **backticks** (`` ` ``) for strings with embedded expressions.

## Basic Usage

```javascript
const name = "John";
const age = 30;

// Old way
const msg1 = "Hello, " + name + "! You are " + age + " years old.";

// Template literal
const msg2 = `Hello, ${name}! You are ${age} years old.`;
```

## Expressions in Template Literals

```javascript
const x = 10;
const y = 5;

console.log(`Sum: ${x + y}`);        // "Sum: 15"
console.log(`Product: ${x * y}`);    // "Product: 50"
console.log(`Is large: ${x > 10 ? "yes" : "no"}`);
```

## Multi-line Strings

```javascript
// Old way
const html1 = "<div>\n" +
              "  <h1>Hello</h1>\n" +
              "</div>";

// Template literal
const html2 = `<div>
  <h1>Hello</h1>
</div>`;
```

## Tagged Templates

```javascript
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + `<b>${values[i] || ""}</b>`;
    }, "");
}

const name = "John";
const msg = highlight`Hello ${name}, welcome!`;
```

## Quick Revision

- Template literals use backticks
- Embed expressions with `${expression}`
- Support multi-line strings
- Better than string concatenation
- Can use tagged templates

---

## Related Topics

- [[What-is-Template-Literal]] - [[What-is-Template-Literal|Template literals]] overview
- [[Template-Literals]] - [[Template-Literals|Using template literals]]
- [[What-is-String]] - [[What-is-String|Strings]]
- [[Create-String]] - [[Create-String|Creating strings]]
