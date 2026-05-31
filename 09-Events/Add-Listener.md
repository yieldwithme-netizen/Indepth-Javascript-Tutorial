# Adding Event Listeners

## Definition

Event listeners allow you to execute code when specific events occur on elements (clicks, key presses, mouse movements, etc.). The `addEventListener()` method is the modern, recommended way to handle events.

## Syntax

```javascript
element.addEventListener(event, handler, options);

// Options (optional)
{
    once: false,      // Remove after first trigger
    capture: false,   // Use capture phase
    passive: false,   // Never call preventDefault()
    signal: AbortSignal // AbortController signal
}
```

## Code Examples

### Basic Click Listener

```javascript
// Add click event
document.getElementById("myButton").addEventListener("click", function() {
    console.log("Button clicked!");
});

// Using arrow function
button.addEventListener("click", () => {
    console.log("Clicked!");
});
```

### Multiple Event Listeners

```javascript
let element = document.getElementById("box");

// Add multiple listeners for same event
element.addEventListener("click", handler1);
element.addEventListener("click", handler2);

// Different events
element.addEventListener("mouseenter", handleMouseEnter);
element.addEventListener("mouseleave", handleMouseLeave);
```

### Event Object

```javascript
button.addEventListener("click", function(event) {
    console.log("Event type:", event.type);
    console.log("Target:", event.target);
    console.log("Mouse position:", event.clientX, event.clientY);
});
```

### Options: Once

```javascript
// Trigger only once
button.addEventListener("click", function() {
    console.log("This runs only once!");
}, { once: true });
```

### Options: Capture

```javascript
// Use capture phase (parent before child)
parent.addEventListener("click", handler, { capture: true });
```

### Options: Passive

```javascript
// Improve scroll performance
window.addEventListener("scroll", function() {
    // Cannot call event.preventDefault()
    console.log("Scrolling...");
}, { passive: true });
```

### AbortController for Removal

```javascript
let controller = new AbortController();

button.addEventListener("click", handler, { signal: controller.signal });

// Later: remove all listeners with this signal
controller.abort();
```

### Removing Event Listeners

```javascript
// Must reference same function
function handleClick() {
    console.log("Clicked!");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

## Common Use Cases

1. **Form handling**: Submit, validate, input events
2. **UI interactions**: Click, hover, drag events
3. **Keyboard shortcuts**: Keydown, keyup events
4. **Window events**: Resize, scroll, load events
5. **Touch events**: Mobile interactions

## Common Mistakes

1. **Using anonymous functions** - Cannot remove them later
2. **Forgetting to store function reference** - Need same reference for removeEventListener
3. **Not using passive for scroll** - Can cause performance issues
4. **Adding listeners in loops** - Consider event delegation instead

## Related Topics

- [[Stop-Bubbling]] - Stop event propagation
- [[Use-Capture]] - Use capture phase
- [[Implement-Delegation]] - Efficient event handling
- [[Change-Text]] - Update text in handlers
- [[Manage-Classes]] - Toggle classes on events

## Quick Revision

| Option | Purpose |
|--------|---------|
| once | Remove after first trigger |
| capture | Use capture phase |
| passive | Never call preventDefault() |
| signal | AbortController for removal |

**Best Practice**: Store handler references for removal; use `passive: true` for scroll/touch events; consider event delegation for dynamic content.
