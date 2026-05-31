# What is an Event?

## Definition

An event is an **action** that happens in the browser (click, keypress, form submit, etc.).

## Adding Event Listeners

```javascript
// addEventListener (recommended)
const button = document.querySelector("button");
button.addEventListener("click", function() {
    console.log("Clicked!");
});

// onclick property
button.onclick = function() {
    console.log("Clicked!");
};
```

## Event Types

| Category | Events |
|----------|--------|
| Mouse | click, dblclick, mouseenter, mouseleave |
| Keyboard | keydown, keyup, keypress |
| Form | submit, change, input, focus, blur |
| Window | load, resize, scroll, unload |
| Touch | touchstart, touchmove, touchend |

## Event Handler

```javascript
function handleClick(event) {
    console.log("Button clicked!");
    console.log(event.target); // clicked element
}

button.addEventListener("click", handleClick);

// Remove listener
button.removeEventListener("click", handleClick);
```

## Quick Revision

- Event = action in browser
- Use `addEventListener()` to handle events
- Handler receives event object
- Can add/remove listeners
- Common: click, submit, keydown

---

## Related Topics

- [[Handle-Clicks]] - Click handling
- [[Handle-Keys]] - Keyboard handling
- [[Handle-Form]] - Form handling
- [[What-is-Bubbling]] - Event bubbling
- [[What-is-Delegation]] - Event delegation
