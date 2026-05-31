# React

## Definition

React is a **JavaScript library** for building user interfaces.

## Basic Example

```jsx
function App() {
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </div>
    );
}
```

## Quick Revision

- React = UI library by Meta
- Component-based
- Uses JSX
- Virtual DOM
- Use for: SPAs, UIs

---

## Related Topics

- [[What-is-React]] - [[What-is-React|React]]
- [[React]] - [[React|React]]
- [[React-Components]] - [[React-Components|Components]]
- [[React-Hooks]] - [[React-Hooks|Hooks]]
- [[What-is-State]] - [[What-is-State|State]]
