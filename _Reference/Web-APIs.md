# Web APIs

## Definition

Web APIs are **browser-provided interfaces** for interacting with the browser and device features.

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
    console.log(position.coords.longitude);
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

// Canvas
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
ctx.fillRect(10, 10, 100, 100);
```

## Quick Revision

- Web APIs = browser interfaces
- DOM, Fetch, Geolocation, Storage
- Canvas for graphics
- Web Workers for background tasks
- WebSocket for real-time

---

## Related Topics

- [[What-is-Web-APIs]] - [[What-is-Web-APIs|Web APIs]]
- [[Web-APIs]] - [[Web-APIs|Web APIs]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
