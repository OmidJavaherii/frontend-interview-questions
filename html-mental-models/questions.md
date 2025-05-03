### How do you conceptualize the HTML Document Object Model (DOM) and its relationship to React’s Virtual DOM?

The **DOM** is the browser's live tree representation of the HTML document. Manipulating it directly is expensive due to reflows/repaints.

React introduces the **Virtual DOM (VDOM)** as a lightweight in-memory representation of the DOM. On state change:

- React computes a **diff** between the previous and next VDOM trees.
- It **batches and minimizes** real DOM operations using reconciliation.

```jsx
useState triggers re-render → VDOM diff → minimal DOM updates
```

This model improves performance and enables declarative UIs.

### What’s your mental model for structuring semantic HTML in a component-based framework like React?

Semantics should guide structure, even within components. I map **roles and intent** to proper tags (`<nav>`, `<main>`, `<section>`, `<article>`).

Example:

```jsx
function BlogPost({ title, content }) {
  return (
    <article>
      <h2>{title}</h2>
      <section>{content}</section>
    </article>
  );
}
```

I avoid div soup by treating components as semantic wrappers and using HTML5 tags when possible. This improves accessibility and SEO.

### How do you think about the interplay between HTML attributes (e.g., `data-*`, `aria-*`) and React props?

React treats standard HTML attributes and special-purpose ones (`aria-*`, `data-*`) as **props passed down to the DOM node** .

```jsx
<button aria-label="Close" data-testid="close-btn">
  X
</button>
```

- `data-*`: For testing, analytics, or behavior hooks.
- `aria-*`: For accessibility (screen reader support).
- Both are **first-class citizens** in JSX and should be handled deliberately.

### How would you design an HTML structure to support a complex, nested UI (e.g., a multi-level navigation menu) while keeping it maintainable?

Approach:

- Use semantic tags: `<nav>`, `<ul>`, `<li>`, and ARIA roles if needed.
- Recursively render nested data.
- Keep components atomic.

```jsx
function NavItem({ label, children }) {
  return (
    <li>
      {label}
      {children && <ul>{children.map(...</ul>}
    </li>
  );
}
```

I separate concerns (e.g., presentation vs logic) and ensure keyboard navigation and accessibility.

### What’s your approach to modeling HTML forms in a React application, especially for validation and submission?

I use **controlled components** for predictable state:

```jsx
const [email, setEmail] = useState("");
<input value={email} onChange={(e) => setEmail(e.target.value)} />;
```

For validation:

- Use **Yup/Zod** + **React Hook Form** or custom logic.
- Inline validation on blur/change.
- Prevent default on submit, handle async actions safely.

```jsx
<form onSubmit={handleSubmit(onSubmit)} />
```

### How do you reason about the impact of HTML parsing and rendering on page load performance?

The **critical rendering path** is impacted by:

- **HTML size** : Larger DOM = slower parse + render.
- **Blocking resources** : Synchronous scripts delay DOM parsing.
- **DOM depth** : Deep trees = more layout/reflow complexity.

I optimize by:

- Reducing initial HTML payload (e.g., lazy load non-critical UI).
- Prioritizing content order (above-the-fold).
- Using SSR/Streaming for time-to-first-byte wins.

### What’s your mental model for handling HTML events in a React application versus vanilla JavaScript?

In React:

- Events are **synthetic** : wrapped for cross-browser consistency.
- They bubble like native events but with React’s delegation system (on root).

```jsx
<button onClick={handleClick}>Click me</button>
```

Benefits:

- Consistent behavior.
- Easier memory management.
- Batched updates.

In vanilla JS:

- You add listeners directly to elements.
- Manual cleanup is required.

React simplifies the lifecycle and performance.

### How do you design HTML structures to support progressive enhancement in a React-based application?

I build HTML to work **without JavaScript first** , then enhance with React.

Example:

- Render SSR HTML with links and buttons functional.
- Hydrate on client for richer interaction (e.g., modals, search).

Use:

- Semantic HTML
- `<noscript>` fallback messaging
- Accessibility-first approach (keyboard navigation, visible focus)

```html
<button type="submit">Search</button>
```

Enhancement adds filters, animations, etc. after hydration.

### How do you think about the role of HTML in a server-side rendered (SSR) React application?

HTML is the **initial render** delivered to the browser in SSR:

- It improves **Time to First Paint (TTFP)** and SEO.
- React rehydrates the HTML into a live app.

My model:

- Keep the markup semantic and minimal for SSR.
- Use `<head>` tags for meta, title, preload (via `react-helmet` or `@vitejs/plugin-react`).
- Avoid non-deterministic rendering (e.g., `window`, `localStorage` on server).

### What’s your mental model for managing HTML metadata (e.g., `<meta>`, `<link>`) in a single-page application (SPA)?

I treat metadata as part of the **dynamic route context** . For SPAs:

- Use `react-helmet` or `@tanstack/router` to declaratively set `<title>`, `<meta>`, etc.
- On route change, update the head for SEO + social sharing.

```jsx
<Helmet>
  <title>Product Page</title>
  <meta name="description" content="Details about product" />
</Helmet>
```

For PWAs, also manage `<link rel="manifest">`, icons, and preload hints.
