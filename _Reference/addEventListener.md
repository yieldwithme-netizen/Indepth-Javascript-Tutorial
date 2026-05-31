# addEventListener

## Definition
The `addEventListener()` method attaches an event handler to a specified element without overwriting existing event handlers. It allows you to listen for events and execute code when they occur.

## Syntax
```javascript
element.addEventListener(event, function, useCapture);
```

- **event**: The event name (e.g., "click", "keydown")
- **function**: The function to run when the event occurs
- **useCapture**: Optional. `true` captures events during the capture phase; `false` (default) during the bubbling phase

## Code Examples

### Basic Click Event
```javascript
const button = document.getElementById('myButton');

button.addEventListener('click', function() {
  console.log('Button clicked!');
});
```

### Multiple Event Listeners
```javascript
const element = document.getElementById('myElement');

element.addEventListener('click', () => {
  console.log('First handler');
});

element.addEventListener('click', () => {
  console.log('Second handler');
});
```

### Passing Arguments to Handler
```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}

const button = document.getElementById('myButton');
button.addEventListener('click', () => greet('Alice'));
```

### Removing Event Listener
```javascript
function handleClick() {
  console.log('Clicked!');
}

const button = document.getElementById('myButton');
button.addEventListener('click', handleClick);

// Remove the listener
button.removeEventListener('click', handleClick);
```

### Event Object
```javascript
button.addEventListener('click', function(event) {
  console.log('Target:', event.target);
  console.log('Type:', event.type);
  console.log('Client X:', event.clientX);
});
```

## Common Use Cases
- Form validation
- Keyboard navigation
- Scroll effects
- Drag-and-drop functionality
- UI interactions

## Common Mistakes
- **Anonymous functions can't be removed**: Use named functions if you need `removeEventListener()`
- **Memory leaks**: Always remove listeners when elements are destroyed
- **Forgetting `this` binding**: Arrow functions don't have their own `this`

## Related Topics
- [[DOM-Events]]
- [[Event-Delegation]]
- [[Mouse-Events]]
- [[Keyboard-Events]]
- [[Touch-Events]]

## Quick Revision
- `addEventListener()` attaches handlers without overwriting
- Use `removeEventListener()` to clean up
- Third parameter controls event capture/bubble phase
- Supports multiple handlers per event per element
