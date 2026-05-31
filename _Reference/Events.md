# Events

## Definition

Events are **actions that happen in the browser** (clicks, keypresses, etc.).

## Event Handling

```javascript
// addEventListener
button.addEventListener("click", () => {
    console.log("Clicked!");
});

// onclick
button.onclick = () => {
    console.log("Clicked!");
};
```

## Event Types

| Type | Examples |
|------|----------|
| Mouse | click, dblclick, mouseenter, mouseleave |
| Keyboard | keydown, keyup |
| Form | submit, change, input |
| Window | load, resize, scroll |
| Touch | touchstart, touchmove, touchend |

## Event Object

```javascript
button.addEventListener("click", (event) => {
    console.log(event.type);   // "click"
    console.log(event.target); // clicked element
    console.log(event.clientX); // x position
});
```

## Quick Revision

- Event = action in browser
- Use `addEventListener()` to handle
- Event object has event details
- Common: click, submit, keydown

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Handle-Clicks]] - [[Handle-Clicks|Click handling]]
- [[Handle-Keys]] - [[Handle-Keys|Key handling]]
- [[Handle-Form]] - [[Handle-Form|Form handling]]
