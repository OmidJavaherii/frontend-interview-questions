### 1. How would you implement server-side rendering (SSR) in a React application?

**Approach** : Use a framework like **Next.js** , which abstracts the SSR setup.

**Concept** :

- SSR renders React components to HTML on the server.
- This improves **initial load performance** , **SEO** , and **time-to-interactive** .

  **Next.js Example** :

```tsx
// pages/index.tsx
export default function Home({ user }) {
  return <div>Hello, {user.name}</div>;
}

export async function getServerSideProps() {
  const res = await fetch("https://api.example.com/user");
  const user = await res.json();

  return { props: { user } };
}
```

**Custom SSR** (using `ReactDOMServer`):

```tsx
const html = ReactDOMServer.renderToString(<App />);
```

**When to use** : Public-facing pages that need SEO or fast first paint (e.g., landing pages, e-commerce).

<br />

### 2. What is the difference between controlled and uncontrolled components in React? When would you use each?

**Controlled** : Form state is managed by React via `useState`.

```tsx
const [value, setValue] = useState("");
<input value={value} onChange={(e) => setValue(e.target.value)} />;
```

**Uncontrolled** : Form state is handled by the DOM via a `ref`.

```tsx
const inputRef = useRef();
<input ref={inputRef} />;
```

**When to use** :

- **Controlled** : For dynamic validation, conditionally enabled fields, or complex form logic.
- **Uncontrolled** : For simple forms, uncontrolled inputs (file uploads), or performance-sensitive scenarios.

<br />

### 3. How do you handle error boundaries in React? Can you write a simple error boundary component?

**Concept** :

Error boundaries catch **rendering** , **lifecycle** , or **constructor** errors in child components — not event handlers or async code.

**Example** :

```tsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // Log error to monitoring service
    console.error(error, info);
  }

  render() {
    return this.state.hasError ? (
      <h1>Something went wrong.</h1>
    ) : (
      this.props.children
    );
  }
}

// Usage
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>;
```

**When to use** : Around major feature components, routes, or dynamic imports.

<br />

### 4. How would you architect a large-scale React application with hundreds of components to ensure scalability and maintainability?

**Key Practices** :

- **Modular folder structure** by domain or feature:
  ```
  /features/user
  /features/dashboard
  /shared/components
  ```
- **Atomic components** and reusable UI patterns (e.g., buttons, inputs).
- **Type safety** with TypeScript.
- **State management** scoped appropriately: local state → context → global (e.g., Redux/Zustand).
- **Code splitting** via React.lazy or Next.js dynamic imports.
- **Linting/formatting/testing** standards enforced with tooling (ESLint, Prettier, Jest).
- **Docs and Storybook** for component discovery and design system alignment.

<br />

### 5. What is the event delegation pattern, and how does React utilize it in its synthetic event system?

**Event Delegation** : A single handler (e.g., on `document`) listens for events from many child elements by leveraging event bubbling.

**React’s Synthetic Events** :

- React attaches one global event listener per event type to the **root DOM container** (not each element).
- Events are wrapped in a **SyntheticEvent** for consistent cross-browser behavior.
- React bubbles events through the **virtual DOM tree** instead of the actual DOM.

  **Example** :

```tsx
function App() {
  return <button onClick={() => console.log("clicked")}>Click Me</button>;
}
```

Under the hood, this `onClick` is handled via a global listener on the root, improving **performance** and **memory efficiency** .
