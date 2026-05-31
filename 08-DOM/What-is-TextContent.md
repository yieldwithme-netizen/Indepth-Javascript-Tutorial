# textContent vs innerText

## Definition

`textContent` and `innerText` both get or set the text content of an element, but they differ in how they handle hidden elements, CSS, and performance. `textContent` returns all text including hidden elements, while `innerText` only returns visible text.

## Basic Syntax

```javascript
// Get text
const text = element.textContent;
const visibleText = element.innerText;

// Set text (both work the same)
element.textContent = 'New text';
element.innerText = 'New text';
```

## Key Differences

```html
<div id="example">
  <p>Hello World</p>
  <p style="display: none;">Hidden Text</p>
  <p>Visible Text</p>
</div>
```

```javascript
const example = document.getElementById('example');

// textContent — returns ALL text (including hidden)
console.log(example.textContent);
// Output: "Hello World Hidden Text Visible Text"

// innerText — returns only VISIBLE text
console.log(example.innerText);
// Output: "Hello World Visible Text"
```

## Performance Comparison

```javascript
// textContent is FASTER — doesn't trigger CSS layout calculations
// innerText is SLOWER — needs to compute what's visible

// Use textContent when possible for better performance
const fast = element.textContent;    // Fast
const slow = element.innerText;      // Slower
```

## Getting Text

```html
<div id="user-card">
  <h2>John Doe</h2>
  <p class="email">john@example.com</p>
  <span class="hidden-id">user-12345</span>
</div>
```

```javascript
const card = document.getElementById('user-card');

// Get all text including hidden
console.log(card.textContent);
// "John Doe john@example.com user-12345"

// Get only visible text
console.log(card.innerText);
// "John Doe john@example.com"
```

## Setting Text

```javascript
const paragraph = document.getElementById('message');

// Both replace content with plain text (no HTML parsing)
paragraph.textContent = 'Hello World';
paragraph.innerText = 'Hello World';

// HTML is NOT parsed — shown as literal text
paragraph.textContent = '<strong>Bold</strong>';
// Shows: <strong>Bold</strong> (as text, not bold)

// WRONG: innerHTML for setting text
paragraph.innerHTML = '<strong>Bold</strong>'; // Actually makes it bold
```

## Practical Examples

### Extracting Article Text

```javascript
const article = document.getElementById('article');

// Get clean text from article
const text = article.textContent;

// Get word count
const wordCount = text.trim().split(/\s+/).length;
console.log(`Word count: ${wordCount}`);
```

### Search and Highlight

```javascript
function highlightText(element, searchTerm) {
  const walker = document.createTreeWalker(
    element,
    NodeFilter.SHOW_TEXT,
    null,
    false
  );

  let node;
  while (node = walker.nextNode()) {
    if (node.textContent.includes(searchTerm)) {
      const span = document.createElement('span');
      span.className = 'highlight';
      span.textContent = node.textContent;
      node.parentNode.replaceChild(span, node);
    }
  }
}
```

### Copying Text to Clipboard

```javascript
async function copyToClipboard(elementId) {
  const element = document.getElementById(elementId);
  const text = element.textContent;

  try {
    await navigator.clipboard.writeText(text);
    alert('Text copied!');
  } catch (err) {
    console.error('Failed to copy:', err);
  }
}
```

### Character Counter

```javascript
const textarea = document.getElementById('bio');
const counter = document.getElementById('char-count');
const maxChars = 280;

textarea.addEventListener('input', () => {
  const count = textarea.value.length;
  counter.textContent = `${count}/${maxChars}`;

  if (count > maxChars) {
    counter.classList.add('over-limit');
  } else {
    counter.classList.remove('over-limit');
  }
});
```

## textContent vs innerText vs innerHTML

| Property | Returns HTML? | Includes Hidden? | Performance |
|----------|---------------|------------------|-------------|
| `textContent` | No | Yes | Fast |
| `innerText` | No | No | Slower |
| `innerHTML` | Yes | Yes | Medium |

## When to Use What

```javascript
// Use textContent when:
// 1. You just need the text
// 2. Performance matters
// 3. You want hidden text too
const text = element.textContent;

// Use innerText when:
// 1. You only want visible text
// 2. You need computed styles
const visibleText = element.innerText;

// Use innerHTML when:
// 1. You need to parse HTML
// 2. You're setting HTML content
element.innerHTML = '<p>Hello</p>';
```

## Common Use Cases

| Use Case | Recommended |
|----------|-------------|
| Read text | `textContent` |
| Set plain text | `textContent` |
| Get visible text only | `innerText` |
| Word/char count | `textContent` |
| Copy to clipboard | `textContent` |

## Common Mistakes to Avoid

1. **Assuming innerText is faster** — textContent is faster
2. **Using innerHTML for text** — Security risk and slower
3. **Forgetting hidden text** — textContent includes everything

```javascript
// WRONG: Using innerHTML to set text
element.innerHTML = 'Hello'; // Parses HTML (unnecessary)

// RIGHT: Use textContent
element.textContent = 'Hello';
```

## Related Topics

- [[What-is-InnerHTML]]
- [[What-is-CreateElement]]
- [[What-is-Style]]
- [[What-is-ClassList]]

## Quick Revision

| Property | Visible Only | Fast | Use When |
|----------|-------------|------|----------|
| `textContent` | No | Yes | Default choice |
| `innerText` | Yes | No | Need visible text |
| `innerHTML` | N/A | Medium | Need HTML parsing |
