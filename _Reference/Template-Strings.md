# Template Strings

## Definition

Template strings (template literals) use **backticks** for strings with embedded expressions and multi-line support.

## Basic Syntax

```javascript
// Regular string
const name = "John";
const msg1 = "Hello, " + name + "!";

// Template string
const msg2 = `Hello, ${name}!`;
```

## Expressions

```javascript
const x = 10;
const y = 5;

console.log(`Sum: ${x + y}`); // "Sum: 15"
console.log(`Product: ${x * y}`); // "Product: 50"
console.log(`Is large: ${x > 10 ? "yes" : "no"}`);
```

## Multi-line

```javascript
// Old way
const html1 = "<div>\n" +
              "  <h1>Hello</h1>\n" +
              "</div>";

// Template string
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
// "Hello <b>John</b>, welcome!"
```

## Quick Revision

- Template strings use backticks
- Embed with `${expression}`
- Support multi-line
- Better than concatenation
- Can use tagged templates

---

## Related Topics

- [[What-is-Template-Literal]] - [[What-is-Template-Literal|Template literals]] overview
- [[Template-Literals]] - [[Template-Literals|Using template literals]]
- [[What-is-String]] - [[What-is-String|Strings]]
- [[Create-String]] - [[Create-String|Creating strings]]
