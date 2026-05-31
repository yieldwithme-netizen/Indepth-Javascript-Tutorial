# React Components

## Definition

Components are **reusable pieces of UI** that return JSX describing what should appear on screen.

## Function Components

```jsx
function Greeting({ name }) {
    return <h1>Hello, {name}!</h1>;
}

// Arrow function
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;
```

## Class Components

```jsx
class Greeting extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}!</h1>;
    }
}
```

## Using Components

```jsx
function App() {
    return (
        <div>
            <Greeting name="John" />
            <Greeting name="Jane" />
        </div>
    );
}
```

## Component Composition

```jsx
function Card({ title, children }) {
    return (
        <div className="card">
            <h2>{title}</h2>
            {children}
        </div>
    );
}

function App() {
    return (
        <Card title="My Card">
            <p>This is the content</p>
        </Card>
    );
}
```

## Quick Revision

- Component = reusable UI piece
- Function or class components
- Props pass data to components
- Children for composition
- Return JSX

---

## Related Topics

- [[What-is-Components]] - [[What-is-Components|Component architecture]]
- [[What-is-React]] - [[What-is-React|React]]
- [[What-is-State]] - [[What-is-State|State management]]
- [[What-is-VirtualDOM]] - [[What-is-VirtualDOM|Virtual DOM]]
