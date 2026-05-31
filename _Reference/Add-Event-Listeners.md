# Add Event Listeners

Event listeners are functions that wait for specific events to occur on HTML elements and execute code in response. They are the foundation of interactive web applications.

## Definition

An event listener monitors a target element for a specified event type and runs a callback function when that event occurs. The `addEventListener()` method is the primary way to attach event handlers.

**Syntax:**
```javascript
element.addEventListener(eventType, callbackFunction, options);
```

**Parameters:**
- `eventType`: A string representing the event name (e.g., 'click', 'keydown')
- `callbackFunction`: The function to execute when the event fires
- `options` (optional): An object with properties like `capture`, `once`, and `passive`

## Basic Examples

```javascript
// Click event listener
const button = document.querySelector('#myButton');

button.addEventListener('click', function() {
    console.log('Button was clicked!');
});

// Using arrow function
button.addEventListener('click', () => {
    console.log('Button clicked with arrow function!');
});

// Event object is passed to callback
button.addEventListener('click', (event) => {
    console.log('Event type:', event.type);
    console.log('Target element:', event.target);
    console.log('Mouse position:', event.clientX, event.clientY);
});
```

## Common Event Types

```javascript
// Mouse events
element.addEventListener('click', handleClick);
element.addEventListener('dblclick', handleDoubleClick);
element.addEventListener('mouseenter', handleMouseEnter);
element.addEventListener('mouseleave', handleMouseLeave);

// Keyboard events
document.addEventListener('keydown', handleKeyDown);
document.addEventListener('keyup', handleKeyUp);

// Form events
input.addEventListener('input', handleInput);
form.addEventListener('submit', handleSubmit);

// Window events
window.addEventListener('resize', handleResize);
window.addEventListener('scroll', handleScroll);
window.addEventListener('load', handleLoad);
```

## Event Listener Options

```javascript
// Capture phase (parent elements fire before children)
element.addEventListener('click', handler, { capture: true });

// Once - automatically remove after first invocation
button.addEventListener('click', handler, { once: true });

// Passive - improves scroll performance
window.addEventListener('scroll', handler, { passive: true });

// Signal - AbortController to remove listener
const controller = new AbortController();
element.addEventListener('click', handler, { signal: controller.signal });
controller.abort(); // Removes the listener
```

## Removing Event Listeners

```javascript
// You need a reference to the same function
function handleClick() {
    console.log('Clicked!');
}

button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick);

// Arrow functions cannot be removed (no reference)
button.addEventListener('click', () => console.log('hi'));
// Cannot remove - no reference to anonymous function
```

## Event Delegation

```javascript
// Instead of adding listeners to each child
const list = document.querySelector('#myList');

// Add one listener to parent
list.addEventListener('click', (event) => {
    if (event.target.matches('li')) {
        console.log('List item clicked:', event.target.textContent);
    }
});
```

## Common Use Cases

- Form validation before submission
- Button click handlers
- Keyboard shortcuts
- Drag and drop functionality
- Scroll-based animations
- Responsive menu toggling

## Common Mistakes

1. **Anonymous functions cannot be removed** - Always use named functions if you need to remove listeners
2. **Forgetting to remove listeners** - Causes memory leaks in long-running applications
3. **Using wrong `this` context** - Arrow functions don't bind `this` to the element
4. **Not preventing default behavior** - Missing `event.preventDefault()` for forms
5. **Adding listeners before DOM is ready** - Elements won't exist yet

## Related Topics

- [[DOM Manipulation]]
- [[Event Object]]
- [[Delegation Pattern]]
- [[Memory Management]]
- [[Callbacks]]

## Quick Revision

| Concept | Syntax |
|---------|--------|
| Add listener | `element.addEventListener('click', fn)` |
| Remove listener | `element.removeEventListener('click', fn)` |
| Once only | `{ once: true }` |
| Capture phase | `{ capture: true }` |
| Prevent default | `event.preventDefault()` |
| Stop propagation | `event.stopPropagation()` |
