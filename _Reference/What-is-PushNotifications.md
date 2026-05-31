# What-is-PushNotifications

## Definition

Push notifications send **messages to users' devices**.

## Example

```javascript
Notification.requestPermission().then(permission => {
    if (permission === 'granted') {
        new Notification('Hello!', { body: 'Push notification' });
    }
});
```

## Quick Revision

- Push = messages to devices
- Request permission first
- Use Notification API
- Service workers for background

---

## Related Topics

- [[What-is-PushNotifications]] - [[What-is-PushNotifications|Push notifications]]
- [[What-is-PushNotifications]] - [[What-is-PushNotifications|Push notifications]]
- [[PushNotifications]] - [[PushNotifications|Push notifications]]
- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
