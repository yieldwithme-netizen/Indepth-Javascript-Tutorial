# What-is-Observer

## Definition

Observer pattern **notifies subscribers** when state changes.

## Example

```javascript
class EventEmitter {
    constructor() {
        this.events = {};
    }
    
    on(event, callback) {
        this.events[event] = this.events[event] || [];
        this.events[event].push(callback);
    }
    
    emit(event, data) {
        if (this.events[event]) {
            this.events[event].forEach(cb => cb(data));
        }
    }
}
```

## Quick Revision

- Observer = publish-subscribe
- Subject notifies observers
- Use for: event systems, state management

---

## Related Topics

- [[Observer-Pattern]] - [[Observer-Pattern|Observer pattern]]
- [[What-is-Observer]] - [[What-is-Observer|Observer]]
- [[Event-Bus]] - [[Event-Bus|Event bus]]
- [[Design-Patterns]] - [[Design-Patterns|Design patterns]]
