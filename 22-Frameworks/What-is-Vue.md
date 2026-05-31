# What is Vue.js?

Vue.js is a progressive JavaScript framework for building user interfaces. It's designed from the ground up to be incrementally adoptable, making it easy to integrate with other libraries or existing projects.

## Definition

Vue.js is a lightweight, flexible framework that focuses on the view layer of applications. It uses a reactive data-binding system and component-based architecture to build modern web interfaces. Vue combines the best features of Angular and React while being simpler to learn.

## Core Concepts

### 1. Reactivity System
```javascript
// Vue 3 Composition API
import { ref, reactive, computed } from 'vue'

export default {
    setup() {
        // Reactive references
        const count = ref(0)
        const user = reactive({
            name: 'John',
            age: 30
        })
        
        // Computed properties
        const doubleCount = computed(() => count.value * 2)
        
        // Methods
        function increment() {
            count.value++
            user.age++
        }
        
        return { count, user, doubleCount, increment }
    }
}
```

### 2. Single File Components (SFC)
```vue
<template>
    <div class="counter">
        <h1>{{ title }}</h1>
        <p>Count: {{ count }}</p>
        <p>Double: {{ doubleCount }}</p>
        <button @click="increment">+1</button>
        <button @click="decrement">-1</button>
    </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
    name: 'Counter',
    props: {
        title: {
            type: String,
            default: 'Counter'
        }
    },
    setup(props) {
        const count = ref(0)
        const doubleCount = computed(() => count.value * 2)
        
        function increment() { count.value++ }
        function decrement() { count.value-- }
        
        return { count, doubleCount, increment, decrement }
    }
}
</script>

<style scoped>
.counter {
    padding: 20px;
    border: 1px solid #ccc;
}
</style>
```

### 3. Template Syntax
```vue
<template>
    <!-- Interpolation -->
    <h1>{{ message }}</h1>
    
    <!-- Directives -->
    <p v-if="show">Conditional rendering</p>
    <p v-else>Else block</p>
    
    <ul>
        <li v-for="item in items" :key="item.id">
            {{ item.name }}
        </li>
    </ul>
    
    <!-- Event handling -->
    <button @click="handleClick">Click me</button>
    <input @input="handleInput" :value="text">
    
    <!-- Binding -->
    <a :href="url">Link</a>
    <div :class="{ active: isActive }">Styled</div>
</template>
```

### 4. Component Communication
```vue
<!-- Parent Component -->
<template>
    <child-component 
        :message="parentMessage"
        @update="handleUpdate"
    />
</template>

<!-- Child Component -->
<template>
    <div>
        <p>{{ message }}</p>
        <button @click="$emit('update', newValue)">Update</button>
    </div>
</template>

<script>
export default {
    props: ['message'],
    emits: ['update']
}
</script>
```

## Common Use Cases

### 1. Building a Todo App
```vue
<template>
    <div>
        <input v-model="newTodo" @keyup.enter="addTodo">
        <button @click="addTodo">Add</button>
        
        <ul>
            <li v-for="todo in todos" :key="todo.id">
                <input type="checkbox" v-model="todo.done">
                <span :class="{ done: todo.done }">{{ todo.text }}</span>
                <button @click="removeTodo(todo.id)">Remove</button>
            </li>
        </ul>
        
        <p>{{ remaining }} items left</p>
    </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
    setup() {
        const todos = ref([])
        const newTodo = ref('')
        
        const remaining = computed(
            () => todos.value.filter(t => !t.done).length
        )
        
        function addTodo() {
            if (newTodo.value.trim()) {
                todos.value.push({
                    id: Date.now(),
                    text: newTodo.value,
                    done: false
                })
                newTodo.value = ''
            }
        }
        
        function removeTodo(id) {
            todos.value = todos.value.filter(t => t.id !== id)
        }
        
        return { todos, newTodo, remaining, addTodo, removeTodo }
    }
}
</script>
```

### 2. Using Vuex/Pinia for State Management
```javascript
// Pinia store (Vue 3 recommended)
import { defineStore } from 'pinia'

export const useUserStore = defineStore('user', {
    state: () => ({
        users: [],
        currentUser: null
    }),
    getters: {
        getUserById: (state) => (id) => {
            return state.users.find(u => u.id === id)
        }
    },
    actions: {
        async fetchUsers() {
            const response = await fetch('/api/users')
            this.users = await response.json()
        },
        setCurrentUser(user) {
            this.currentUser = user
        }
    }
})
```

## Common Mistakes

1. **Mutating props directly** - Use events instead:
   ```vue
   <!-- Wrong -->
   <template>
       <input :value="propValue" @input="propValue = $event.target.value">
   </template>
   
   <!-- Correct -->
   <template>
       <input :value="propValue" @input="$emit('update', $event.target.value)">
   </template>
   ```

2. **Not using `:key` with `v-for`** - Poor performance:
   ```vue
   <!-- Wrong -->
   <li v-for="item in items">{{ item.name }}</li>
   
   <!-- Correct -->
   <li v-for="item in items" :key="item.id">{{ item.name }}</li>
   ```

3. **Overusing Vue 2 syntax in Vue 3** - Use Composition API:
   ```javascript
   // Vue 2 Options API (still works but less flexible)
   export default {
       data() { return { count: 0 } },
       methods: { increment() { this.count++ } }
   }
   
   // Vue 3 Composition API (recommended)
   export default {
       setup() {
           const count = ref(0)
           function increment() { count.value++ }
           return { count, increment }
       }
   }
   ```

## Related Topics

- [[What-is-Angular]]
- [[Compile-TypeScript]]

## Quick Revision

- Vue.js is a progressive, lightweight JavaScript framework
- Uses reactive data-binding and component-based architecture
- Single File Components (SFC) combine template, script, and style
- Vue 3 Composition API provides better code organization
- Directives: `v-if`, `v-for`, `v-model`, `v-bind`, `v-on`
- Use Pinia for state management in Vue 3
- Vue is incrementally adoptable - use as much or as little as needed
- Great for both small projects and large enterprise applications
