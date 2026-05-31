# React useEffect Cleanup

## Definition

useEffect cleanup **prevents memory leaks** and cleans up resources.

## Basic Usage

```jsx
import { useEffect } from 'react';

function Timer() {
    useEffect(() => {
        const interval = setInterval(() => {
            console.log('Tick');
        }, 1000);
        
        // Cleanup function
        return () => {
            clearInterval(interval);
        };
    }, []);
    
    return <p>Timer running</p>;
}
```

## Quick Revision

- Cleanup function returned from useEffect
- Runs on unmount or before re-run
- Prevents memory leaks
- Use for: timers, subscriptions, event listeners

---

## Related Topics

- [[React-Hooks]] - [[React-Hooks|React hooks]]
- [[React-UseEffect-Cleanup]] - [[React-UseEffect-Cleanup|useEffect cleanup]]
- [[What-is-React]] - [[What-is-React|React]]
- [[Prevent-MemoryLeaks]] - [[Prevent-MemoryLeaks|Memory leaks]]
