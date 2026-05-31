# removeChild and Removing Elements

## Definition

Removing elements from the DOM is essential for dynamic pages. You can remove a child element from its parent, or use the modern `remove()` method to directly remove an element.

## Basic Syntax

```javascript
// Remove child from parent
parent.removeChild(child);

// Modern way — remove element directly
element.remove();
```

## removeChild

```javascript
const parent = document.getElementById('parent');
const child = document.getElementById('child');

// Remove child from parent
parent.removeChild(child);

// If parent reference is unknown
child.parentNode.removeChild(child);
```

## remove — Modern Method

```javascript
const element = document.getElementById('item');

// Remove directly
element.remove();

// Chained removal
document.getElementById('card').remove();
```

## Practical Examples

### Delete Todo Item

```javascript
const list = document.getElementById('todo-list');

// Using event delegation
list.addEventListener('click', (e) => {
  if (e.target.classList.contains('delete-btn')) {
    const item = e.target.closest('.todo-item');
    item.remove();
    saveTodos();
  }
});
```

### Confirmation Before Delete

```javascript
function deleteWithConfirm(element, message) {
  const confirmed = confirm(message || 'Are you sure?');
  if (confirmed) {
    element.remove();
    return true;
  }
  return false;
}

// Usage
document.querySelectorAll('.delete-btn').forEach(btn => {
  btn.addEventListener('click', (e) => {
    const item = e.target.closest('.item');
    deleteWithConfirm(item, 'Delete this item?');
  });
});
```

### Remove All Children

```javascript
const container = document.getElementById('container');

// Method 1: while loop
while (container.firstChild) {
  container.removeChild(container.firstChild);
}

// Method 2: innerHTML
container.innerHTML = '';

// Method 3: textContent
container.textContent = '';
```

### Animated Removal

```javascript
function removeWithAnimation(element, duration = 300) {
  return new Promise(resolve => {
    element.style.transition = `opacity ${duration}ms, transform ${duration}ms`;
    element.style.opacity = '0';
    element.style.transform = 'translateX(-100px)';

    setTimeout(() => {
      element.remove();
      resolve();
    }, duration);
  });
}

// Usage
const item = document.getElementById('item');
await removeWithAnimation(item);
```

### Remove Filtered Items

```javascript
const items = document.querySelectorAll('.list-item');

function removeByFilter(filterText) {
  items.forEach(item => {
    if (item.textContent.toLowerCase().includes(filterText.toLowerCase())) {
      item.remove();
    }
  });
}

// Usage
removeByFilter('completed');
```

### Remove with Delay

```javascript
function autoRemove(element, delay = 5000) {
  setTimeout(() => {
    if (element.parentNode) {
      element.style.transition = 'opacity 0.5s';
      element.style.opacity = '0';
      setTimeout(() => element.remove(), 500);
    }
  }, delay);
}

// Usage for notifications
function showNotification(message) {
  const notification = document.createElement('div');
  notification.className = 'notification';
  notification.textContent = message;
  document.getElementById('notifications').appendChild(notification);
  autoRemove(notification);
}
```

### Remove from Multiple Places

```javascript
function removeFromAll(selector) {
  document.querySelectorAll(selector).forEach(el => el.remove());
}

// Usage
removeFromAll('.temporary');
removeFromAll('[data-remove="true"]');
```

## Comparison Table

| Method | Browser Support | Returns | Removes Text Nodes |
|--------|-----------------|---------|-------------------|
| `element.remove()` | Modern browsers | void | Yes |
| `parent.removeChild()` | All browsers | removed child | Yes |
| `element.parentNode.remove()` | Modern browsers | void | Yes |

## Alternative Approaches

```javascript
// Hide instead of remove
element.style.display = 'none';

// Replace with empty
element.innerHTML = '';

// Replace with other element
parent.replaceChild(newElement, oldElement);

// Detach (jQuery pattern)
const detached = parent.removeChild(child);
// Later: parent.appendChild(detached);
```

## Common Use Cases

| Use Case | Method |
|----------|--------|
| Remove single element | `element.remove()` |
| Remove from parent | `parent.removeChild(child)` |
| Remove all children | `while (el.firstChild) el.removeChild(el.firstChild)` |
| Remove with animation | `setTimeout` + `remove()` |
| Remove filtered items | `querySelectorAll` + `forEach` + `remove()` |

## Common Mistakes to Avoid

1. **Removing without confirmation** — Always confirm destructive actions
2. **Not handling null references** — Check if element exists before removing
3. **Removing during iteration** — Collect elements first, then remove

```javascript
// WRONG: Modifying collection during iteration
const items = document.querySelectorAll('.item');
items.forEach(item => {
  if (item.classList.contains('old')) {
    item.remove(); // May skip items
  }
});

// RIGHT: Collect first, then remove
const itemsToRemove = document.querySelectorAll('.item.old');
itemsToRemove.forEach(item => item.remove());
```

## Related Topics

- [[What-is-CreateElement]]
- [[What-is-AppendChild]]
- [[What-is-Traversal]]
- [[What-is-Style]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| `element.remove()` | Remove element directly |
| `parent.removeChild(child)` | Remove child from parent |
| `el.innerHTML = ''` | Clear all children |
| `el.textContent = ''` | Clear all children |
| `while (el.firstChild) el.removeChild(el.firstChild)` | Remove all children |
