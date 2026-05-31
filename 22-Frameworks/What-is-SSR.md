# What is SSR (Server-Side Rendering)

## Definition

Server-Side Rendering (SSR) is a technique where the HTML content of a web page is generated on the server and sent to the client as a fully rendered page. The client's browser receives the complete HTML and can display it immediately, while JavaScript loads in the background to make the page interactive.

## How SSR Works

```
1. Browser requests a page (e.g., /about)
2. Server receives the request
3. Server executes React/Vue/Svelte code
4. Server generates HTML from the component tree
5. Server sends complete HTML to browser
6. Browser displays the HTML immediately (visible)
7. JavaScript loads and hydrates the page (interactive)
8. Page is now a fully functional SPA
```

## SSR vs CSR

| Feature | SSR | CSR |
|---------|-----|-----|
| Initial Load | Faster (pre-rendered HTML) | Slower (blank page → render) |
| SEO | Excellent (HTML in response) | Poor (relies on JavaScript) |
| TTFB | Slightly higher | Lower |
| Interactivity Delay | Hydration delay | Immediate after load |
| Server Load | Higher | Lower |
| Complexity | More complex | Simpler |
| Caching | Easier (static HTML) | Harder |

## Implementation Examples

### Next.js (React SSR)

```javascript
// pages/index.js
// Next.js handles SSR automatically for pages

function HomePage({ posts }) {
  return (
    <div>
      <h1>Blog</h1>
      {posts.map(post => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.excerpt}</p>
        </article>
      ))}
    </div>
  );
}

// This runs on the server
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();

  return {
    props: { posts }  // Passed to component as props
  };
}

// Static Generation (SSG) - builds at build time
export async function getStaticProps() {
  const posts = await fetchPosts();

  return {
    props: { posts },
    revalidate: 60  // Revalidate every 60 seconds
  };
}
```

### Nuxt.js (Vue SSR)

```javascript
// pages/index.vue
<template>
  <div>
    <h1>{{ title }}</h1>
    <ul>
      <li v-for="post in posts" :key="post.id">
        {{ post.title }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get('/api/posts');
    return { posts: data.posts };
  },

  head() {
    return {
      title: this.title,
      meta: [
        { name: 'description', content: 'Blog posts' }
      ]
    };
  }
}
</script>
```

### SvelteKit SSR

```svelte
<!-- src/routes/+page.svelte -->
<script>
  export let data;  // Data from load function
</script>

<h1>{data.title}</h1>
{#each data.posts as post}
  <article>
    <h2>{post.title}</h2>
    <p>{post.content}</p>
  </article>
{/each}

<!-- src/routes/+page.js -->
<script context="module">
  export async function load({ fetch }) {
    const res = await fetch('/api/posts');
    const posts = await res.json();

    return {
      props: {
        title: 'Blog',
        posts
      }
    };
  }
</script>
```

## Hydration

Hydration is the process where JavaScript takes over the server-rendered HTML to make it interactive.

```javascript
// React hydration (Next.js handles this automatically)
import { hydrateRoot } from 'react-dom/client';

// Instead of createRoot (CSR)
// hydrateRoot reuses existing HTML
hydrateRoot(
  document.getElementById('root'),
  <App />
);

// What happens during hydration:
// 1. React attaches event listeners to existing HTML
// 2. React builds its internal state tree
// 3. Page becomes fully interactive
// 4. Client-side navigation takes over
```

## Common Use Cases

- **SEO-Critical Pages**: Blogs, marketing sites, e-commerce
- **Social Media Previews**: Open Graph tags need HTML in response
- **Content-Heavy Sites**: News, documentation
- **Performance on Slow Devices**: Server does the heavy lifting
- **Email Templates**: HTML must be in the response

## Common Mistakes

### Fetching Data in Client Components Only

```javascript
// BAD: Data only available after JavaScript loads (CSR)
function Blog() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetch('/api/posts')
      .then(res => res.json())
      .then(data => setPosts(data));
  }, []);

  return (
    <div>
      {posts.map(post => <Article key={post.id} {...post} />)}
    </div>
  );
}

// GOOD: Fetch data on server for SSR
// Next.js
export async function getServerSideProps() {
  const posts = await fetchPosts();
  return { props: { posts } };
}

function Blog({ posts }) {
  return (
    <div>
      {posts.map(post => <Article key={post.id} {...post} />)}
    </div>
  );
}
```

### Using Browser APIs During SSR

```javascript
// BAD: window/localStorage not available on server
function Component() {
  const theme = localStorage.getItem('theme');  // Error on server!

  return <div className={theme}>Hello</div>;
}

// GOOD: Check if window exists or use framework's method
function Component() {
  const [theme, setTheme] = useState('light');

  useEffect(() => {
    const saved = localStorage.getItem('theme');
    if (saved) setTheme(saved);
  }, []);

  return <div className={theme}>Hello</div>;
}

// Or in Next.js
function Component() {
  const theme = typeof window !== 'undefined'
    ? localStorage.getItem('theme')
    : 'light';

  return <div className={theme}>Hello</div>;
}
```

### Not Handling Loading States

```javascript
// BAD: No loading state during hydration
function Page({ data }) {
  return <div>{data.content}</div>;
}

// GOOD: Handle loading and hydration
import { Suspense } from 'react';

function Page({ data }) {
  return (
    <Suspense fallback={<Loading />}>
      <div>{data.content}</div>
    </Suspense>
  );
}
```

## SEO Benefits

```javascript
// SSR ensures search engines can crawl your content

// BAD: CSR - Search engine sees empty HTML
<html>
  <body>
    <div id="root"></div>
    <!-- Empty! Search engines can't see content -->
    <script src="bundle.js"></script>
  </body>
</html>

// GOOD: SSR - Search engine sees full content
<html>
  <head>
    <title>Blog Post</title>
    <meta name="description" content="Full article content...">
  </head>
  <body>
    <div id="root">
      <h1>Blog Post</h1>
      <p>Full article content that search engines can index...</p>
    </div>
    <script src="bundle.js"></script>
  </body>
</html>
```

## Framework Support

| Framework | SSR Support | Approach |
|-----------|------------|----------|
| Next.js | Built-in | File-based SSR/SSG |
| Nuxt.js | Built-in | File-based SSR/SSG |
| SvelteKit | Built-in | File-based SSR/SSG |
| React | Manual | react-dom/server |
| Vue | Manual | vue-server-renderer |
| Angular | Manual | Angular Universal |
| Svelte | Manual | svelte/server |

## Related Topics

- [[What-is-State]]
- [[What-is-Components]]
- [[What-is-Routing]]
- [[What-is-VirtualDOM]]
- [[Choose-Framework]]
- [[What-is-Svelte]]

## Quick Revision

- SSR renders HTML on the server, sends complete page to browser
- **Hydration**: JavaScript takes over server-rendered HTML
- **Benefits**: Better SEO, faster initial load, social media previews
- **Drawbacks**: More complex, higher server load, hydration delay
- **SSG** (Static Site Generation): Pre-renders at build time
- **ISR** (Incremental Static Regeneration): Updates static pages periodically
- Always check for `window` before using browser APIs in SSR
- Fetch data on server using `getServerSideProps` (Next) or `load` (SvelteKit)
