# Changing Text in the DOM

## Definition

Changing text content in the DOM allows you to dynamically update what users see on a webpage without reloading. JavaScript provides two main properties for this: `textContent` and `innerText`.

- **textContent**: Gets or sets the text content of a node, including all text within child elements
- **innerText**: Similar to textContent, but only returns "visible" text (respects CSS styling)

## Syntax

```javascript
// Using textContent
element.textContent = "New text here";

// Using innerText
element.innerText = "New text here";

// Reading current text
let currentText = element.textContent;
```

## Code Examples

### Basic Text Change

```javascript
// Change text of a paragraph
document.getElementById("demo").textContent = "Hello, World!";

// Change text with innerText
document.querySelector(".message").innerText = "Updated message!";
```

### Using Variables

```javascript
let userName = "John";
let greeting = `Welcome, ${userName}!`;

document.getElementById("greeting").textContent = greeting;
```

### TextContent vs InnerText

```javascript
// textContent - returns all text (including hidden)
<div id="example">
  Hello
  <span style="display:none">Hidden</span>
  World
</div>

element.textContent;  // "Hello Hidden World"
element.innerText;    // "Hello World" (skips hidden text)
```

### Updating Multiple Elements

```javascript
let paragraphs = document.querySelectorAll("p");

paragraphs.forEach(p => {
    p.textContent = "Updated paragraph";
});
```

## Common Use Cases

1. **Dynamic greetings**: Display user names after login
2. **Form validation messages**: Show error/success text
3. **Content updates**: Update news feeds, comments, notifications
4. **Counter displays**: Update scores, quantities, totals

## Common Mistakes

1. **Using innerHTML for text only** - Use textContent/innerText instead (more secure)
2. **Forgetting to select the element first** - Always ensure element exists before changing
3. **Using innerText in performance-critical code** - textContent is faster

## Related Topics

- [[Manage-Classes]] - Change element classes
- [[Change-Styles]] - Modify element styles
- [[Add-Listener]] - Handle user interactions
- [[Create-Elements]] - Create new DOM elements

## Quick Revision

| Property | Use Case | Performance |
|----------|----------|-------------|
| textContent | General text (all content) | Fast |
| innerText | Visible text only | Slower |

**Best Practice**: Use `textContent` for most cases unless you need to respect CSS visibility.
