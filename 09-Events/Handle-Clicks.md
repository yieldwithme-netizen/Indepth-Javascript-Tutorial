# Handle Clicks

## Definition

Click events fire when an element is clicked. Use `addEventListener('click', handler)` to respond to user clicks on buttons, links, or any clickable element.

## Code Examples

### Basic Click Handler

```javascript
const button = document.getElementById('myButton');

button.addEventListener('click', () => {
  console.log('Button was clicked!');
});
```

### Click with Event Object

```javascript
button.addEventListener('click', (event) => {
  console.log('Clicked element:', event.target);
  console.log('Mouse position:', event.clientX, event.clientY);
});
```

### Double Click

```javascript
button.addEventListener('dblclick', () => {
  console.log('Double clicked!');
});
```

### Event Delegation

```javascript
document.querySelector('.list').addEventListener('click', (event) => {
  if (event.target.matches('.list-item')) {
    console.log('Item clicked:', event.target.textContent);
  }
});
```

### Dynamic Element Handling

```javascript
document.body.addEventListener('click', (event) => {
  if (event.target.matches('.dynamic-button')) {
    event.target.classList.toggle('active');
  }
});
```

## Common Use Cases

1. **Button actions** - Submit forms, toggle features
2. **Navigation** - Handle menu clicks
3. **UI interactions** - Modal open/close, accordion toggle
4. **Form elements** - Custom checkboxes, radio buttons

## Common Mistakes

```javascript
// Wrong: Adding multiple listeners with same function
button.addEventListener('click', handleClick);
button.addEventListener('click', handleClick); // Duplicate

// Correct: Use once option if needed
button.addEventListener('click', handleClick, { once: true });

// Wrong: Forgetting to use event delegation for dynamic elements
document.querySelectorAll('.button').forEach(btn => {
  btn.addEventListener('click', handler); // Won't work for new buttons
});
```

## Related Topics

- [[Use-Event-Props]]
- [[Stop-Propagation]]
- [[Prevent-Default]]
- [[Handle-Keys]]

## Quick Revision

| Event | When It Fires |
|-------|---------------|
| `click` | Single click |
| `dblclick` | Double click |
| `mousedown` | Mouse button pressed |
| `mouseup` | Mouse button released |
| Use delegation | For dynamically created elements |
