# State Updating

## Definition

State updating involves **changing component state** in React.

## Examples

```javascript
// useState
const [count, setCount] = useState(0);
setCount(count + 1);
setCount(prev => prev + 1);

// useReducer
dispatch({ type: 'INCREMENT' });
```

## Quick Revision

- Never mutate state directly
- Use setter function from useState
- Use dispatch with useReducer
- State updates trigger re-render

---

## Related Topics

- [[What-is-State]] - [[What-is-State|State]]
- [[React-State-Management]] - [[React-State-Management|React state]]
- [[State-Updating]] - [[State-Updating|State updating]]
- [[React-Hooks]] - [[React-Hooks|React hooks]]
