# Stopping Event Bubbling

## Definition

Event bubbling is when an event on a child element "bubbles up" to its parent elements. You can stop this propagation using `stopPropagation()` or `stopImmediatePropagation()`.

## How Bubbling Works

```html
<div id="parent">
    <div id="child">
        <button id="button">Click</button>
    </div>
</div>

<!-- Click on button triggers: -->
<!-- 1. button click handler -->
<!-- 2. child click handler -->
<!-- 3. parent click handler -->
<!-- 4. document click handler -->
```

## Syntax

```javascript
// Stop propagation to parent elements
event.stopPropagation();

// Stop immediate handlers + propagation
event.stopImmediatePropagation();
```

## Code Examples

### Basic stopPropagation

```javascript
// Parent click handler
document.getElementById("parent").addEventListener("click", function() {
    console.log("Parent clicked");
});

// Child click handler - stops bubbling
document.getElementById("child").addEventListener("click", function(event) {
    console.log("Child clicked");
    event.stopPropagation(); // Parent won't be notified
});

// Click on child logs only "Child clicked"
```

### stopImmediatePropagation

```javascript
// Multiple handlers on same element
button.addEventListener("click", function(event) {
    console.log("Handler 1");
    event.stopImmediatePropagation(); // Stops handler 2
});

button.addEventListener("click", function() {
    console.log("Handler 2"); // Never runs
});

// Click logs only "Handler 1"
```

### Practical Example: Dropdown Menu

```javascript
// Close dropdown when clicking outside
document.addEventListener("click", function() {
    closeAllDropdowns();
});

// Prevent dropdown from closing when clicking inside
dropdown.addEventListener("click", function(event) {
    event.stopPropagation();
    // Dropdown stays open
});
```

### Nested Elements

```javascript
// Stop bubbling at specific level
document.getElementById("outer").addEventListener("click", function() {
    console.log("Outer");
});

document.getElementById("inner").addEventListener("click", function(event) {
    console.log("Inner");
    event.stopPropagation(); // Middle won't log
});

document.getElementById("middle").addEventListener("click", function() {
    console.log("Middle");
});

// Click on inner: logs "Inner" only
```

### Modal Example

```javascript
// Prevent modal content from closing modal
modalContent.addEventListener("click", function(event) {
    event.stopPropagation();
});

// Click outside modal closes it
modalOverlay.addEventListener("click", function() {
    closeModal();
});
```

## Common Use Cases

1. **Dropdown menus**: Prevent closing when interacting with menu
2. **Modal dialogs**: Click inside doesn't close modal
3. **Nested components**: Prevent parent handlers from firing
4. **Form validation**: Stop form submit on specific field validation
5. **Context menus**: Right-click menu handling

## Common Mistakes

1. **Overusing stopPropagation** - Can break other functionality
2. **Forgetting capture phase** - stopPropagation works in both phases
3. **Not considering parent handlers** - May need parent events for other features
4. **Using on both phases** - May need different handling for capture/bubble

## Related Topics

- [[Use-Capture]] - Capture phase before bubbling
- [[Add-Listener]] - Add event handlers
- [[Implement-Delegation]] - Use bubbling for delegation
- [[Change-Styles]] - Update styles on events

## Quick Revision

| Method | Effect |
|--------|--------|
| stopPropagation() | Stops bubbling to parent elements |
| stopImmediatePropagation() | Stops other handlers + bubbling |

**Key Point**: Bubbling goes from target → parent → document. Use `stopPropagation()` to prevent parent handlers from running.

**Best Practice**: Use sparingly; prefer event delegation when possible to avoid propagation issues.
