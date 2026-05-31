# Event Bus

## Definition

An event bus is a **messaging system** that allows decoupled components to communicate through events.

## Basic Implementation

```javascript
class EventBus {
    constructor() {
        this.events = {};
    }
    
    on(event, callback) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(callback);
    }
    
    off(event, callback) {
        if (!this.events[event]) return;
        this.events[event] = this.events[event].filter(cb => cb !== callback);
    }
    
    emit(event, data) {
        if (!this.events[event]) return;
        this.events[event].forEach(callback => callback(data));
    }
}

// Usage
const bus = new EventBus();

bus.on('userLoggedIn', (user) => {
    console.log(`Welcome, ${user.name}!`);
});

bus.emit('userLoggedIn', { name: "John" }); // "Welcome, John!"
```

## React Example

```javascript
// EventContext.js
const EventContext = React.createContext();

export function EventProvider({ children }) {
    const [events, setEvents] = useState({});
    
    const emit = (event, data) => {
        if (events[event]) {
            events[event].forEach(callback => callback(data));
        }
    };
    
    const on = (event, callback) => {
        setEvents(prev => ({
            ...prev,
            [event]: [...(prev[event] || []), callback]
        }));
    };
    
    return (
        <EventContext.Provider value={{ on, emit }}>
            {children}
        </EventContext.Provider>
    );
}
```

## Quick Revision

- Event bus = decoupled communication
- `on()` subscribes to events
- `off()` unsubscribes
- `emit()` triggers events
- Use for: component communication, state management

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Add-Listener]] - [[Add-Listener|Adding listeners]]
- [[What-is-State]] - [[What-is-State|State management]]
- [[What-is-Context-API]] - [[What-is-Context-API|Context API]]
