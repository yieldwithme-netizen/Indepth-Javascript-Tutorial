# DOM Traversal

## Definition

DOM Traversal is the ability to navigate through the HTML document tree by accessing parent, child, or sibling elements relative to a starting node. Think of it as moving through a family tree — each element is a node with relationships to other nodes.

## The DOM Tree Structure

```html
<div id="parent">
  <p id="child1">First paragraph</p>
  <p id="child2">Second paragraph</p>
  <span id="child3">Span element</span>
</div>
```

In this structure:
- `<div>` is the **parent** of `<p>` and `<span>`
- `<p>` elements are **siblings** to each other
- Text nodes are **children** of their parent elements

## Parent Traversal

```javascript
// Get a reference element
const child = document.getElementById('child1');

// Move to the parent element
const parent = child.parentElement;        // <div id="parent">
const parentNode = child.parentNode;        // Same, but includes text nodes
const closest = child.closest('#parent');   // Finds nearest ancestor matching selector
```

### parentNode vs parentElement

```javascript
// parentNode includes all node types (elements, text, comments)
// parentElement only returns element nodes
const div = document.getElementById('child1');
console.log(div.parentNode);     // Could be a text node
console.log(div.parentElement);  // Returns the <div> element
```

## Child Traversal

```javascript
const parent = document.getElementById('parent');

// Access all child nodes (includes text nodes)
const allNodes = parent.childNodes;        // NodeList

// Access only element children
const allChildren = parent.children;       // HTMLCollection

// Access first and last child
const first = parent.firstChild;           // First node (may be text)
const firstEl = parent.firstElementChild;  // First element only
const last = parent.lastChild;             // Last node
const lastEl = parent.lastElementChild;    // Last element only
```

### Practical Example

```javascript
const ul = document.getElementById('menu');

// Loop through all children
for (let child of ul.children) {
  console.log(child.textContent);
}

// Get specific child by index
const secondItem = ul.children[1];
console.log(secondItem.textContent); // "Second item"
```

## Sibling Traversal

```javascript
const current = document.getElementById('child2');

// Access siblings
const next = current.nextElementSibling;     // <span id="child3">
const prev = current.previousElementSibling; // <p id="child1">
const nextNode = current.nextSibling;        // Next node (may be text)
const prevNode = current.previousSibling;    // Previous node
```

### Practical Example — Navigation Menu

```javascript
const menuItems = document.querySelectorAll('.menu-item');
let currentIndex = 0;

function navigateTo(index) {
  menuItems[currentIndex].classList.remove('active');
  currentIndex = index;
  menuItems[currentIndex].classList.add('active');
}

// Navigate to next item
function goNext() {
  const nextIndex = (currentIndex + 1) % menuItems.length;
  navigateTo(nextIndex);
}

// Navigate to previous item
function goPrev() {
  const prevIndex = (currentIndex - 1 + menuItems.length) % menuItems.length;
  navigateTo(prevIndex);
}
```

## Complete Traversal Example

```html
<div id="root">
  <header>
    <h1>Title</h1>
    <nav>
      <a href="#">Home</a>
      <a href="#">About</a>
    </nav>
  </header>
  <main>
    <article>
      <h2>Article Title</h2>
      <p>First paragraph</p>
      <p>Second paragraph</p>
    </article>
    <aside>
      <p>Sidebar content</p>
    </aside>
  </main>
  <footer>
    <p>Footer text</p>
  </footer>
</div>
```

```javascript
const root = document.getElementById('root');

// Get the main content area
const main = root.children[1]; // <main>

// Get the article
const article = main.firstElementChild;

// Traverse from article to footer
const footer = root.lastElementChild;

// Get all paragraphs in article
const paragraphs = article.querySelectorAll('p');

// Traverse backwards from last paragraph
const lastP = paragraphs[paragraphs.length - 1];
const secondP = lastP.previousElementSibling;
console.log(secondP.textContent); // "First paragraph"
```

## Common Use Cases

| Use Case | Method |
|----------|--------|
| Get parent element | `element.parentElement` |
| Get all children (elements only) | `element.children` |
| Get first child element | `element.firstElementChild` |
| Get next sibling | `element.nextElementSibling` |
| Find ancestor by selector | `element.closest('.class')` |
| Navigate siblings | Loop with `nextElementSibling` |

## Common Mistakes to Avoid

1. **Confusing nodes with elements** — `childNodes` includes text nodes; `children` only includes elements
2. **Using `firstChild` instead of `firstElementChild`** — Whitespace between elements creates text nodes
3. **Not null-checking** — Siblings may be `null` if at the boundary

```javascript
// WRONG: May include text nodes
const kids = element.childNodes;

// RIGHT: Only elements
const kids = element.children;
```

## Related Topics

- [[What-is-GetById]]
- [[What-is-QuerySelector]]
- [[What-is-CreateElement]]
- [[What-is-AppendChild]]
- [[What-is-RemoveChild]]

## Quick Revision

| Property | Returns |
|----------|---------|
| `parentNode` | Parent node (any type) |
| `parentElement` | Parent element |
| `children` | All child elements |
| `firstElementChild` | First child element |
| `lastElementChild` | Last child element |
| `nextElementSibling` | Next sibling element |
| `previousElementSibling` | Previous sibling element |
| `closest(selector)` | Nearest ancestor matching selector |
