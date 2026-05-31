# What is a Mouse Event?

## Definition

Mouse events fire when the user **interacts with the mouse** (or pointer).

## Mouse Events

| Event | When It Fires |
|-------|---------------|
| click | Button clicked |
| dblclick | Double clicked |
| mousedown | Button pressed |
| mouseup | Button released |
| mouseenter | Pointer enters element |
| mouseleave | Pointer leaves element |
| mousemove | Pointer moves |

## Examples

```javascript
// Click
button.addEventListener("click", () => {
    console.log("Clicked!");
});

// Double click
element.addEventListener("dblclick", () => {
    console.log("Double clicked!");
});

// Mouse enter/leave (hover)
element.addEventListener("mouseenter", () => {
    element.classList.add("hover");
});

element.addEventListener("mouseleave", () => {
    element.classList.remove("hover");
});

// Mouse move
document.addEventListener("mousemove", (e) => {
    console.log(`X: ${e.clientX}, Y: ${e.clientY}`);
});
```

## Mouse Event Properties

```javascript
element.addEventListener("click", (e) => {
    console.log(e.clientX, e.clientY); // viewport position
    console.log(e.pageX, e.pageY);    // page position
    console.log(e.screenX, e.screenY); // screen position
    console.log(e.button); // 0=left, 1=middle, 2=right
});
```

## Quick Revision

- Mouse events: click, dblclick, mousedown/up
- Hover: mouseenter/mouseleave
- Track position: clientX/clientY
- Button: 0=left, 1=middle, 2=right
- Use for: clicks, hover effects, drag

---

## Related Topics

- [[Handle-Clicks]] - Click handling
- [[What-is-Event]] - Events
- [[What-is-EventObject]] - Event object
- [[What-is-KeyboardEvent]] - Keyboard events
