# What-is-Notification

## Definition

Notification API shows **browser notifications**.

## Example

```javascript
// Request permission
Notification.requestPermission().then(permission => {
    if (permission === 'granted') {
        new Notification('Hello!', { body: 'This is a notification' });
    }
});
```

## Quick Revision

- Request permission first
- new Notification() to show
- Options: body, icon, sound
- Use for: alerts, updates

---

## Related Topics

- [[What-is-Notification]] - [[What-is-Notification|Notification]]
- [[What-is-Notification]] - [[What-is-Notification|Notification]]
- [[PushNotifications]] - [[PushNotifications|Push notifications]]
- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
