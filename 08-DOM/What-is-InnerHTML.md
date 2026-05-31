# innerHTML

## Definition

`innerHTML` gets or sets the HTML content inside an element. It returns a string of all the HTML markup (including child elements) within an element, and you can assign new HTML to replace or add content.

## Basic Syntax

```javascript
// Get HTML content
const html = element.innerHTML;

// Set HTML content (replaces existing)
element.innerHTML = '<p>New content</p>';
```

## Getting innerHTML

```html
<div id="content">
  <h2>Title</h2>
  <p>Hello <strong>World</strong></p>
</div>
```

```javascript
const content = document.getElementById('content');
console.log(content.innerHTML);
// Output:
// <h2>Title</h2>
// <p>Hello <strong>World</strong></p>
```

## Setting innerHTML

```javascript
const container = document.getElementById('container');

// Replace all content with new HTML
container.innerHTML = `
  <h1>New Title</h1>
  <p>New paragraph with <a href="#">link</a></p>
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
`;
```

## Appending with innerHTML

```javascript
// WRONG: Replaces all existing content
container.innerHTML = '<p>New paragraph</p>';

// RIGHT: Append to existing content
container.innerHTML += '<p>New paragraph</p>';

// BETTER: Use insertAdjacentHTML (see below)
container.insertAdjacentHTML('beforeend', '<p>New paragraph</p>');
```

## Creating Dynamic Content

### Dynamic List

```javascript
const items = ['Apple', 'Banana', 'Cherry'];
const list = document.getElementById('fruit-list');

list.innerHTML = items
  .map(item => `<li>${item}</li>`)
  .join('');
```

### Dynamic Table

```javascript
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 }
];

const table = document.getElementById('user-table');

table.innerHTML = `
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    ${users.map(user => `
      <tr>
        <td>${user.name}</td>
        <td>${user.age}</td>
      </tr>
    `).join('')}
  </tbody>
`;
```

### Dynamic Card Grid

```javascript
const products = [
  { id: 1, name: 'Laptop', price: 999 },
  { id: 2, name: 'Phone', price: 699 },
  { id: 3, name: 'Tablet', price: 499 }
];

const grid = document.getElementById('product-grid');

grid.innerHTML = products.map(product => `
  <div class="card">
    <h3>${product.name}</h3>
    <p>$${product.price}</p>
    <button onclick="addToCart(${product.id})">Add to Cart</button>
  </div>
`).join('');
```

## insertAdjacentHTML — Better than innerHTML for Appending

```javascript
// Insert at different positions
element.insertAdjacentHTML('beforebegin', html);  // Before the element
element.insertAdjacentHTML('afterbegin', html);   // Inside, at start
element.insertAdjacentHTML('beforeend', html);    // Inside, at end
element.insertAdjacentHTML('afterend', html);     // After the element

// Example: Append without replacing
container.insertAdjacentHTML('beforeend', '<p>New paragraph</p>');
```

## Security Considerations — XSS Vulnerability

```javascript
// DANGEROUS: Never use innerHTML with user input
const userInput = '<img src="x" onerror="alert(\'hacked\')">';
element.innerHTML = userInput; // Executes the script!

// SAFE: Use textContent for user input
element.textContent = userInput; // Shows as plain text

// SAFE: Sanitize HTML before inserting
function sanitize(html) {
  const div = document.createElement('div');
  div.textContent = html;
  return div.innerHTML;
}

element.innerHTML = sanitize(userInput);
```

## Common Use Cases

| Use Case | Code |
|----------|------|
| Set content | `el.innerHTML = '<p>Hello</p>'` |
| Get content | `el.innerHTML` |
| Append content | `el.innerHTML += '<p>More</p>'` |
| Clear content | `el.innerHTML = ''` |
| Insert at position | `el.insertAdjacentHTML('beforeend', html)` |

## Common Mistakes to Avoid

1. **XSS attacks** — Never use innerHTML with unsanitized user input
2. **Performance** — innerHTML reparses all HTML; use DOM methods for small changes
3. **Event listeners lost** — innerHTML replaces elements, destroying attached listeners
4. **Appending incorrectly** — `+=` replaces and re-adds; use `insertAdjacentHTML`

```javascript
// WRONG: Loses event listeners
container.innerHTML = '<button>Click</button>';
container.innerHTML += '<p>More</p>'; // Button listener gone!

// RIGHT: Use insertAdjacentHTML
container.insertAdjacentHTML('beforeend', '<p>More</p>');
```

## Related Topics

- [[What-is-TextContent]]
- [[What-is-CreateElement]]
- [[What-is-AppendChild]]
- [[What-is-Style]]

## Quick Revision

| Property | Returns | Use Case |
|----------|---------|----------|
| `innerHTML` | HTML string including tags | When you need markup |
| `textContent` | Plain text only | When you need text |
| `innerText` | Rendered text | When you need visible text |
| `insertAdjacentHTML` | — | Appending HTML without replacing |
