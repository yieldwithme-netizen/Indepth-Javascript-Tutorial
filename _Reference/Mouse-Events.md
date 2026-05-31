# Mouse Events

## Definition

Mouse events fire when the user **interacts with the mouse**.

## Events

| Event | When |
|-------|------|
| click | Button clicked |
| dblclick | Double clicked |
| mousedown | Button pressed |
| mouseup | Button released |
| mouseenter | Pointer enters |
| mouseleave | Pointer leaves |
| mousemove | Pointer moves |

## Examples

```javascript
button.addEventListener("click", () => {
    console.log("Clicked!");
});

element.addEventListener("mouseenter", () => {
    element.classList.add("hover");
});

element.addEventListener("mouseleave", () => {
    element.classList.remove("hover");
});
```

## Quick Revision

- click: button clicked
- mouseenter/mouseleave: hover
- mousedown/mouseup: press/release
- mousemove: pointer moves

---

## Related Topics

- [[What-is-MouseEvent]] - [[What-is-MouseEvent|Mouse events]]
- [[Mouse-Events]] - [[Mouse-Events|Mouse events]]
- [[Handle-Clicks]] - [[Handle-Clicks|Click handling]]
- [[What-is-Event]] - [[What-is-Event|Events]]
