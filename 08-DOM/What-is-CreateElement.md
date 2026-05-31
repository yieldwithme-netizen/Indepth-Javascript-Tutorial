# createElement

## Definition

`createElement()` creates a new HTML element in memory that you can then add to the DOM. The element is not visible until you append it to the document tree. This is the fundamental way to dynamically build page content.

## Basic Syntax

```javascript
const element = document.createElement('tagName');
```

## Creating Elements

```javascript
// Create a div
const div = document.createElement('div');

// Create a paragraph
const p = document.createElement('p');

// Create a button
const button = document.createElement('button');

// Create a link
const link = document.createElement('a');
```

## Setting Properties

```javascript
const link = document.createElement('a');

// Set attributes
link.href = 'https://example.com';
link.textContent = 'Click Here';
link.target = '_blank';
link.className = 'nav-link';
link.id = 'main-link';

// Set data attributes
link.dataset.userId = '123';

// Add event listener
link.addEventListener('click', (e) => {
  e.preventDefault();
  console.log('Link clicked');
});
```

## Building a Complete Element

```javascript
function createProductCard(product) {
  const card = document.createElement('div');
  card.className = 'product-card';
  card.dataset.productId = product.id;

  card.innerHTML = `
    <img src="${product.image}" alt="${product.name}">
    <h3>${product.name}</h3>
    <p class="price">$${product.price}</p>
    <p class="description">${product.description}</p>
    <button class="add-to-cart">Add to Cart</button>
  `;

  // Add event listener after setting innerHTML
  const button = card.querySelector('.add-to-cart');
  button.addEventListener('click', () => {
    addToCart(product.id);
  });

  return card;
}

// Usage
const container = document.getElementById('products');
products.forEach(product => {
  const card = createProductCard(product);
  container.appendChild(card);
});
```

## Practical Examples

### Dynamic List Item

```javascript
function createListItem(text) {
  const li = document.createElement('li');
  li.textContent = text;

  const deleteBtn = document.createElement('button');
  deleteBtn.textContent = '×';
  deleteBtn.className = 'delete-btn';
  deleteBtn.addEventListener('click', () => {
    li.remove();
  });

  li.appendChild(deleteBtn);
  return li;
}

// Usage
const list = document.getElementById('todo-list');
const newItem = createListItem('Learn JavaScript');
list.appendChild(newItem);
```

### Modal Dialog

```javascript
function createModal(title, content) {
  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';

  const modal = document.createElement('div');
  modal.className = 'modal';

  modal.innerHTML = `
    <div class="modal-header">
      <h2>${title}</h2>
      <button class="close-btn">&times;</button>
    </div>
    <div class="modal-body">
      ${content}
    </div>
  `;

  overlay.appendChild(modal);

  // Close on overlay click
  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) {
      overlay.remove();
    }
  });

  // Close button
  modal.querySelector('.close-btn').addEventListener('click', () => {
    overlay.remove();
  });

  document.body.appendChild(overlay);
  return overlay;
}

// Usage
createModal('Confirm', '<p>Are you sure?</p>');
```

### Building a Form

```javascript
function createForm(fields) {
  const form = document.createElement('form');
  form.className = 'dynamic-form';

  fields.forEach(field => {
    const group = document.createElement('div');
    group.className = 'form-group';

    const label = document.createElement('label');
    label.textContent = field.label;
    label.htmlFor = field.name;

    let input;
    if (field.type === 'textarea') {
      input = document.createElement('textarea');
    } else {
      input = document.createElement('input');
      input.type = field.type || 'text';
    }

    input.name = field.name;
    input.id = field.name;
    input.placeholder = field.placeholder || '';
    input.required = field.required || false;

    group.appendChild(label);
    group.appendChild(input);
    form.appendChild(group);
  });

  const submitBtn = document.createElement('button');
  submitBtn.type = 'submit';
  submitBtn.textContent = 'Submit';
  form.appendChild(submitBtn);

  return form;
}

// Usage
const myForm = createForm([
  { label: 'Name', name: 'name', type: 'text', required: true },
  { label: 'Email', name: 'email', type: 'email', required: true },
  { label: 'Message', name: 'message', type: 'textarea' }
]);
document.getElementById('form-container').appendChild(myForm);
```

### Notification System

```javascript
function createNotification(message, type = 'info') {
  const notification = document.createElement('div');
  notification.className = `notification notification-${type}`;
  notification.textContent = message;

  const closeBtn = document.createElement('button');
  closeBtn.className = 'notification-close';
  closeBtn.textContent = '×';
  closeBtn.addEventListener('click', () => {
    notification.remove();
  });

  notification.appendChild(closeBtn);

  const container = document.getElementById('notifications');
  container.appendChild(notification);

  // Auto-remove after 5 seconds
  setTimeout(() => {
    if (notification.parentNode) {
      notification.remove();
    }
  }, 5000);

  return notification;
}
```

## Fragment for Performance

```javascript
// BAD: Multiple reflows
for (let i = 0; i < 100; i++) {
  const item = document.createElement('li');
  item.textContent = `Item ${i}`;
  list.appendChild(item); // Reflow each time
}

// GOOD: Single reflow with DocumentFragment
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
  const item = document.createElement('li');
  item.textContent = `Item ${i}`;
  fragment.appendChild(item); // No reflow yet
}
list.appendChild(fragment); // Single reflow
```

## Common Use Cases

| Use Case | Code |
|----------|------|
| Create element | `document.createElement('div')` |
| Set text | `el.textContent = 'text'` |
| Set HTML | `el.innerHTML = '<p>html</p>'` |
| Set attributes | `el.setAttribute('id', 'x')` |
| Add to DOM | `parent.appendChild(el)` |
| Use fragment | `document.createDocumentFragment()`` |

## Common Mistakes to Avoid

1. **Not appending the element** — createElement only creates in memory
2. **Multiple reflows** — Use DocumentFragment for bulk additions
3. **Adding innerHTML before appending** — Works but less efficient

```javascript
// WRONG: Element never added to DOM
const div = document.createElement('div');
div.textContent = 'Hello';
// Forgot to append!

// RIGHT: Create and append
const div = document.createElement('div');
div.textContent = 'Hello';
document.body.appendChild(div);
```

## Related Topics

- [[What-is-AppendChild]]
- [[What-is-RemoveChild]]
- [[What-is-InnerHTML]]
- [[What-is-ClassList]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| `createElement()` | Create new element |
| `createDocumentFragment()` | Create lightweight container for bulk adds |
| `appendChild()` | Add element to DOM |
| `textContent` | Set text content |
| `innerHTML` | Set HTML content |
| `setAttribute()` | Set element attribute |
