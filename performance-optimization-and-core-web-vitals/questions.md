### What techniques can you use to optimize the performance of a React application?

- **Code splitting** with `React.lazy` and dynamic imports.
- **Memoization** using `React.memo`, `useMemo`, and `useCallback` to avoid unnecessary computations and re-renders.
- **Virtualization** with `react-window` or `react-virtualized` for large lists.
- **Debouncing/throttling** expensive updates (e.g., search inputs).
- **Selective rendering** using `shouldComponentUpdate`, `React.PureComponent`, or proper `useEffect` dependencies.
- **Image optimization** , lazy-loading assets, and serving compressed bundles.

### Explain the purpose of `React.memo` and when you would use it.

`React.memo` is a HOC that **prevents unnecessary re-renders** of a component if its props haven’t changed.

**Use case:** Functional components that are **pure** and receive the same props repeatedly.

It's especially useful in **list rendering** or nested components where the parent re-renders frequently.

### How do `useCallback` and `useMemo` hooks work, and how do they help with performance?

- `useCallback(fn, deps)` returns a **memoized version of a callback** .
- `useMemo(fn, deps)` returns a **memoized result of a computation** .

**Why it matters:** Avoids **unnecessary recreation** of functions or values, which helps prevent re-renders or re-calculations, especially when passing props to child components or dependencies to `useEffect`.

### What is code splitting in React, and how can you implement it using lazy loading?

Code splitting breaks your app into **smaller chunks** that load **on demand** , improving initial load time.

This loads `Profile` only when needed. Use `React.lazy` with `Suspense` for route-based or component-level code splitting.

### How would you debug and resolve a performance issue caused by excessive re-rendering in a React app?

Steps:

1. **Enable React DevTools** → highlight re-renders.
2. Use the **Profiler tab** to find frequently updated components.
3. Check props/state dependencies and **memoize components** (`React.memo`) or callbacks (`useCallback`).
4. Ensure `useEffect` and context consumers aren’t triggering unnecessary updates.
5. Apply **shouldComponentUpdate** logic or `useMemo` to prevent re-renders.

### How do you measure and improve Core Web Vitals (LCP, FID, CLS) in a React.js application?

- **Measure** using Web Vitals library, Lighthouse, or tools like PageSpeed Insights.
- **LCP** : Optimize images, server-side render above-the-fold content.
- **FID** : Minimize JavaScript blocking time, defer non-critical JS.
- **CLS** : Use proper image/video dimensions, avoid layout-shifting components.

### Describe a time when you identified and fixed a performance bottleneck in a frontend application.

In a data-heavy dashboard, charts re-rendered on every state change due to **new props** passed down each time. I:

- Wrapped chart components in `React.memo`.
- Used `useMemo` to compute props.
- Batched state updates using `useTransition`.

This reduced render time from ~800ms to ~100ms and eliminated UI lag.

### How would you optimize a React app that suffers from slow initial load times due to a large bundle size?

- **Code split** by route and component (`React.lazy`).
- **Tree-shake** unused libraries (e.g., import only needed functions from lodash).
- Use **dynamic imports** for heavy components (e.g., charts, maps).
- **Defer third-party scripts** like analytics.
- Use **webpack analyzer** to inspect bundle size and eliminate bloat.

### How do you profile a React application to identify unnecessary renders or memory leaks?

- Use **React DevTools Profiler** :
  - Record interactions.
  - Look for "why did this render?" via React 18's insights.
- Use Chrome DevTools → Memory tab:
  - Look for **Detached DOM trees** and retained objects.
- Add logs or `console.count` inside components to monitor re-renders.

### You notice a memory leak in a React application. How would you identify and fix it?

**Identification:**

- Use Chrome DevTools → Memory → Take snapshots.
- Look for retained event listeners, timers, or closures.
- Check for unmounted components still in memory.

**Fixes:**

- **Clean up side effects** in `useEffect`:
- Avoid retaining state in **global singletons or closures** .
- Ensure refs and DOM nodes are released properly.
