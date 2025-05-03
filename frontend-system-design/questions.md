### 1. How would you design a frontend system to handle a high-traffic e-commerce platform with millions of concurrent users?

**Key strategies** :

- **Server-Side Rendering (SSR)** or **Static Site Generation (SSG)** via Next.js for product/category pages to reduce TTFB.
- **CDN caching** for static assets and SSR responses.
- Use **code-splitting** and **lazy loading** for product details, reviews, etc.
- Offload heavy UI (e.g., search, filters) using **client-side rendering** with async APIs.

```jsx
const Product = React.lazy(() => import("./Product"));
```

<br />

### 2. What’s your approach to designing a frontend architecture that supports real-time updates across multiple clients (e.g., a chat app or live dashboard)?

**Approach** :

- Use **WebSockets** (e.g., [Socket.IO](http://Socket.IO) or native) or **Server-Sent Events** for real-time data.
- Use state management with **event queues** to debounce updates and ensure UI consistency.

```jsx
socket.on("newMessage", (msg) => {
  dispatch(addMessage(msg));
});
```

- Implement **presence** , **typing indicators** , and **message queues** on client for resilience.

<br />

### 3. How do you design a frontend system that can seamlessly switch between online and offline modes?

**Techniques** :

- Use **Service Workers** for offline caching (via Workbox).
- Cache user actions in **IndexedDB** using libraries like `idb`.
- Sync queued actions when network is restored (via `navigator.onLine` events).

```jsx
window.addEventListener("online", syncQueuedRequests);
```

- Use optimistic UI updates with rollback on sync failure.

<br />

### 4. How would you architect a frontend system to support internationalization (i18n) and localization (L10n) for a global audience?

**Architecture** :

- Abstract all strings and formats using tools like `react-i18next` or `vue-i18n`.
- Use **locale-based routing** (`/en/`, `/fr/`) for SEO.
- Dynamically load locale files to avoid bloated bundles.

```jsx
i18n.changeLanguage("fr"); // dynamically switches language
```

<br />

### 5. What’s your strategy for designing a frontend system that integrates with a microservices backend?

**Strategy** :

- Use **API gateway** on backend to abstract microservices from frontend.
- Normalize and transform microservice responses in a client-side **API layer** .
- Handle versioning, retries, and fallbacks centrally.

<br />

### 6. How do you design a React application to minimize bundle size and optimize initial load time for a large-scale app?

**Tactics** :

- **Code-splitting** with React.lazy + Suspense.
- Use **tree-shaking-friendly** libraries.
- Load non-critical JS with `async`/`defer`.
- Use **dynamic imports** and **preload** critical assets.
- Compress with Brotli and serve via CDN.

<br />

### 7. How would you design a frontend system to handle pagination, infinite scrolling, and large datasets (e.g., 100,000+ records)?

**Best Practices** :

- Use **cursor-based pagination** from the backend.
- For infinite scroll, use `IntersectionObserver` and **windowed rendering** (e.g., `react-window`, `vue-virtual-scroll-list`).

```jsx
<FixedSizeList height={600} itemSize={35} itemCount={items.length}>
  {Row}
</FixedSizeList>
```

- Avoid loading 100k records into memory.

<br />

### 8. What’s your approach to designing a frontend caching strategy to reduce redundant API calls in a React application?

**Layers of caching** :

- **In-memory** with React Query / SWR (`stale-while-revalidate` pattern).
- **Persistent** caching via IndexedDB or localStorage for static data.
- Use cache keys + invalidation on mutation.

<br />

### 9. How do you design a frontend system to handle peak traffic spikes (e.g., Black Friday sales) without degrading performance?

- SSR with full-page CDN caching.
- Minimize dynamic JS — use **islands architecture** or **partial hydration** .
- Show graceful loading states, use fallback UI for rate-limited APIs.
- Use feature flags to **disable non-critical features** during load spikes.

<br />

### 10. How would you architect a React application to support progressive web app (PWA) features like push notifications and offline caching?

**Architecture** :

- Use `create-react-app` or Next.js PWA plugin with **Service Workers** .
- Push notifications via **Web Push API** + server with VAPID.
- Cache routes/assets via Workbox strategies (`cacheFirst`, `networkFirst`).

```jsx
self.addEventListener('push', event => {
  self.registration.showNotification('New Offer!', {...});
});
```

<br />

### 11. How do you design a state management system for a React app with complex, interdependent UI components?

**Approach** :

- Use **Zustand** , **Redux Toolkit** , or **Pinia** (Vue).
- Split global and local state.
- Normalize relational data to avoid nesting and circular dependencies.
- Use derived selectors and memoization for performance.

<br />

### 12. What’s your approach to designing a frontend system that syncs data bidirectionally with a backend in real time (e.g., collaborative editing)?

**Pattern** :

- Use WebSockets + OT (Operational Transform) or CRDT (e.g., Yjs).
- Local changes are applied optimistically.
- Server resolves conflicts and broadcasts updates.

```
editor.onChange = (change) => socket.emit('doc:update', change);
```

- Maintain versioning or timestamps to prevent collisions.

<br />

### 13. How would you design a frontend system to manage client-side data persistence across page refreshes and sessions?

**Tools** :

- Use `localStorage`/`sessionStorage` for light data (e.g., theme, tokens).
- Use **IndexedDB** for complex or structured data (e.g., offline orders).
- Sync to memory cache on load.

```livescript
localStorage.setItem('theme', 'dark');
```

<br />

### 14. How do you design a frontend system to handle rate-limited or unreliable APIs without compromising user experience?

**Strategies** :

- Queue requests and **retry with backoff** .
- Use **circuit breaker pattern** —disable problematic features temporarily.
- Graceful fallbacks: cached data, skeleton UI, offline mode.

```jsx
axiosRetry(api, { retries: 3, retryDelay: exponentialDelay });
```

<br />

### 15. What’s your approach to designing a frontend system that supports optimistic UI updates while ensuring data integrity with the backend?

**Flow** :

1. **Immediately update UI** state (e.g., like a post).
2. Send async request to backend.
3. On error, **rollback** to previous state.

```jsx
mutation.mutate(updateItem, {
  onMutate: () => updateCacheLocally(),
  onError: () => rollback(),
});
```

Use `react-query` or a custom mutation manager for tracking.

<br />

### 16. How do you design a frontend system to work with a GraphQL backend versus a RESTful backend?

**Key differences** :

- **GraphQL** offers flexible queries and fewer round trips.
- **REST** requires multiple endpoints and often over-fetching.

  **GraphQL Strategy** :

- Use a GraphQL client like **Apollo Client** or **urql** .
- Define queries colocated with components.
- Leverage built-in caching and fragments for modularity.

```tsx
const { data, loading } = useQuery(GET_PRODUCT, { variables: { id } });
```

**REST Strategy** :

- Use a centralized API layer with **Axios** or **fetch** .
- Normalize responses with tools like **normalizr** .
- Handle pagination/versioning manually.

<br />

### 17. How would you architect a frontend system to integrate with a legacy backend that has inconsistent APIs and slow response times?

**Approach** :

- Introduce a **client-side adapter layer** to normalize data and abstract quirks.
- Use **API gateways** or **BFF (Backend-for-Frontend)** when possible to decouple frontend from backend inconsistencies.
- Implement **timeouts** , **caching** , **loading skeletons** , and **graceful error handling** for slow APIs.

```jsx
async function getNormalizedUser(id) {
  const raw = await legacyAPI.get(`/user/${id}`);
  return { name: raw.username, age: raw.details?.age ?? null };
}
```

<br />

### 18. What’s your approach to designing a frontend system that supports A/B testing and feature toggles at scale?

**System Design** :

- Use a **feature flag provider** (e.g., LaunchDarkly, Unleash, or homegrown).
- Flags should be resolved **before rendering** to prevent flickering (SSR or early fetch).
- For A/B tests, integrate analytics to capture variant behavior.

```jsx
{
  featureFlags.newCheckout ? <NewCheckout /> : <LegacyCheckout />;
}
```

- Flags should be typed, scoped, and cleanly removed after experiments.

<br />

### 19. How do you design a frontend system to collaborate with a backend team using a contract-first approach (e.g., OpenAPI, GraphQL schemas)?

**Best Practices** :

- Use **auto-generated types** from contracts (e.g., `graphql-codegen`, `openapi-typescript`).
- Validate requests/responses via schemas (e.g., Zod or Yup).
- Share **mock servers** or use **MSW** for contract testing.

```jsx
// types generated from GraphQL schema
type GetUserQuery = {
  user: { id: string, name: string },
};
```

- Enforce contract evolution rules (e.g., additive changes only) to avoid breaking consumers.

<br />

### 20. How would you design a frontend system to support multiple deployment environments (e.g., dev, staging, prod) with different configurations?

**Approach** :

- Externalize config using **environment variables** (e.g., `.env.*`).
- Use `process.env` (React) or `import.meta.env` (Vite) to inject at build time.

```
const apiBase = process.env.REACT_APP_API_URL;
```

- Ensure sensitive values are only injected at build and not leaked to client.
- Automate deploys with CI/CD pipelines per environment.

<br />

### 21. How do you design a frontend system to gracefully handle browser compatibility issues across a wide range of devices and versions?

**Techniques** :

- Use **autoprefixer** + **Babel polyfills** (via `@babel/preset-env`).
- Detect features with **Modernizr** or via runtime checks (`'IntersectionObserver' in window`).
- Serve different bundles using **differential loading** or **[polyfill.io](http://polyfill.io)** .

```jsx
if (!window.fetch) {
  loadPolyfill().then(initApp);
}
```

- Test with tools like BrowserStack or Playwright.

<br />

### 22. What’s your approach to designing a frontend system with zero-downtime deployments?

**Practices** :

- Use **immutable builds** with content hash (e.g., `main.abc123.js`) and **atomic deploys** .
- Avoid breaking changes to APIs or storage between versions.
- Use **feature flags** to decouple deployment from release.

```html
<script src="/static/js/main.[hash].js" defer></script>
```

- Use Service Workers (PWA) carefully to avoid stale caches (version them explicitly).

<br />

### 23. How do you design a frontend monitoring system to track performance, errors, and user behavior in production?

**Tooling** :

- Errors: **Sentry** , **Rollbar** for JS exceptions and stack traces.
- Performance: **Web Vitals** , custom metrics via **Google Analytics** , **Datadog RUM** , or **New Relic** .
- Behavior: **FullStory** , **PostHog** , or custom event tracking.

```jsx
reportWebVitals((metric) => sendToAnalytics(metric));
```

- Group and alert on errors by component/version, and use source maps in production.

<br />

### 24. How would you architect a frontend system to support long-term maintenance by a team of varying skill levels?

**Strategies** :

- Enforce **coding standards** with ESLint, Prettier, and strict TypeScript.
- Use **design systems** and component libraries (e.g., Storybook).
- Provide clear folder structure, README docs, and linted commit hooks.

```json
"scripts": {
  "lint": "eslint src --ext .js,.ts,.tsx",
  "typecheck": "tsc --noEmit"
}
```

- Emphasize **code reviews** , mentorship, and writing tests (unit + E2E).

<br />

### 25. What’s your approach to designing a frontend system that can recover from catastrophic failures (e.g., corrupted state, network outages)?

**Defense Strategy** :

- Wrap stateful logic in **error boundaries** to isolate crashes.

```tsx
<ErrorBoundary fallback={<Fallback />}>
  <Dashboard />
</ErrorBoundary>
```

- Store critical state in **resilient storage** (e.g., IndexedDB with schema validation).
- Queue offline actions and retry with exponential backoff.
- Fallback to cached UI where possible (Service Worker, localStorage snapshot).
