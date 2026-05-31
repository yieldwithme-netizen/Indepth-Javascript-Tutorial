# What is State Management

## Definition

State management is the process of managing and maintaining the data (state) of an application. State represents the current condition or snapshot of your application's data at any given time, including UI state, user input, server data, and application configuration.

## Types of State

### 1. Local Component State

```javascript
// React: useState hook
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

### 2. lifted State (Shared State)

```javascript
// State lifted to parent component
function App() {
  const [todos, setTodos] = useState([]);

  const addTodo = (text) => {
    setTodos([...todos, { id: Date.now(), text, done: false }]);
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, done: !todo.done } : todo
    ));
  };

  return (
    <div>
      <TodoInput onAdd={addTodo} />
      <TodoList items={todos} onToggle={toggleTodo} />
    </div>
  );
}
```

### 3. Global State (Application State)

```javascript
// React Context API
const TodoContext = createContext();

function TodoProvider({ children }) {
  const [todos, setTodos] = useState([]);

  const addTodo = (text) => {
    setTodos([...todos, { id: Date.now(), text, done: false }]);
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, done: !todo.done } : todo
    ));
  };

  return (
    <TodoContext.Provider value={{ todos, addTodo, toggleTodo }}>
      {children}
    </TodoContext.Provider>
  );
}

// Any descendant can access state
function TodoInput() {
  const { addTodo } = useContext(TodoContext);
  const [text, setText] = useState("");

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => { addTodo(text); setText(""); }}>
        Add
      </button>
    </div>
  );
}
```

## State Management Libraries

### Redux

```javascript
// actions/todoActions.js
export const ADD_TODO = 'ADD_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO';

export const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { id: Date.now(), text, done: false }
});

export const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id
});

// reducers/todoReducer.js
const initialState = [];

export function todoReducer(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, done: !todo.done }
          : todo
      );
    default:
      return state;
  }
}

// store.js
import { createStore } from 'redux';
import { todoReducer } from './reducers/todoReducer';

export const store = createStore(todoReducer);

// Component
function TodoInput() {
  const dispatch = useDispatch();
  const [text, setText] = useState("");

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => {
        dispatch(addTodo(text));
        setText("");
      }}>
        Add
      </button>
    </div>
  );
}
```

### Zustand (Lightweight)

```javascript
import { create } from 'zustand';

const useTodoStore = create((set) => ({
  todos: [],
  addTodo: (text) => set((state) => ({
    todos: [...state.todos, { id: Date.now(), text, done: false }]
  })),
  toggleTodo: (id) => set((state) => ({
    todos: state.todos.map(todo =>
      todo.id === id ? { ...todo, done: !todo.done } : todo
    )
  }))
}));

// Component
function TodoInput() {
  const addTodo = useTodoStore((state) => state.addTodo);
  const [text, setText] = useState("");

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => { addTodo(text); setText(""); }}>
        Add
      </button>
    </div>
  );
}
```

### Svelte Stores

```javascript
// stores/todos.js
import { writable } from 'svelte/store';

export const todos = writable([]);

export function addTodo(text) {
  todos.update(items => [
    ...items,
    { id: Date.now(), text, done: false }
  ]);
}

export function toggleTodo(id) {
  todos.update(items =>
    items.map(todo =>
      todo.id === id ? { ...todo, done: !todo.done } : todo
    )
  );
}

// Component
<script>
  import { todos, addTodo, toggleTodo } from './stores/todos.js';

  let text = '';

  function handleAdd() {
    addTodo(text);
    text = '';
  }
</script>

<input bind:value={text} />
<button on:click={handleAdd}>Add</button>

{#each $todos as todo}
  <div on:click={() => toggleTodo(todo.id)}>
    {todo.text}
  </div>
{/each}
```

## Common Use Cases

- **User Authentication**: Storing login state, user preferences
- **Shopping Cart**: Managing cart items, quantities, prices
- **Form Data**: Multi-step forms, form validation state
- **UI State**: Modal visibility, sidebar open/close, theme
- **Cached Data**: Server responses, API data caching

## Common Mistakes

### Over-Using Global State

```javascript
// BAD: Everything in global state
const store = createStore({
  todos: [],
  theme: 'dark',
  modalOpen: false,
  inputValue: '',  // This should be local state
  hoveredItem: null,  // This should be local state
  scrollPosition: [],  // This shouldn't be state at all
});

// GOOD: Use local state for component-specific data
function TodoInput() {
  const [text, setText] = useState("");  // Local state is fine
  const addTodo = useTodoStore(state => state.addTodo);  // Global for shared data

  return <input value={text} onChange={(e) => setText(e.target.value)} />;
}
```

### State Driven by Props

```javascript
// BAD: Deriving state from props
function UserCard({ user }) {
  const [fullName, setFullName] = useState("");

  useEffect(() => {
    setFullName(`${user.firstName} ${user.lastName}`);
  }, [user]);

  return <h3>{fullName}</h3>;
}

// GOOD: Derive directly in render
function UserCard({ user }) {
  const fullName = `${user.firstName} ${user.lastName}`;

  return <h3>{fullName}</h3>;
}
```

### Not Normalizing State

```javascript
// BAD: Nested state makes updates difficult
const state = {
  todos: [
    { id: 1, text: "Learn React", assignee: { name: "John", id: 1 } },
    { id: 2, text: "Learn Redux", assignee: { name: "John", id: 1 } }
  ]
};

// GOOD: Normalize state
const state = {
  todos: {
    1: { id: 1, text: "Learn React", assigneeId: 1 },
    2: { id: 2, text: "Learn Redux", assigneeId: 1 }
  },
  users: {
    1: { id: 1, name: "John" }
  }
};
```

## State Management Comparison

| Library | Bundle Size | Complexity | Best For |
|---------|------------|------------|----------|
| useState | 0KB | Low | Local state |
| Context API | 0KB | Low | Simple global state |
| Zustand | ~1KB | Low | Medium apps |
| Redux Toolkit | ~11KB | Medium | Large apps |
| MobX | ~16KB | Medium | Complex objects |
| Jotai | ~2KB | Low | Atomic state |
| Recoil | ~6KB | Low | React-specific |

## Related Topics

- [[What-is-Components]]
- [[What-is-VirtualDOM]]
- [[What-is-Svelte]]
- [[What-is-Routing]]
- [[Choose-Framework]]

## Quick Revision

- State represents the current data of your application
- **Local state**: Component-specific (useState)
- **Lifted state**: Shared between siblings (parent state)
- **Global state**: Application-wide (Context, Redux, Zustand)
- Don't over-use global state — keep state as local as possible
- Derive values instead of storing redundant state
- Normalize complex nested state for easier updates
- Choose state management based on app size and complexity
