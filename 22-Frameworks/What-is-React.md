# What is React?

## Definition

React is a **JavaScript library for building user interfaces**, maintained by Meta.

## Core Concepts

### JSX

```jsx
const element = <h1>Hello, World!</h1>;
const element = <h1>Hello, {name}!</h1>;
```

### Components

```jsx
// Function component
function Greeting({ name }) {
    return <h1>Hello, {name}!</h1>;
}

// Class component
class Greeting extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}!</h1>;
    }
}
```

### State

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

### Props

```jsx
// Parent
<Greeting name="John" age={30} />

// Child
function Greeting({ name, age }) {
    return <h1>Hello, {name}! Age: {age}</h1>;
}
```

## Quick Revision

- React = UI library by Meta
- Uses JSX (HTML-like syntax)
- Components: function or class
- State: component data
- Props: data passed to components

---

## Related Topics

- [[What-is-React]] - React overview
- [[What-is-Vue]] - Vue.js
- [[What-is-Angular]] - Angular
- [[What-is-Components]] - Component architecture
- [[What-is-State]] - State management
- [[What-is-VirtualDOM]] - Virtual DOM
