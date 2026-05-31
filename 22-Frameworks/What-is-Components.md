# What is Component-Based Architecture

## Definition

Component-based architecture is a design pattern where a UI is broken down into independent, reusable, and self-contained pieces called **components**. Each component encapsulates its own structure (HTML), behavior (JavaScript), and styling (CSS), and can be composed together to build complex interfaces.

## Core Principles

1. **Encapsulation**: Each component manages its own state and logic
2. **Reusability**: Components can be used multiple times across the application
3. **Composition**: Complex UIs are built by combining simpler components
4. **Separation of Concerns**: Each component handles a single responsibility

## Component Anatomy

### React Component

```javascript
// React: Functional Component
function UserCard({ name, email, avatar }) {
  return (
    <div className="user-card">
      <img src={avatar} alt={name} />
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}

// Usage
<UserCard
  name="John Doe"
  email="john@example.com"
  avatar="/images/john.jpg"
/>
```

### Vue Component

```vue
<!-- Vue: Single File Component -->
<template>
  <div class="user-card">
    <img :src="avatar" :alt="name" />
    <h3>{{ name }}</h3>
    <p>{{ email }}</p>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    email: String,
    avatar: String
  }
}
</script>

<style scoped>
.user-card {
  border: 1px solid #ccc;
  padding: 16px;
  border-radius: 8px;
}
</style>
```

### Svelte Component

```svelte
<!-- Svelte Component -->
<script>
  export let name;
  export let email;
  export let avatar;
</script>

<div class="user-card">
  <img src={avatar} alt={name} />
  <h3>{name}</h3>
  <p>{email}</p>
</div>

<style>
  .user-card {
    border: 1px solid #ccc;
    padding: 16px;
    border-radius: 8px;
  }
</style>
```

## Component Hierarchy

```javascript
// App
// ├── Header
// │   ├── Logo
// │   ├── Navigation
// │   │   ├── NavItem
// │   │   ├── NavItem
// │   │   └── NavItem
// │   └── UserMenu
// ├── MainContent
// │   ├── Sidebar
// │   │   ├── SearchBar
// │   │   └── CategoryList
// │   └── ContentArea
// │       ├── ArticleList
// │       │   ├── ArticleCard
// │       │   ├── ArticleCard
// │       │   └── ArticleCard
// │       └── Pagination
// └── Footer
//     ├── Links
//     └── Copyright
```

## Props (Passing Data)

Props are how components receive data from their parents.

### React

```javascript
// Parent component
function App() {
  const user = { name: "John", email: "john@example.com" };

  return (
    <div>
      <UserCard
        name={user.name}
        email={user.email}
      />
      <UserCard
        name="Jane"
        email="jane@example.com"
      />
    </div>
  );
}

// Child component
function UserCard({ name, email }) {
  return (
    <div>
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}
```

### Vue

```vue
<!-- Parent -->
<template>
  <UserCard :name="user.name" :email="user.email" />
</template>

<script>
import UserCard from './UserCard.vue';

export default {
  components: { UserCard },
  data() {
    return {
      user: { name: "John", email: "john@example.com" }
    }
  }
}
</script>

<!-- UserCard.vue -->
<template>
  <div>
    <h3>{{ name }}</h3>
    <p>{{ email }}</p>
  </div>
</template>

<script>
export default {
  props: ['name', 'email']
}
</script>
```

## Events (Child to Parent Communication)

### React

```javascript
// Parent
function App() {
  const [message, setMessage] = useState("");

  return (
    <div>
      <p>Message: {message}</p>
      <MessageInput onMessage={setMessage} />
    </div>
  );
}

// Child
function MessageInput({ onMessage }) {
  const [text, setText] = useState("");

  return (
    <div>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
      <button onClick={() => onMessage(text)}>
        Send
      </button>
    </div>
  );
}
```

### Vue

```vue
<!-- Parent -->
<template>
  <p>Message: {{ message }}</p>
  <MessageInput @message="message = $event" />
</template>

<!-- Child -->
<template>
  <div>
    <input v-model="text" />
    <button @click="$emit('message', text)">Send</button>
  </div>
</template>

<script>
export default {
  data() {
    return { text: '' }
  }
}
</script>
```

## Common Use Cases

- **UI Element Libraries**: Buttons, Modals, Cards, Dropdowns
- **Page Layouts**: Headers, Sidebars, Footers
- **Data Display**: Tables, Lists, Charts
- **Forms**: Input fields, Select menus, Form validation
- **Navigation**: Menus, Tabs, Breadcrumbs

## Common Mistakes

### Making Components Too Large

```javascript
// BAD: Everything in one component
function Dashboard() {
  // 500 lines of mixed concerns
}

// GOOD: Break into smaller components
function Dashboard() {
  return (
    <div>
      <DashboardHeader />
      <DashboardSidebar />
      <DashboardContent />
      <DashboardFooter />
    </div>
  );
}
```

### Not Reusing Components

```javascript
// BAD: Duplicating similar components
function UserCard1() { /* ... */ }
function UserCard2() { /* ... */ }
function UserCard3() { /* ... */ }

// GOOD: One reusable component
function UserCard({ user }) {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
    </div>
  );
}

// Usage
users.map(user => <UserCard key={user.id} user={user} />)
```

### Deep Prop Drilling

```javascript
// BAD: Passing props through many levels
function App() {
  return <Layout user={user} />;
}
function Layout({ user }) {
  return <Sidebar user={user} />;
}
function Sidebar({ user }) {
  return <UserMenu user={user} />;
}

// GOOD: Use Context or Store
const UserContext = createContext();

function App() {
  return (
    <UserContext.Provider value={user}>
      <Layout />
    </UserContext.Provider>
  );
}
function UserMenu() {
  const user = useContext(UserContext);
  return <div>{user.name}</div>;
}
```

## Related Topics

- [[What-is-State]]
- [[What-is-VirtualDOM]]
- [[What-is-Svelte]]
- [[Choose-Framework]]
- [[What-is-Routing]]

## Quick Revision

- Components are independent, reusable UI building blocks
- Each component encapsulates its own HTML, JS, and CSS
- Components communicate via **props** (parent → child) and **events** (child → parent)
- Keep components small, focused, and reusable
- Component hierarchy forms a tree structure
- Modern frameworks (React, Vue, Svelte) are built around component-based architecture
