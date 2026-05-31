# React State Management

## Definition

React state management refers to the techniques and patterns used to handle data that changes over time in React applications. State determines how components render and behave. Effective state management ensures data consistency, prevents bugs, and improves application performance as complexity grows.

## Types of State

### Local State (Component State)

```javascript
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

### Derived/Computed State

```javascript
function TodoList({ todos }) {
  const completedCount = todos.filter(todo => todo.completed).length;
  const pendingCount = todos.length - completedCount;

  return (
    <div>
      <p>Total: {todos.length}</p>
      <p>Completed: {completedCount}</p>
      <p>Pending: {pendingCount}</p>
    </div>
  );
}
```

### Global State (Context API)

```javascript
import { createContext, useContext, useState } from 'react';

const UserContext = createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);

  return (
    <UserContext.Provider value={{ user, login, logout }}>
      {children}
    </UserContext.Provider>
  );
}

function useUser() {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}

// Usage in component
function Profile() {
  const { user, logout } = useUser();

  if (!user) return <p>Please log in</p>;

  return (
    <div>
      <h2>Welcome, {user.name}</h2>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

### useReducer for Complex State

```javascript
import { useReducer } from 'react';

const initialState = {
  items: [],
  loading: false,
  error: null
};

function todoReducer(state, action) {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        items: [...state.items, action.payload]
      };
    case 'TOGGLE_TODO':
      return {
        ...state,
        items: state.items.map(item =>
          item.id === action.payload
            ? { ...item, completed: !item.completed }
            : item
        )
      };
    case 'DELETE_TODO':
      return {
        ...state,
        items: state.items.filter(item => item.id !== action.payload)
      };
    case 'SET_LOADING':
      return { ...state, loading: action.payload };
    case 'SET_ERROR':
      return { ...state, error: action.payload };
    default:
      return state;
  }
}

function TodoApp() {
  const [state, dispatch] = useReducer(todoReducer, initialState);

  const addTodo = (text) => {
    dispatch({
      type: 'ADD_TODO',
      payload: { id: Date.now(), text, completed: false }
    });
  };

  return (
    <div>
      <button onClick={() => addTodo('New Todo')}>Add Todo</button>
      {state.items.map(todo => (
        <div key={todo.id}>
          <span
            style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
            onClick={() => dispatch({ type: 'TOGGLE_TODO', payload: todo.id })}
          >
            {todo.text}
          </span>
          <button
            onClick={() => dispatch({ type: 'DELETE_TODO', payload: todo.id })}
          >
            Delete
          </button>
        </div>
      ))}
    </div>
  );
}
```

## State Management Libraries

### Redux Toolkit (RTK)

```javascript
import { createSlice, configureStore } from '@reduxjs/toolkit';

const todoSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      state.push({
        id: Date.now(),
        text: action.payload,
        completed: false
      });
    },
    toggleTodo: (state, action) => {
      const todo = state.find(todo => todo.id === action.payload);
      if (todo) todo.completed = !todo.completed;
    }
  }
});

const store = configureStore({
  reducer: {
    todos: todoSlice.reducer
  }
});

export const { addTodo, toggleTodo } = todoSlice.actions;
```

### Zustand (Lightweight)

```javascript
import create from 'zustand';

const useTodoStore = create((set) => ({
  todos: [],
  addTodo: (text) => set((state) => ({
    todos: [...state.todos, { id: Date.now(), text, completed: false }]
  })),
  toggleTodo: (id) => set((state) => ({
    todos: state.todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    )
  }))
}));

// Usage
function TodoApp() {
  const { todos, addTodo, toggleTodo } = useTodoStore();

  return (
    <div>
      <button onClick={() => addTodo('New Todo')}>Add</button>
      {todos.map(todo => (
        <div key={todo.id} onClick={() => toggleTodo(todo.id)}>
          {todo.text}
        </div>
      ))}
    </div>
  );
}
```

## Common Use Cases

1. **Form handling** - Managing input values, validation, submission
2. **UI state** - Modal open/close, active tabs, theme
3. **Data fetching** - Loading states, cached data
4. **Shopping cart** - Items, quantities, totals
5. **Authentication** - User sessions, permissions

## Common Mistakes

1. **Mutating state directly** - Always create new objects/arrays
2. **Forgetting keys in lists** - Use unique, stable keys
3. **Overusing useState** - Consider useReducer for complex state
4. **Prop drilling too deep** - Use Context or state management library
5. **Not normalizing state** - Keep flat structures for complex data

```javascript
// WRONG: Mutating state
state.items.push(newItem);

// RIGHT: Creating new state
setState({ ...state, items: [...state.items, newItem] });
```

## Quick Revision Summary

- Local state: `useState` for simple component data
- Complex state: `useReducer` for multi-step state transitions
- Global state: Context API for lightweight, Redux/Zustand for complex apps
- Always create new state objects, never mutate
- Derive state when possible instead of storing duplicates
- Choose state management based on app complexity

## Related Topics

- [[React-Hooks]]
- [[React-Components]]
- [[Context-API]]
- [[Redux-Patterns]]
- [[State-Updating]]
- [[Performance-Optimization]]
