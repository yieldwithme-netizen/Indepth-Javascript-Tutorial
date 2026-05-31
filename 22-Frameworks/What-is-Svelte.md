# What is Svelte

## Definition

Svelte is a radical new approach to building user interfaces. Unlike traditional frameworks like React, Vue, or Angular, Svelte is a **compiler** that converts your component code into highly efficient vanilla JavaScript at build time. There is no virtual DOM — Svelte generates direct DOM manipulations that run at vanilla JavaScript speed.

## Key Characteristics

- **Compiler-based**: Svelte compiles components to efficient imperative code at build time
- **No Virtual DOM**: Updates the real DOM directly with minimal overhead
- **Less Boilerplate**: Write significantly less code compared to other frameworks
- **Reactive by Default**: Variables are automatically reactive — no need for useState or useState-like patterns
- **Truly Reactive Assignments**: Reassignments trigger updates automatically

## Basic Component Syntax

```svelte
<script>
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<button on:click={increment}>
  Count: {count}
</button>

<style>
  button {
    padding: 10px 20px;
    font-size: 1rem;
    cursor: pointer;
  }
</style>
```

### How It Works

1. `let count = 0` — declares a reactive variable
2. `count += 1` — assignment triggers DOM update (no setter function needed)
3. `{count}` — mustache syntax binds variable to template
4. `<style>` — scoped CSS by default

## Reactivity in Svelte

Svelte uses the assignment operator `=` to trigger reactivity. This is different from React where you use `setState` or the `useState` setter.

```svelte
<script>
  let items = [];

  function addItem() {
    items = [...items, items.length]; // Must reassign to trigger reactivity
  }

  // This won't trigger reactivity:
  // items.push(items.length);
</script>

<button on:click={addItem}>Add Item</button>

<ul>
  {#each items as item}
    <li>{item}</li>
  {/each}
</ul>
```

## Store System (Svelte Stores)

Svelte has a built-in store system that provides reactive state management without external libraries.

```svelte
<script>
  import { writable } from 'svelte/store';

  const count = writable(0);

  function increment() {
    count.update(n => n + 1);
  }
</script>

<button on:click={increment}>
  Count: {$count}
</button>
```

## SvelteKit (Application Framework)

SvelteKit is the official application framework for Svelte, similar to Next.js for React.

```javascript
// src/routes/+page.svelte
<script>
  export let data;

  let { posts } = data;
</script>

{#each posts as post}
  <article>
    <h2>{post.title}</h2>
    <p>{post.excerpt}</p>
  </article>
{/each}
```

## Common Use Cases

- **Small to Medium SPAs**: Excellent performance with minimal overhead
- **Static Sites**: SvelteKit provides excellent static generation
- **Prototypes**: Rapid development with less boilerplate
- **Performance-Critical Apps**: Direct DOM updates are very fast
- **Embedded Widgets**: Small bundle sizes for widget integration

## Common Mistakes

### Forgetting Reassignment

```svelte
<script>
  let items = [];

  // WRONG — doesn't trigger reactivity
  function addItem() {
    items.push(items.length);
  }

  // CORRECT — reassign to trigger update
  function addItem() {
    items = [...items, items.length];
  }
</script>
```

### Mutating Arrays Directly

```svelte
<script>
  let todos = [];

  // WRONG
  function toggleTodo(index) {
    todos[index].done = !todos[index].done;
  }

  // CORRECT
  function toggleTodo(index) {
    todos = todos.map((todo, i) =>
      i === index ? { ...todo, done: !todo.done } : todo
    );
  }
</script>
```

### Not Using the $: Syntax for Derived Values

```svelte
<script>
  let width = 10;
  let height = 20;

  // Reactive declaration using $:
  $: area = width * height;

  // Reactive statement using $:
  $: console.log('Area changed:', area);
</script>

<p>Area: {area}</p>
```

## Comparison with React

| Feature | Svelte | React |
|---------|--------|-------|
| DOM Updates | Compiler-generated direct updates | Virtual DOM diffing |
| State | `let` variables | `useState` hook |
| Boilerplate | Minimal | More verbose |
| Bundle Size | Very small | Larger (includes React runtime) |
| Learning Curve | Lower | Moderate |
| Reactivity | Assignment-based | Explicit state updates |

## Related Topics

- [[What-is-Components]]
- [[What-is-State]]
- [[What-is-VirtualDOM]]
- [[Choose-Framework]]
- [[What-is-Routing]]
- [[What-is-SSR]]

## Quick Revision

- Svelte is a **compiler** that generates vanilla JavaScript at build time
- No virtual DOM — direct DOM manipulation for maximum performance
- Reactivity is triggered by **assignment** (`=`), not explicit state setters
- Built-in store system for state management
- SvelteKit provides routing and SSR capabilities
- Significantly less boilerplate than React or Angular
- Bundle size is much smaller since no framework runtime is needed
