# What is Dynamic Import in JavaScript

## Definition

Dynamic import is a JavaScript feature that allows you to load modules asynchronously at runtime using the `import()` function expression. Unlike static imports that are resolved at parse time, dynamic imports enable on-demand loading, code splitting, and conditional module loading.

## Syntax

```javascript
// Basic syntax
import('./module.js')
  .then(module => {
    // Use module exports
  })
  .catch(error => {
    // Handle error
  });

// With async/await
const module = await import('./module.js');
```

## Code Examples

### Basic Dynamic Import

```javascript
// math.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

// main.js
async function loadMath() {
  const math = await import('./math.js');

  console.log(math.add(5, 3)); // 8
  console.log(math.subtract(5, 3)); // 2
}

loadMath();
```

### Conditional Dynamic Import

```javascript
async function loadFeature(featureName) {
  switch (featureName) {
    case 'charts':
      return await import('./features/charts.js');
    case 'tables':
      return await import('./features/tables.js');
    case 'editor':
      return await import('./features/editor.js');
    default:
      throw new Error(`Unknown feature: ${featureName}`);
  }
}

async function initApp() {
  const charts = await loadFeature('charts');
  charts.init('#chart-container');
}
```

### Dynamic Import with Error Handling

```javascript
async function loadModuleSafely(modulePath) {
  try {
    const module = await import(modulePath);
    return { success: true, module };
  } catch (error) {
    console.error(`Failed to load ${modulePath}:`, error);
    return { success: false, error };
  }
}

async function init() {
  const result = await loadModuleSafely('./heavy-module.js');

  if (result.success) {
    result.module.init();
  } else {
    console.log('Using fallback...');
  }
}
```

### Dynamic Import in React

```javascript
import React, { useState, useEffect, lazy, Suspense } from 'react';

// Method 1: React.lazy
const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}

// Method 2: Manual dynamic import
function Dashboard() {
  const [Chart, setChart] = useState(null);

  useEffect(() => {
    import('./Chart').then(module => {
      setChart(() => module.default);
    });
  }, []);

  if (!Chart) return <div>Loading chart...</div>;

  return <Chart data={[]} />;
}
```

### Dynamic Import with Caching

```javascript
const moduleCache = new Map();

async function importWithCache(modulePath) {
  if (moduleCache.has(modulePath)) {
    return moduleCache.get(modulePath);
  }

  const module = await import(modulePath);
  moduleCache.set(modulePath, module);
  return module;
}

// Usage
const charts = await importWithCache('./charts.js');
const charts2 = await importWithCache('./charts.js'); // Cached
console.log(charts === charts2); // true
```

### Dynamic Import in Node.js

```javascript
// ESM (ECMAScript Modules)
async function loadConfig() {
  const { readFileSync } = await import('fs');
  const config = JSON.parse(readFileSync('./config.json', 'utf-8'));
  return config;
}

// Conditional import based on environment
async function getDatabaseDriver() {
  if (process.env.NODE_ENV === 'production') {
    return await import('./drivers/postgres.js');
  }
  return await import('./drivers/sqlite.js');
}
```

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| Code Splitting | Load only needed code per route |
| Lazy Loading | Defer loading non-critical modules |
| Feature Flags | Load features based on conditions |
| Plugin Systems | Load plugins on demand |
| Heavy Components | Defer loading complex UI components |
| Environment-Based | Different modules for dev/prod |

## Common Mistakes

```javascript
// ❌ Wrong: Using dynamic import in static context
if (condition) {
  import './module.js'; // SyntaxError
}

// ✅ Correct: Always use import() as function
if (condition) {
  await import('./module.js');
}

// ❌ Wrong: Not handling errors
async function load() {
  const module = await import('./module.js'); // Unhandled rejection
}

// ✅ Correct: Always handle errors
async function load() {
  try {
    const module = await import('./module.js');
    return module;
  } catch (error) {
    console.error('Import failed:', error);
  }
}

// ❌ Wrong: Importing with wrong path
const module = await import('module.js'); // Missing ./ or package name

// ✅ Correct: Use relative paths or package names
const module = await import('./module.js');
const react = await import('react');

// ❌ Wrong: Assuming synchronous behavior
import('./module.js').then(module => {
  // This runs async!
});
console.log('This runs before module loads');

// ✅ Correct: Use async/await for sequential logic
await import('./module.js');
console.log('Module is now loaded');
```

## Related Topics

- [[Lazy-Load]] - Lazy loading modules
- [[Default-Export]] - Default export syntax
- [[What-is-Scope]] - Module scope concepts
- [[Create-Class]] - Creating classes for modules
- [[Implement-Encapsulation]] - Encapsulation principles

## Quick Revision

| Concept | Description |
|---------|-------------|
| Syntax | `import('./module.js')` |
| Returns | Promise resolving to module namespace |
| Async | Always asynchronous, never synchronous |
| Code Splitting | Primary use for reducing bundle size |
| Error Handling | Always use try/catch or .catch() |
| Caching | Cache results to avoid re-loading |
| React.lazy | Built-in React support for dynamic import |
