# Redux Patterns

## Definition

Redux patterns are **best practices** for state management with Redux.

## Basic Setup

```javascript
// Actions
const ADD_TODO = 'ADD_TODO';
const addTodo = (text) => ({ type: ADD_TODO, text });

// Reducer
function todos(state = [], action) {
    switch (action.type) {
        case ADD_TODO:
            return [...state, { text: action.text, completed: false }];
        default:
            return state;
    }
}

// Store
const store = createStore(todos);

// Dispatch
store.dispatch(addTodo('Learn Redux'));
```

## Quick Revision

- Redux: predictable state container
- Actions: describe what happened
- Reducers: specify state changes
- Store: holds application state
- Dispatch: triggers state updates

---

## Related Topics

- [[What-is-State]] - [[What-is-State|State management]]
- [[Redux-Patterns]] - [[Redux-Patterns|Redux patterns]]
- [[React-State-Management]] - [[React-State-Management|React state]]
- [[What-is-State]] - [[What-is-State|State]]
