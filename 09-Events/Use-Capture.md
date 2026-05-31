# Using the Capture Phase

## Definition

Event propagation has three phases: capture, target, and bubble. The capture phase occurs before the target phase, allowing parent elements to intercept events before they reach the target element.

## Event Propagation Phases

```html
<div id="grandparent">
    <div id="parent">
        <button id="child">Click</button>
    </div>
</div>

<!-- Phase 1: Capture (grandparent → parent → child) -->
<!-- Phase 2: Target (child) -->
<!-- Phase 3: Bubble (child → parent → grandparent) -->
```

## Syntax

```javascript
// Enable capture phase
element.addEventListener(event, handler, { capture: true });

// Or using third argument as boolean
element.addEventListener(event, handler, true); // capture
element.addEventListener(event, handler, false); // bubble (default)
```

## Code Examples

### Basic Capture Phase

```javascript
// Parent captures event FIRST (before child)
document.getElementById("parent").addEventListener("click", function() {
    console.log("Parent (capture)");
}, { capture: true });

// Child handles event
document.getElementById("child").addEventListener("click", function() {
    console.log("Child");
});

// Click on child logs:
// 1. "Parent (capture)" - parent captures first
// 2. "Child" - child handles event
```

### Capture vs Bubble Comparison

```javascript
// Capture phase (top → down)
parent.addEventListener("click", handler, true);

// Bubble phase (bottom → up) - default
parent.addEventListener("click", handler, false);

// Both phases
parent.addEventListener("click", handlerCapture, true);
parent.addEventListener("click", handlerBubble, false);
```

### Practical Example: Event Delegation with Capture

```javascript
// Intercept all clicks early
document.addEventListener("click", function(event) {
    // Track all clicks
    console.log("Click detected at:", event.target);
    
    // Can intercept before target handlers
    if (event.target.matches(".disabled")) {
        event.stopPropagation();
        return;
    }
}, { capture: true });
```

### Form Validation

```javascript
// Validate before form submits
form.addEventListener("submit", function(event) {
    if (!isValid()) {
        event.preventDefault(); // Stop submission
        event.stopPropagation(); // Stop bubbling
    }
}, { capture: true });
```

### Keyboard Shortcuts

```javascript
// Global keyboard shortcuts (capture phase)
document.addEventListener("keydown", function(event) {
    // Ctrl+S to save
    if (event.ctrlKey && event.key === "s") {
        event.preventDefault();
        saveDocument();
    }
}, { capture: true });
```

### Nested Scroll Handling

```javascript
// Parent captures scroll events
parent.addEventListener("scroll", function() {
    console.log("Parent scroll captured");
}, { capture: true });

// Child scroll doesn't bubble normally
child.addEventListener("scroll", function() {
    console.log("Child scroll");
});
```

## Common Use Cases

1. **Global event handling**: Intercept events at document level
2. **Event delegation**: Handle events before they reach target
3. **Keyboard shortcuts**: Global hotkeys
4. **Form validation**: Validate before submission
5. **Analytics tracking**: Track all clicks/interactions
6. **Security**: Block events on disabled elements

## Common Mistakes

1. **Confusing capture and bubble** - Capture goes down, bubble goes up
2. **Overusing capture** - Can interfere with other handlers
3. **Forgetting to stopPropagation** - May need to prevent bubble phase
4. **Not considering performance** - Capture on document affects all events

## Related Topics

- [[Stop-Bubbling]] - Stop event propagation
- [[Add-Listener]] - Add event listeners
- [[Implement-Delegation]] - Use capture for delegation
- [[Change-Styles]] - Update styles on events

## Quick Revision

| Phase | Direction | Use Case |
|-------|-----------|----------|
| Capture | Top → Down | Intercept early, global handling |
| Target | At element | Direct element handling |
| Bubble | Bottom → Up | Default, parent handling |

**Key Point**: Use `{ capture: true }` to handle events in capture phase (parent before child). Default is bubble phase (child before parent).

**Best Practice**: Use capture for global handlers; use bubble for most cases; use both when needed.
