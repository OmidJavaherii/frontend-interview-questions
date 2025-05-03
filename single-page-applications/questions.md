### 1. Why were Single Page Applications (SPAs) developed, and what problems were they intended to solve compared to traditional multi-page applications (MPAs)?

SPAs reduce full-page reloads by dynamically updating the UI using client-side JavaScript. This solves latency and UX issues in MPAs where each navigation required a round trip to the server and full DOM re-render.

<br />

### 2. What historical context (e.g., browser capabilities, JavaScript advancements) led to the rise of SPAs in web development?

The rise of SPAs coincided with:

- Advancements in AJAX (XMLHttpRequest, later `fetch`)
- Modern JS engines (V8, Chakra) that made heavy client-side logic feasible
- The push for more responsive, desktop-like UX in browsers (e.g., Gmail)

<br />

### 3. How do you explain the trade-offs that drove the adoption of SPAs over MPAs to a non-technical stakeholder?

SPAs feel faster because only parts of the page update. MPAs reload the entire page, which can feel clunky. SPAs reduce wait time after the initial load but require more effort for SEO and performance.

<br />

### 4. What are the key advantages of SPAs that justified their development, and how do they align with modern web app requirements?

- Seamless UX (no reloads)
- Better perceived performance
- Easier to create responsive, mobile-like experiences
- Decoupled front/backend development

<br />

### 5. How did the limitations of traditional server-rendered MPAs (e.g., page reloads, latency) influence the design philosophy behind SPAs?

MPAs suffered from latency (full-page loads), unnecessary rendering, and janky UX. SPAs shifted rendering responsibility to the browser to offer near-instant transitions and smoother interactions.

<br />

### 6. What’s your perspective on why SPAs became the default choice for frameworks like React, Angular, and Vue.js?

These frameworks were built to solve UI complexity by enabling reactive, state-driven rendering—perfect for SPAs. They offered routing, data fetching, and component models that naturally aligned with single-page experiences.

<br />

### 7. How do you assess whether an SPA is the right architectural choice for a given project versus an MPA?

Use an SPA when:

- You need high interactivity (dashboards, apps)
- Performance post-initial-load matters
- You're building a mobile/desktop-like experience
  Avoid SPAs for content-heavy or SEO-critical websites unless SSR is in place.

<br />

### 8. What challenges did early SPA implementations face, and how have modern tools addressed those issues?

- Poor SEO (no server-rendered HTML)
- Long initial load times
- Browser history issues
- Security (exposing APIs to client)
  Modern tools like Next.js, SSR, hydration, and lazy loading solve most of these.

<br />

### 9. How do you educate a team on the purpose and benefits of SPAs when transitioning from an MPA codebase?

- Explain SPA lifecycle and routing
- Introduce concepts like hydration, lazy loading
- Emphasize client-side state and component-driven design
- Walk through real-world examples (e.g., React Router)

<br />

### 10. What’s your take on the evolution of SPAs since their inception, and where do you see the paradigm heading in the future?

SPAs are evolving into **hybrid apps** with:

- SSR/SSG (e.g., Next.js)
- Islands architecture
- Partial hydration
  Future direction emphasizes performance, SEO, and developer experience.

<br />

### 11. How does routing work in a Single Page Application, and how does it differ from traditional server-side routing?

- SPA routing is **client-side** via the History API (`pushState`)
- MPA routing is **server-driven** via HTTP requests
  SPA routers intercept link clicks and render views dynamically without reloading.

<br />

### 12. What’s your approach to implementing client-side routing in a React SPA using React Router?

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";

<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/dashboard" element={<Dashboard />} />
  </Routes>
</BrowserRouter>;
```

<br />

### 13. How do you manage state in a React SPA when navigating between routes?

Use:

- Global state (e.g., Zustand, Redux)
- URL query params for route-specific state
- `localStorage` or `sessionStorage` for persistence

<br />

### 14. What are the challenges of handling browser history and back/forward navigation in an SPA?

- Managing `back/forward` without reloads
- Restoring scroll positions
- Deep linking & state persistence

React Router handles much of this via `<BrowserRouter>` and `useNavigate`.

<br />

### 15. How do you optimize SPA routing performance to minimize delays during navigation?

- **Code splitting** via `React.lazy` and `Suspense`
- **Prefetching** data or chunks
- Use `memo`, `useMemo`, and `useCallback` to avoid unnecessary renders

<br />

### 16. What’s your strategy for handling 404s or invalid routes in a React SPA?

`<Route path="*" element={<NotFound />} />`
Handle invalid routes with fallback components and optionally redirect to safe pages.

<br />

### 17. How do you integrate server-side rendering (SSR) with SPA routing in a React app to improve SEO?

Use frameworks like **Next.js** :

- Server-render pages with `getServerSideProps`
- Improve SEO with meaningful `<head>` tags and pre-rendered content

<br />

### 18. What’s your approach to securing routes in a React SPA (e.g., protected routes requiring authentication)?

Use a protected route pattern:

```jsx
const PrivateRoute = ({ children }) =>
  isAuthenticated ? children : <Navigate to="/login" />;
```

Integrate token checks, role-based access, and redirect unauthorized users.

<br />

### 19. How do you test SPA routing logic in a React application to ensure reliability across navigation scenarios?

Use:

- **React Testing Library** to simulate navigation
- **Cypress/Playwright** for E2E route transitions
- Mock route params and history

Example:

```jsx
render(
  <MemoryRouter initialEntries={["/dashboard"]}>
    <App />
  </MemoryRouter>
);
```

<br />

### 20. How do you debug routing issues in a React SPA when navigation fails or renders unexpected content?

- Use **React DevTools** + browser DevTools (network, console)
- Inspect `window.history` state
- Validate route configuration
- Check dynamic imports and lazy components for fallback/render issues
