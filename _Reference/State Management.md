# State Management

## Definition

State management involves **managing application data** and its changes.

## React State

```javascript
// useState
const [count, setCount] = useState(0);

// useReducer
const [state, dispatch] = useReducer(reducer, initialState);
```

## Redux

```javascript
// Store
const store = createStore(rootReducer);

// Dispatch
store.dispatch({ type: 'INCREMENT' });

// Select
const count = store.getState().count;
```

## Quick Revision

- State = application data
- React: useState, useReducer
- Redux: global state
- Use for: shared data

---

## Related Topics

- [[What-is-State]] - [[What-is-State|State management]]
- [[State Management]] - [[State Management|State management]]
- [[React-State-Management]] - [[React-State-Management|React state]]
- [[React-Hooks]] - [[React-Hooks|React hooks]]
