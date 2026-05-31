# What is Routing

## Definition

Routing is the mechanism that maps URLs to specific views or components in a web application. It enables navigation between different pages or views without full page reloads, providing a seamless single-page application (SPA) experience.

## Types of Routing

### 1. Client-Side Routing

```javascript
// Router runs in the browser
// URL changes without server request
// Components render based on current URL

// Example: React Router
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users/:id" element={<UserProfile />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### 2. Server-Side Routing

```javascript
// Server handles URL mapping
// Full page reload on navigation
// Traditional multi-page applications

// Example: Express.js
app.get('/', (req, res) => {
  res.sendFile('home.html');
});

app.get('/about', (req, res) => {
  res.sendFile('about.html');
});
```

## Client-Side Routing Implementation

### React Router

```javascript
import {
  BrowserRouter,
  Routes,
  Route,
  Link,
  useParams,
  useNavigate
} from 'react-router-dom';

// Layout Component
function Layout() {
  return (
    <div>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
      </nav>

      <main>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/users" element={<Users />} />
          <Route path="/users/:id" element={<UserProfile />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </main>
    </div>
  );
}

// Page Components
function Home() {
  return <h1>Home Page</h1>;
}

function About() {
  return <h1>About Page</h1>;
}

function Users() {
  return <h1>Users List</h1>;
}

function UserProfile() {
  const { id } = useParams();
  const navigate = useNavigate();

  return (
    <div>
      <h1>User Profile: {id}</h1>
      <button onClick={() => navigate('/')}>
        Go Home
      </button>
    </div>
  );
}

function NotFound() {
  return <h1>404 - Page Not Found</h1>;
}
```

### Vue Router

```javascript
import { createRouter, createWebHistory } from 'vue-router';
import Home from './views/Home.vue';
import About from './views/About.vue';
import UserProfile from './views/UserProfile.vue';

const routes = [
  { path: '/', name: 'Home', component: Home },
  { path: '/about', name: 'About', component: About },
  { path: '/users/:id', name: 'UserProfile', component: UserProfile },
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;

// In main.js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';

createApp(App).use(router).mount('#app');

// In components
<template>
  <nav>
    <router-link to="/">Home</router-link>
    <router-link to="/about">About</router-link>
  </nav>

  <router-view />
</template>
```

### SvelteKit Routing

```svelte
<!-- SvelteKit: File-based routing -->
<!-- src/routes/+page.svelte = Home page -->
<script>
  import { goto } from '$app/navigation';
</script>

<h1>Home Page</h1>
<button on:click={() => goto('/about')}>Go to About</button>

<!-- src/routes/about/+page.svelte = About page -->
<h1>About Page</h1>

<!-- src/routes/users/[id]/+page.svelte = Dynamic route -->
<script>
  import { page } from '$app/stores';
</script>

<h1>User: {$page.params.id}</h1>
```

## Route Parameters

```javascript
// Dynamic URL segments
// /users/123 → params.id = "123"
// /posts/hello-world → params.slug = "hello-world"

// React Router
<Route path="/users/:id" element={<User />} />

function User() {
  const { id } = useParams();
  // id = "123" for URL /users/123
}

// Vue Router
{ path: '/users/:id', name: 'User', component: User }

// SvelteKit
// File: src/routes/users/[id]/+page.svelte
// Access: $page.params.id
```

## Navigation

### Programmatic Navigation

```javascript
// React Router
function SearchForm() {
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();
    navigate('/search?q=' + searchTerm);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={(e) => setSearchTerm(e.target.value)} />
      <button type="submit">Search</button>
    </form>
  );
}

// Vue Router
this.$router.push('/about');
this.$router.push({ name: 'User', params: { id: 123 } });

// SvelteKit
import { goto } from '$app/navigation';

goto('/about');
goto('/users/123');
```

### Navigation Guards

```javascript
// React Router
function ProtectedRoute({ children }) {
  const { isAuthenticated } = useAuth();

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return children;
}

// Usage
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>

// Vue Router
const router = createRouter({
  routes: [
    {
      path: '/dashboard',
      component: Dashboard,
      meta: { requiresAuth: true }
    }
  ]
});

router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isAuthenticated()) {
    next('/login');
  } else {
    next();
  }
});
```

## Nested Routes

```javascript
// React Router
<Route path="/users" element={<UsersLayout />}>
  <Route index element={<UsersList />} />
  <Route path=":id" element={<UserDetail />} />
  <Route path=":id/posts" element={<UserPosts />} />
</Route>

function UsersLayout() {
  return (
    <div>
      <h1>Users</h1>
      <Outlet /> {/* Child routes render here */}
    </div>
  );
}

// Vue Router
{
  path: '/users',
  component: UsersLayout,
  children: [
    { path: '', component: UsersList },
    { path: ':id', component: UserDetail },
    { path: ':id/posts', component: UserPosts }
  ]
}
```

## Common Use Cases

- **Multi-page SPAs**: Navigation between views without reloads
- **Dynamic URLs**: `/users/123`, `/posts/hello-world`
- **Protected Routes**: Authentication-based access control
- **Nested Layouts**: Shared layouts with child routes
- **404 Pages**: Catch-all routes for undefined paths
- **Breadcrumbs**: URL-based navigation history

## Common Mistakes

### Forgetting to Wrap in Router Provider

```javascript
// BAD: Routes won't work without provider
function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
    </Routes>
  );
}

// GOOD: Wrap with provider
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Using `<a>` Instead of `<Link>`

```javascript
// BAD: Full page reload
<a href="/about">About</a>

// GOOD: Client-side navigation
<Link to="/about">About</Link>
```

### Not Handling 404 Routes

```javascript
// BAD: No fallback for unknown routes
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>

// GOOD: Add catch-all route
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="*" element={<NotFound />} />
</Routes>
```

### Not Preserving Scroll Position

```javascript
// BAD: Page scrolls to top on every navigation
// Some routers do this by default

// GOOD: Preserve scroll for back/forward navigation
<BrowserRouter
  future={{
    v7_scrollRestoration: true  // React Router v6.4+
  }}
>
```

## Related Topics

- [[What-is-State]]
- [[What-is-Components]]
- [[What-is-SSR]]
- [[Choose-Framework]]
- [[What-is-Svelte]]

## Quick Revision

- Routing maps URLs to views without full page reloads
- **Client-side routing**: Runs in browser, enables SPAs
- **Server-side routing**: Traditional multi-page apps
- Use `<Link>` instead of `<a>` for client-side navigation
- Route parameters enable dynamic URLs (`:id`)
- Navigation guards protect routes from unauthorized access
- Nested routes enable shared layouts with child views
- Always handle 404 routes with a catch-all pattern
