# Browser APIs

## Definition

Browser APIs provide **interfaces** for interacting with the browser and device features.

## Common APIs

| API | Purpose |
|-----|---------|
| DOM | Manipulate HTML/CSS |
| Fetch | HTTP requests |
| Geolocation | User location |
| LocalStorage | Client storage |
| Canvas | 2D graphics |
| Web Workers | Background threads |
| WebSocket | Real-time communication |
| Notification | Browser notifications |

## Examples

```javascript
// Geolocation
navigator.geolocation.getCurrentPosition((position) => {
    console.log(position.coords.latitude);
});

// LocalStorage
localStorage.setItem("name", "John");
const name = localStorage.getItem("name");

// Notification
Notification.requestPermission().then((permission) => {
    if (permission === "granted") {
        new Notification("Hello!");
    }
});
```

## Quick Revision

- Browser APIs = browser interfaces
- DOM, Fetch, Geolocation, Storage
- Canvas for graphics
- Web Workers for background tasks

---

## Related Topics

- [[What-is-Web-APIs]] - [[What-is-Web-APIs|Web APIs]]
- [[Browser APIs]] - [[Browser APIs|Browser APIs]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
