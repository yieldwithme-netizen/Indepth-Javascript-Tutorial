# How to Lazy Load Modules in JavaScript

## Definition

Lazy loading is a design pattern that delays the loading of non-critical resources until they are needed. In JavaScript modules, lazy loading improves initial page load performance by splitting code into smaller chunks and loading them on demand using dynamic `import()`.

## Syntax

```javascript
// Basic lazy load
async function loadModule() {
  const module = await import('./heavy-module.js');
  module.init();
}

// React lazy
const Component = lazy(() => import('./Component'));
```

## Code Examples

### Basic Lazy Loading

```javascript
// utils.js - Heavy utility module
export function processLargeData(data) {
  return data.map(item => ({
    ...item,
    processed: true,
    timestamp: Date.now()
  }));
}

export function generateReport(data) {
  return data.reduce((acc, item) => {
    acc[item.type] = (acc[item.type] || 0) + 1;
    return acc;
  }, {});
}

// main.js - Only load when needed
let utils = null;

async function getUtils() {
  if (!utils) {
    utils = await import('./utils.js');
  }
  return utils;
}

// Usage
document.getElementById('process-btn').addEventListener('click', async () => {
  const { processLargeData } = await getUtils();
  const result = processLargeData(largeDataset);
  console.log(result);
});
```

### Lazy Loading Routes

```javascript
const routes = {
  '/': () => import('./pages/Home.js'),
  '/about': () => import('./pages/About.js'),
  '/contact': () => import('./pages/Contact.js'),
  '/dashboard': () => import('./pages/Dashboard.js')
};

async function loadRoute(path) {
  const loader = routes[path] || routes['/'];

  try {
    const page = await loader();
    document.getElementById('content').innerHTML = page.default();
    return true;
  } catch (error) {
    console.error('Failed to load route:', error);
    return false;
  }
}

// SPA navigation
window.addEventListener('popstate', () => {
  loadRoute(window.location.pathname);
});
```

### React Lazy Loading

```javascript
import React, { Suspense, lazy, useState, useEffect } from 'react';

// Lazy load components
const HeavyChart = lazy(() => import('./components/HeavyChart'));
const DataGrid = lazy(() => import('./components/DataGrid'));
const MarkdownEditor = lazy(() => import('./components/MarkdownEditor'));

function Dashboard() {
  const [activeTab, setActiveTab] = useState('charts');

  return (
    <div className="dashboard">
      <nav>
        <button onClick={() => setActiveTab('charts')}>Charts</button>
        <button onClick={() => setActiveTab('grid')}>Data Grid</button>
        <button onClick={() => setActiveTab('editor')}>Editor</button>
      </nav>

      <Suspense fallback={<div className="loading">Loading...</div>}>
        {activeTab === 'charts' && <HeavyChart data={chartData} />}
        {activeTab === 'grid' && <DataGrid data={gridData} />}
        {activeTab === 'editor' && <MarkdownEditor />}
      </Suspense>
    </div>
  );
}
```

### Lazy Loading Images

```javascript
class LazyImageLoader {
  constructor() {
    this.observer = new IntersectionObserver(
      (entries) => this.handleIntersection(entries),
      { rootMargin: '100px' }
    );
  }

  handleIntersection(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const img = entry.target;
        img.src = img.dataset.src;
        img.classList.add('loaded');
        this.observer.unobserve(img);
      }
    });
  }

  observe(images) {
    images.forEach(img => this.observer.observe(img));
  }
}

// Usage
const loader = new LazyImageLoader();
const images = document.querySelectorAll('img[data-src]');
loader.observe(images);
```

### Lazy Loading with Preloading

```javascript
class ModulePreloader {
  #cache = new Map();
  #preloadQueue = [];

  async load(modulePath) {
    if (this.#cache.has(modulePath)) {
      return this.#cache.get(modulePath);
    }

    const module = await import(modulePath);
    this.#cache.set(modulePath, module);
    return module;
  }

  preload(modulePaths) {
    return Promise.all(
      modulePaths.map(path => this.load(path))
    );
  }

  preloadInBackground(modulePaths) {
    modulePaths.forEach(path => {
      if (!this.#cache.has(path)) {
        this.load(path); // Don't await
      }
    });
  }
}

const preloader = new ModulePreloader();

// Preload critical modules
await preloader.preload(['./core.js', './utils.js']);

// Lazy load others
const charts = await preloader.load('./charts.js');
```

### Lazy Loading Utility Functions

```javascript
const lazyUtils = {
  _cache: {},

  async formatNumber(num) {
    if (!this._cache.formatNumber) {
      const { format } = await import('./number-utils.js');
      this._cache.formatNumber = format;
    }
    return this._cache.formatNumber(num);
  },

  async parseMarkdown(text) {
    if (!this._cache.parseMarkdown) {
      const { marked } = await import('./markdown-utils.js');
      this._cache.parseMarkdown = marked;
    }
    return this._cache.parseMarkdown(text);
  },

  async generatePDF(data) {
    if (!this._cache.generatePDF) {
      const { jsPDF } = await import('./pdf-utils.js');
      this._cache.generatePDF = new jsPDF();
    }
    return this._cache.generatePDF.generate(data);
  }
};

// Usage - only loads when called
const formatted = await lazyUtils.formatNumber(1234567);
const html = await lazyUtils.parseMarkdown('# Hello');
```

## Common Use Cases

| Use Case | Benefit |
|----------|---------|
| Route-based | Load pages only when visited |
| Component-based | Load complex components on demand |
| Feature-based | Load features based on user actions |
| Image Loading | Defer off-screen images |
| Admin Panels | Load admin tools only for admins |
| Heavy Libraries | Load charting, PDF, etc. when needed |

## Common Mistakes

```javascript
// ❌ Wrong: Loading everything upfront
import { heavyFunction } from './heavy.js'; // Always loaded
import { anotherHeavy } from './another-heavy.js';

// ✅ Correct: Lazy load what's needed
async function doWork() {
  const { heavyFunction } = await import('./heavy.js');
  heavyFunction();
}

// ❌ Wrong: No loading state
function App() {
  const [Component, setComponent] = useState(null);

  useEffect(() => {
    import('./HeavyComponent').then(m => setComponent(m.default));
  }, []);

  return <Component />; // null initially!
}

// ✅ Correct: Show loading state
function App() {
  const [Component, setComponent] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    import('./HeavyComponent')
      .then(m => setComponent(() => m.default))
      .finally(() => setLoading(false));
  }, []);

  if (loading) return <div>Loading...</div>;
  return <Component />;
}

// ❌ Wrong: Not handling errors
async function load() {
  const module = await import('./might-fail.js');
}

// ✅ Correct: Handle load failures
async function load() {
  try {
    const module = await import('./might-fail.js');
    return module;
  } catch {
    return await import('./fallback.js');
  }
}
```

## Related Topics

- [[What-is-DynamicImport]] - Dynamic import syntax
- [[Default-Export]] - Exporting modules
- [[What-is-Scope]] - Module scope
- [[Create-Class]] - Creating classes
- [[Implement-Encapsulation]] - Encapsulation patterns

## Quick Revision

| Concept | Description |
|---------|-------------|
| Purpose | Defer loading until needed |
| Primary API | `import()` dynamic imports |
| React | `React.lazy()` + `<Suspense>` |
| Caching | Store loaded modules to avoid re-loading |
| Preloading | Load in background before needed |
| Error Handling | Always handle load failures |
| Performance | Reduces initial bundle size |
