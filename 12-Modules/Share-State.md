# Share State Between Modules

## Definition

Sharing state between modules refers to the practice of allowing multiple JavaScript modules to access and modify the same data or resources. This is essential for building applications where different parts of the codebase need to communicate with each other, such as a shopping cart that needs to be updated from multiple pages or components.

There are several approaches to sharing state, including the Singleton pattern, module-level variables, event emitters, and dedicated state management libraries. Each approach has trade-offs in terms of complexity, performance, and maintainability.

## Common Approaches

### 1. Module-Level Singleton

The simplest way to share state is by exporting a single object from a module. All imports reference the same instance.

```javascript
// state.js
const state = {
  user: null,
  theme: 'light',
  notifications: []
};

export function setUser(user) {
  state.user = user;
}

export function getUser() {
  return state.user;
}

export function setTheme(theme) {
  state.theme = theme;
}

export function getTheme() {
  return state.theme;
}

export function addNotification(message) {
  state.notifications.push(message);
}

export function getNotifications() {
  return [...state.notifications];
}
```

```javascript
// app.js
import { setUser, getUser, setTheme } from './state.js';

setUser({ name: 'Alice', email: 'alice@example.com' });
console.log(getUser()); // { name: 'Alice', email: 'alice@example.com' }

setTheme('dark');
```

```javascript
// header.js
import { getUser, getTheme } from './state.js';

function renderHeader() {
  const user = getUser();
  const theme = getTheme();
  console.log(`Rendering header for ${user.name} with ${theme} theme`);
}

renderHeader();
```

### 2. Observer Pattern with Event Emitter

For more decoupled communication, use an event emitter to notify modules when state changes.

```javascript
// eventBus.js
class EventBus {
  constructor() {
    this.listeners = {};
  }

  on(event, callback) {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event].push(callback);
  }

  off(event, callback) {
    if (!this.listeners[event]) return;
    this.listeners[event] = this.listeners[event].filter(cb => cb !== callback);
  }

  emit(event, data) {
    if (!this.listeners[event]) return;
    this.listeners[event].forEach(callback => callback(data));
  }
}

export const eventBus = new EventBus();
```

```javascript
// store.js
import { eventBus } from './eventBus.js';

const store = {
  _state: { count: 0 },
  
  getState() {
    return { ...this._state };
  },
  
  setState(newState) {
    this._state = { ...this._state, ...newState };
    eventBus.emit('stateChanged', this._state);
  }
};

export default store;
```

```javascript
// counter.js
import store from './store.js';
import { eventBus } from './eventBus.js';

export function increment() {
  const current = store.getState();
  store.setState({ count: current.count + 1 });
}

eventBus.on('stateChanged', (state) => {
  console.log('Count updated:', state.count);
});
```

### 3. Using localStorage for Persistence

When state needs to persist across page reloads, localStorage provides a simple solution.

```javascript
// persistentState.js
export function getPersistentState(key, defaultValue) {
  const stored = localStorage.getItem(key);
  return stored ? JSON.parse(stored) : defaultValue;
}

export function setPersistentState(key, value) {
  localStorage.setItem(key, JSON.stringify(value));
}

export function clearPersistentState(key) {
  localStorage.removeItem(key);
}
```

```javascript
// settings.js
import { getPersistentState, setPersistentState } from './persistentState.js';

export function getSettings() {
  return getPersistentState('appSettings', {
    language: 'en',
    fontSize: 16,
    notifications: true
  });
}

export function updateSettings(newSettings) {
  const current = getSettings();
  setPersistentState('appSettings', { ...current, ...newSettings });
}
```

## Common Use Cases

- **Shopping cart**: Multiple product pages need to update the same cart
- **User authentication**: Login state shared across protected routes
- **Theme preferences**: UI theme consistent across components
- **Form data**: Multi-step forms that span multiple components
- **Real-time data**: Chat applications where messages appear in multiple places

## Common Mistakes

1. **Circular dependencies**: Module A imports Module B which imports Module A
   - Solution: Extract shared state into a separate module

2. **Mutation without notification**: Changing state without alerting dependent modules
   - Solution: Use the observer pattern or event emitters

3. **Forgetting to handle state cleanup**: Leaving stale state in memory
   - Solution: Implement cleanup functions and unsubscribe from events

4. **Over-coupling**: Modules directly importing state from each other
   - Solution: Use dependency injection or event-based communication

5. **Not handling concurrent updates**: Race conditions in async state updates
   - Solution: Use queues or immutable state patterns

## Quick Revision Summary

- **Singleton pattern**: Export a single state object from a module
- **Event emitter**: Decouple modules with publish-subscribe communication
- **localStorage**: Persist state across browser sessions
- **Avoid circular dependencies**: Keep shared state in dedicated modules
- **Use immutable updates**: Prevent unexpected side effects
- **Subscribe to changes**: Listen for state updates rather than polling

## Related Topics

- [[Modules]]
- [[Singleton-Pattern]]
- [[Event-Handling]]
- [[LocalStorage]]
- [[Design-Patterns]]
- [[Closures]]
- [[Scope]]
