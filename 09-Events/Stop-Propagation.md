# Stop Propagation

## Definition

`stopPropagation()` prevents an event from bubbling up or capturing down the DOM tree. It stops the event from reaching parent or child elements.

## Syntax

```javascript
element.addEventListener('event', (event) => {
  event.stopPropagation();
});
```

## Event Flow

```
Capture Phase:  Document → html → body → div → target
Bubble Phase:   target → div → body → html → Document
```

## Code Examples

### Basic Bubble Stopping

```javascript
const parent = document.getElementById('parent');
const child = document.getElementById('child');

parent.addEventListener('click', () => {
  console.log('Parent clicked');
});

child.addEventListener('click', (event) => {
  event.stopPropagation();
  console.log('Child clicked - parent will not receive this event');
});
```

### Stop Capturing Phase

```javascript
parent.addEventListener('click', (event) => {
  event.stopImmediatePropagation();
  console.log('Parent captured - child will not receive this event');
}, true); // true = capture phase
```

### Stop Immediate Propagation

```javascript
element.addEventListener('click', (event) => {
  event.stopImmediatePropagation();
  console.log('This handler runs');
});

element.addEventListener('click', () => {
  console.log('This handler will NOT run');
});
```

## Common Use Cases

1. **Nested click handlers** - Prevent parent from reacting to child clicks
2. **Modal overlays** - Click outside modal doesn't close it
3. **Dropdown menus** - Click inside menu doesn't close parent
4. **Event delegation** - Control which elements respond to events

## Common Mistakes

```javascript
// Wrong: Confusing stopPropagation with preventDefault
event.stopPropagation(); // Stops event from propagating
event.preventDefault();  // Stops browser default behavior

// Wrong: Using stopPropagation in event delegation
document.addEventListener('click', (event) => {
  event.stopPropagation(); // Breaks event delegation
});
```

## Related Topics

- [[Prevent-Default]]
- [[Use-Event-Props]]
- [[Handle-Clicks]]
- [[Handle-Form]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| `stopPropagation()` | Stops event from bubbling/capturing |
| `stopImmediatePropagation()` | Stops all handlers on same element |
| Use when | Nested elements need separate click handling |
