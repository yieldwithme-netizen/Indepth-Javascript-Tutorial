# Context API

## Definition

Context API **shares data** across components without prop drilling.

## Basic Usage

```javascript
import { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

// Provider
function App() {
    return (
        <ThemeContext.Provider value="dark">
            <Child />
        </ThemeContext.Provider>
    );
}

// Consumer
function Child() {
    const theme = useContext(ThemeContext);
    return <div className={theme}>Hello</div>;
}
```

## Quick Revision

- Context = global data
- `createContext()` to create
- `Provider` to provide value
- `useContext()` to consume
- Avoids prop drilling

---

## Related Topics

- [[What-is-Context-API]] - [[What-is-Context-API|Context API]]
- [[Context-API]] - [[Context-API|Context API]]
- [[React-Hooks]] - [[React-Hooks|React hooks]]
- [[What-is-State]] - [[What-is-State|State]]
