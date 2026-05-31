# What is Code Splitting

## Definition

**Code splitting** is a build optimization technique that breaks your bundle into smaller chunks loaded on demand, reducing initial page load time.

## Why Code Split?

```javascript
// Without code splitting: Single large bundle
import { heavyFunction } from "./heavy-module";
// User downloads entire bundle even if they never use heavyFunction
```

## Webpack Dynamic Imports

### Route-Based Splitting

```javascript
// Before: Static import
import Dashboard from "./Dashboard";

// After: Dynamic import (creates separate chunk)
const Dashboard = React.lazy(() => import("./Dashboard"));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </Suspense>
  );
}
```

### Component-Based Splitting

```javascript
const HeavyChart = React.lazy(() => import("./HeavyChart"));

function Dashboard() {
  const [showChart, setShowChart] = useState(false);

  return (
    <div>
      <button onClick={() => setShowChart(true)}>Show Chart</button>
      {showChart && (
        <Suspense fallback={<Spinner />}>
          <HeavyChart />
        </Suspense>
      )}
    </div>
  );
}
```

## Native ES Modules (No Build Tool)

```javascript
// Dynamic import in vanilla JS
button.addEventListener("click", async () => {
  const { default: Chart } = await import("./chart.js");
  const chart = new Chart(data);
  chart.render();
});
```

## Vite Code Splitting

```javascript
// Vite automatically splits by route with dynamic import
const About = () => import("./pages/About.vue");

const routes = [
  { path: "/", component: Home },
  { path: "/about", component: About },
];
```

## Prefetching

```javascript
// Prefetch during idle time
const link = document.createElement("link");
link.rel = "prefetch";
link.href = "/chunk-about.js";
document.head.appendChild(link);
```

## Common Use Cases

- **Single Page Apps**: Route-based chunks
- **Feature toggles**: Load features on demand
- **Modal/Dialogs**: Lazy load rarely used UI
- **Admin panels**: Separate from main bundle

## Common Mistakes

```javascript
// Mistake: Over-splitting (too many small chunks)
const Button = React.lazy(() => import("./Button")); // Bad: tiny module

// Good: Split at meaningful boundaries
const AdminPanel = React.lazy(() => import("./AdminPanel")); // Good: large feature
```

## Related Topics

- [[What-is-LazyLoading]]
- [[What-is-TreeShaking]]
- [[Optimize-Rendering]]
- [[Measure-Performance]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| What | Split bundle into chunks |
| Why | Reduce initial load time |
| How | Dynamic `import()` |
| Route split | One chunk per page |
| Component split | Load features on demand |
