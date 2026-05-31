# Push Notifications

## Definition

Push notifications **send messages** to users' devices.

## Example

```javascript
// Request permission
const permission = await Notification.requestPermission();

// Show notification
if (permission === 'granted') {
    new Notification('Hello!', {
        body: 'This is a push notification',
        icon: '/icon.png'
    });
}
```

## Quick Revision

- Push notifications = messages to devices
- Request permission first
- Use Notification API
- Service workers for background

---

## Related Topics

- [[What-is-PushNotifications]] - [[What-is-PushNotifications|Push notifications]]
- [[PushNotifications]] - [[PushNotifications|Push notifications]]
- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
- [[What-is-Notification]] - [[What-is-Notification|Notification API]]
