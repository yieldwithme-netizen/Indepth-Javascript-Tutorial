# appendChild, append, and prepend

## Definition

These methods add elements to the DOM. `appendChild()` adds a child to the end of a parent. `append()` and `prepend()` are newer methods that can add multiple elements and text in one call.

## Basic Syntax

```javascript
// appendChild — adds to end
parent.appendChild(child);

// append — adds to end (more flexible)
parent.append(child1, child2, 'text');

// prepend — adds to beginning
parent.prepend(child1, child2, 'text');
```

## appendChild

```javascript
const container = document.getElementById('container');
const newDiv = document.createElement('div');
newDiv.textContent = 'Hello World';

// Add to end of container
container.appendChild(newDiv);
```

### Moving Elements

```javascript
const listA = document.getElementById('list-a');
const listB = document.getElementById('list-b');
const item = document.getElementById('item-1');

// Moving from listA to listB
// Element is removed from listA automatically
listB.appendChild(item);
// item is now in listB, removed from listA
```

## append — More Flexible

```javascript
const container = document.getElementById('container');

// Add multiple elements at once
const div1 = document.createElement('div');
const div2 = document.createElement('div');
container.append(div1, div2);

// Add text and elements together
container.append('Some text', div1, 'More text');

// Append HTML string (converted to text, not parsed)
container.append('<p>This shows as plain text</p>');
```

## prepend — Add to Beginning

```javascript
const list = document.getElementById('list');

const firstItem = document.createElement('li');
firstItem.textContent = 'First Item';

// Add to beginning of list
list.prepend(firstItem);

// Prepend multiple items
const secondItem = document.createElement('li');
secondItem.textContent = 'Second Item';
list.prepend(secondItem, firstItem);
```

## Practical Examples

### Dynamic Todo List

```javascript
const input = document.getElementById('todo-input');
const addBtn = document.getElementById('add-btn');
const list = document.getElementById('todo-list');

addBtn.addEventListener('click', () => {
  const text = input.value.trim();
  if (!text) return;

  const li = document.createElement('li');
  li.className = 'todo-item';

  const span = document.createElement('span');
  span.textContent = text;

  const deleteBtn = document.createElement('button');
  deleteBtn.textContent = '×';
  deleteBtn.className = 'delete-btn';
  deleteBtn.addEventListener('click', () => li.remove());

  li.append(span, deleteBtn);
  list.prepend(li); // New items at top

  input.value = '';
  input.focus();
});
```

### Chat Message System

```javascript
const chatContainer = document.getElementById('chat');
const input = document.getElementById('message-input');
const sendBtn = document.getElementById('send-btn');

function addMessage(text, sender) {
  const message = document.createElement('div');
  message.className = `message message-${sender}`;

  const bubble = document.createElement('div');
  bubble.className = 'message-bubble';
  bubble.textContent = text;

  const time = document.createElement('span');
  time.className = 'message-time';
  time.textContent = new Date().toLocaleTimeString();

  message.append(bubble, time);
  chatContainer.append(message);

  // Auto-scroll to bottom
  chatContainer.scrollTop = chatContainer.scrollHeight;
}

sendBtn.addEventListener('click', () => {
  const text = input.value.trim();
  if (!text) return;

  addMessage(text, 'sent');
  input.value = '';

  // Simulate response
  setTimeout(() => {
    addMessage('Thanks for your message!', 'received');
  }, 1000);
});
```

### Loading Indicator

```javascript
function showLoading(container) {
  const loader = document.createElement('div');
  loader.className = 'loading-spinner';
  loader.innerHTML = `
    <div class="spinner"></div>
    <p>Loading...</p>
  `;
  container.prepend(loader);
  return loader;
}

function hideLoading(loader) {
  loader.remove();
}

// Usage
const content = document.getElementById('content');
const loader = showLoading(content);

// After data loads
const data = await fetchData();
loader.remove();
```

### Pagination

```javascript
const pagination = document.getElementById('pagination');
const totalPages = 10;
let currentPage = 1;

function renderPagination() {
  pagination.innerHTML = '';

  // Previous button
  if (currentPage > 1) {
    const prev = document.createElement('button');
    prev.textContent = '← Prev';
    prev.addEventListener('click', () => {
      currentPage--;
      renderPagination();
    });
    pagination.appendChild(prev);
  }

  // Page numbers
  for (let i = 1; i <= totalPages; i++) {
    const page = document.createElement('button');
    page.textContent = i;
    page.className = i === currentPage ? 'active' : '';
    page.addEventListener('click', () => {
      currentPage = i;
      renderPagination();
    });
    pagination.appendChild(page);
  }

  // Next button
  if (currentPage < totalPages) {
    const next = document.createElement('button');
    next.textContent = 'Next →';
    next.addEventListener('click', () => {
      currentPage++;
      renderPagination();
    });
    pagination.appendChild(next);
  }
}

renderPagination();
```

## Comparison Table

| Method | Adds Multiple? | Adds Text? | Returns |
|--------|----------------|------------|---------|
| `appendChild()` | No | No | The appended child |
| `append()` | Yes | Yes | void |
| `prepend()` | Yes | Yes | void |
| `insertBefore()` | No | No | The appended child |
| `after()` | Yes | Yes | void |
| `before()` | Yes | Yes | void |

## DocumentFragment for Performance

```javascript
// Multiple appendChild calls = multiple reflows
for (let i = 0; i < 1000; i++) {
  const li = document.createElement('li');
  li.textContent = `Item ${i}`;
  list.appendChild(li); // Reflow each time
}

// Single append with fragment = one reflow
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
  const li = document.createElement('li');
  li.textContent = `Item ${i}`;
  fragment.appendChild(li);
}
list.appendChild(fragment); // Single reflow
```

## Common Use Cases

| Use Case | Method |
|----------|--------|
| Add element to end | `appendChild()` or `append()` |
| Add element to start | `prepend()` |
| Add multiple elements | `append()` or `prepend()` |
| Add text and elements | `append()` |
| Move element between parents | `appendChild()` |
| Insert before specific element | `insertBefore()` |

## Common Mistakes to Avoid

1. **Not removing from old parent** — appendChild moves, doesn't copy
2. **Using innerHTML for adding elements** — Use DOM methods for better performance
3. **Not checking if parent exists** — Throws error if parent is null

```javascript
// WRONG: Losing event listeners
container.innerHTML += '<button>Click</button>';

// RIGHT: Create and append
const button = document.createElement('button');
button.textContent = 'Click';
button.addEventListener('click', handler);
container.appendChild(button);
```

## Related Topics

- [[What-is-RemoveChild]]
- [[What-is-CreateElement]]
- [[What-is-InnerHTML]]
- [[What-is-Traversal]]

## Quick Revision

| Method | Position | Multiple? | Text? |
|--------|----------|-----------|-------|
| `appendChild()` | End | No | No |
| `append()` | End | Yes | Yes |
| `prepend()` | Start | Yes | Yes |
| `insertBefore()` | Before reference | No | No |
| `after()` | After element | Yes | Yes |
| `before()` | Before element | Yes | Yes |
