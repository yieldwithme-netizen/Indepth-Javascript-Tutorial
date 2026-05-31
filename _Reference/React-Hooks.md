# React Hooks

## Definition

Hooks let you **use state and other React features** in function components.

## useState

```jsx
import { useState } from 'react';

function Counter() {
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

## useEffect

```jsx
import { useEffect } from 'react';

function Timer() {
    const [seconds, setSeconds] = useState(0);
    
    useEffect(() => {
        const interval = setInterval(() => {
            setSeconds(s => s + 1);
        }, 1000);
        
        return () => clearInterval(interval);
    }, []);
    
    return <p>Seconds: {seconds}</p>;
}
```

## useContext

```jsx
import { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemedButton() {
    const theme = useContext(ThemeContext);
    return <button className={theme}>Click me</button>;
}
```

## Quick Revision

- `useState` - manage state
- `useEffect` - side effects
- `useContext` - context access
- `useRef` - DOM references
- `useMemo`/`useCallback` - optimization

---

## Related Topics

- [[What-is-React]] - [[What-is-React|React]]
- [[React-Components]] - [[React-Components|Components]]
- [[What-is-State]] - [[What-is-State|State management]]
- [[What-is-State]] - [[What-is-State|State]]
