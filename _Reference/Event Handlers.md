# Event Handlers

## Definition

Event handlers are **functions that run** when an event occurs.

## addEventListener

```javascript
button.addEventListener("click", function() {
    console.log("Clicked!");
});
```

## onclick Property

```javascript
button.onclick = function() {
    console.log("Clicked!");
};
```

## Removing Handlers

```javascript
function handleClick() {
    console.log("Clicked!");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

## Quick Revision

- `addEventListener()` to add handlers
- `removeEventListener()` to remove
- Handler receives event object
- Can add multiple handlers

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Event-Handlers]] - [[Event-Handlers|Event handlers]]
- [[Add-Listener]] - [[Add-Listener|Adding listeners]]
- [[Handle-Clicks]] - [[Handle-Clicks|Click handling]]
