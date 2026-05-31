# Use Event Properties

## Definition

The event object contains properties that provide information about the event that occurred, such as the target element, mouse position, key pressed, and more.

## Code Examples

### Basic Event Object

```javascript
button.addEventListener('click', (event) => {
  console.log('Event type:', event.type);
  console.log('Target element:', event.target);
  console.log('Current element:', event.currentTarget);
});
```

### Mouse Event Properties

```javascript
document.addEventListener('click', (event) => {
  console.log('Mouse X:', event.clientX);
  console.log('Mouse Y:', event.clientY);
  console.log('Page X:', event.pageX);
  console.log('Page Y:', event.pageY);
  console.log('Screen X:', event.screenX);
  console.log('Screen Y:', event.screenY);
  console.log('Shift key:', event.shiftKey);
  console.log('Ctrl key:', event.ctrlKey);
});
```

### Keyboard Event Properties

```javascript
document.addEventListener('keydown', (event) => {
  console.log('Key:', event.key);
  console.log('Code:', event.code);
  console.log('Alt key:', event.altKey);
  console.log('Repeat:', event.repeat);
});
```

### Target vs CurrentTarget

```javascript
const parent = document.getElementById('parent');
const child = document.getElementById('child');

parent.addEventListener('click', (event) => {
  console.log('Target:', event.target); // Element clicked on
  console.log('CurrentTarget:', event.currentTarget); // Element listener is attached to
});
```

## Common Event Properties

| Property | Description |
|----------|-------------|
| `type` | Event type (click, keydown, etc.) |
| `target` | Element that triggered the event |
| `currentTarget` | Element with the event listener |
| `timeStamp` | Time event was created |
| `bubbles` | Whether event bubbles up |
| `cancelable` | Whether event can be cancelled |

## Common Use Cases

1. **Identifying clicked element** - Use `event.target`
2. **Keyboard shortcuts** - Use `event.key` and modifier keys
3. **Mouse tracking** - Use `clientX` and `clientY`
4. **Event delegation** - Check `event.target` matches selector

## Common Mistakes

```javascript
// Wrong: Confusing target and currentTarget
parent.addEventListener('click', (event) => {
  event.target.classList.add('active'); // May not be parent
  event.currentTarget.classList.add('active'); // Always parent
});

// Correct usage
parent.addEventListener('click', (event) => {
  const clickedElement = event.target;
  if (clickedElement.matches('.button')) {
    // Handle button click
  }
});
```

## Related Topics

- [[Handle-Clicks]]
- [[Handle-Keys]]
- [[Handle-Form]]
- [[Stop-Propagation]]

## Quick Revision

| Property | Use Case |
|----------|----------|
| `event.target` | Element that triggered event |
| `event.currentTarget` | Element with listener |
| `event.key` | Key pressed (keyboard events) |
| `event.clientX/Y` | Mouse position (mouse events) |
| `event.preventDefault()` | Stop default browser behavior |
