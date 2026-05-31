# What is Lazy Loading

## Definition

**Lazy loading** is a design pattern that defers initialization of resources until they are actually needed, reducing initial load time and memory consumption.

## Image Lazy Loading

### Native Browser Support

```html
<img src="placeholder.jpg" data-src="actual-image.jpg" loading="lazy" alt="Lazy loaded" />
```

### JavaScript Implementation

```javascript
const lazyImages = document.querySelectorAll("img[data-src]");

const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      img.removeAttribute("data-src");
      imageObserver.unobserve(img);
    }
  });
});

lazyImages.forEach((img) => imageObserver.observe(img));
```

## Component Lazy Loading

### React

```javascript
const Settings = React.lazy(() => import("./Settings"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

### Vue

```javascript
const routes = [
  {
    path: "/dashboard",
    component: () => import("./views/Dashboard.vue"),
  },
];
```

## Vanilla JavaScript

```javascript
function loadScript(url) {
  return new Promise((resolve, reject) => {
    const script = document.createElement("script");
    script.src = url;
    script.onload = resolve;
    script.onerror = reject;
    document.body.appendChild(script);
  });
}

// Load only when needed
button.addEventListener("click", async () => {
  await loadScript("/analytics.js");
  trackEvent("button_click");
});
```

## Intersection Observer Pattern

```javascript
function lazyLoad(element, loadFn) {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          loadFn(entry.target);
          observer.unobserve(entry.target);
        }
      });
    },
    { rootMargin: "100px" } // Start loading 100px before visible
  );

  observer.observe(element);
}

// Usage
lazyLoad(document.getElementById("heavy-component"), (el) => {
  import("./HeavyComponent.js").then((mod) => mod.init(el));
});
```

## Common Use Cases

- **Images**: Load when scrolled into view
- **Routes**: Load page content on navigation
- **Modals**: Load dialog content on first open
- **Infinite scroll**: Load next batch of data
- **Below-fold content**: Defer non-visible sections

## Common Mistakes

```javascript
// Mistake: No placeholder/reserve space
// Causes layout shifts when content loads

// Good: Reserve space
<img
  src="placeholder.jpg"
  data-src="actual.jpg"
  loading="lazy"
  style="width: 300px; height: 200px"
/>
```

## Related Topics

- [[What-is-CodeSplitting]]
- [[Optimize-Rendering]]
- [[Measure-Performance]]
- [[What-is-TreeShaking]]

## Quick Revision

| Type | Implementation |
|------|----------------|
| Images | `loading="lazy"` or IntersectionObserver |
| Components | `React.lazy()` / dynamic `import()` |
| Scripts | Dynamic `<script>` creation |
| Data | Fetch on scroll/demand |
