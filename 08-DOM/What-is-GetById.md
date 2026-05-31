# getElementById and Selection Methods

## Definition

`getElementById()` selects a single DOM element by its `id` attribute. It is the fastest and simplest way to select a specific element. The DOM also provides other selection methods like `getElementsByClassName()` and `getElementsByTagName()`.

## Basic Syntax

```javascript
const element = document.getElementById('myId');
```

## Selection Methods Overview

```html
<div id="main" class="container">
  <p class="text">First paragraph</p>
  <p class="text highlight">Second paragraph</p>
  <span>Span element</span>
</div>
```

```javascript
// By ID — returns single element
const main = document.getElementById('main');

// By class — returns HTMLCollection (live)
const texts = document.getElementsByClassName('text');

// By tag — returns HTMLCollection (live)
const paragraphs = document.getElementsByTagName('p');
```

## getElementById

```javascript
const header = document.getElementById('header');
console.log(header); // <div id="header">...</div>

// Returns null if not found
const missing = document.getElementById('nonexistent');
console.log(missing); // null
```

### Practical Example

```javascript
const submitBtn = document.getElementById('submit-btn');

submitBtn.addEventListener('click', () => {
  const form = document.getElementById('contact-form');
  form.submit();
});
```

## getElementsByClassName

```javascript
const items = document.getElementsByClassName('list-item');

// HTMLCollection is LIVE — updates automatically
console.log(items.length); // 3

// Loop through collection
for (let i = 0; i < items.length; i++) {
  console.log(items[i].textContent);
}

// Convert to array for forEach
Array.from(items).forEach(item => {
  item.classList.add('loaded');
});
```

## getElementsByTagName

```javascript
const allDivs = document.getElementsByTagName('div');
console.log(allDivs.length); // All divs in the document

// Specific to a parent element
const container = document.getElementById('container');
const containerDivs = container.getElementsByTagName('div');
```

## querySelector vs getElementById

```javascript
// Both select by ID
const byId = document.getElementById('header');
const bySelector = document.querySelector('#header');

// getElementById is faster but less flexible
// querySelector supports any CSS selector
```

## Comparison Table

| Method | Returns | Live? | Selector Type |
|--------|---------|-------|---------------|
| `getElementById()` | Single element | No | ID only |
| `getElementsByClassName()` | HTMLCollection | **Yes** | Class name |
| `getElementsByTagName()` | HTMLCollection | **Yes** | Tag name |
| `querySelector()` | Single element | No | CSS selector |
| `querySelectorAll()` | Static NodeList | No | CSS selector |

## Live vs Static Collections

```javascript
// LIVE — automatically updates when DOM changes
const liveCollection = document.getElementsByClassName('item');
console.log(liveCollection.length); // 3

// Add new element
const newItem = document.createElement('li');
newItem.className = 'item';
document.getElementById('list').appendChild(newItem);

console.log(liveCollection.length); // 4 — updated automatically

// STATIC — snapshot of DOM at time of query
const staticList = document.querySelectorAll('.item');
console.log(staticList.length); // 3

document.getElementById('list').appendChild(newItem);
console.log(staticList.length); // Still 3 — does not update
```

## Practical Examples

### Highlight Current Section

```javascript
const sections = document.getElementsByTagName('section');

Array.from(sections).forEach(section => {
  section.addEventListener('click', () => {
    // Remove highlight from all
    Array.from(sections).forEach(s => s.classList.remove('highlighted'));
    // Add to clicked
    section.classList.add('highlighted');
  });
});
```

### Form Field Validation

```javascript
const nameField = document.getElementById('name');
const emailField = document.getElementById('email');

function validateForm() {
  let isValid = true;

  if (!nameField.value.trim()) {
    nameField.classList.add('error');
    isValid = false;
  } else {
    nameField.classList.remove('error');
  }

  if (!emailField.value.includes('@')) {
    emailField.classList.add('error');
    isValid = false;
  } else {
    emailField.classList.remove('error');
  }

  return isValid;
}
```

### Dynamic Content Loading

```javascript
const loader = document.getElementById('loader');
const content = document.getElementById('content');

async function loadContent() {
  loader.style.display = 'block';

  const response = await fetch('/api/data');
  const data = await response.json();

  content.innerHTML = data.html;
  loader.style.display = 'none';
}
```

## Common Use Cases

| Use Case | Recommended Method |
|----------|-------------------|
| Select one unique element | `getElementById()` |
| Select elements by class | `querySelectorAll()` or `getElementsByClassName()` |
| Complex selection logic | `querySelector()` |
| Need live updates | `getElementsByClassName()` |
| Need array methods | `querySelectorAll()` |

## Common Mistakes to Avoid

1. **ID must be unique** — Only one element should have each ID
2. **Forgetting the hash** — `getElementById('myId')` not `getElementById('#myId')`
3. **HTMLCollection is live** — Unexpected behavior if DOM changes during iteration

```javascript
// WRONG: Including hash symbol
const el = document.getElementById('#myId'); // Returns null

// RIGHT: No hash
const el = document.getElementById('myId');
```

## Related Topics

- [[What-is-QuerySelector]]
- [[What-is-Traversal]]
- [[What-is-CreateElement]]
- [[What-is-ClassList]]

## Quick Revision

| Method | Best For |
|--------|----------|
| `getElementById()` | Fast, unique element selection |
| `getElementsByClassName()` | Live collection by class |
| `getElementsByTagName()` | Live collection by tag |
| `querySelector()` | First element with CSS selector |
| `querySelectorAll()` | All elements with CSS selector |
