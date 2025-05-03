### 1. How do you define and enforce a performance budget for a React application?

**Answer:**

A performance budget is a set limit on key performance metrics. I define it early based on business needs and target devices using metrics like:

- **First Contentful Paint (FCP)** < 2s
- **Time to Interactive (TTI)** < 5s
- **Total JS size** < 170KB (gzipped)
- **Image size per route** < 300KB

**Enforcement tools:**

- `lighthouse-ci` for metric thresholds
- Webpack’s `performance` config:

```

performance: {
  maxEntrypointSize: 250000,
  maxAssetSize: 250000,
},

```

<br />

### 2. What’s your approach to auditing a React app against a performance budget during development?

**Answer:**

I use **automated and manual tools** in dev workflows:

- **Lighthouse** (manual or CI via `lighthouse-ci`)
- **Webpack Bundle Analyzer** to audit JS size
- **React Profiler** to identify unnecessary re-renders
- **Chrome DevTools** for real-world TTI, layout shifts

Integrate metrics into PR reviews to ensure accountability.

<br />

### 3. How do you balance feature development with maintaining a performance budget in a React project?

**Answer:**

Balance through **guardrails and priorities** :

- Set **budget alerts in CI** so performance regressions surface early
- Encourage **code splitting, lazy loading** , and **incremental hydration** for new features
- Involve product in **performance trade-offs** during planning

I advocate for performance as a **non-functional requirement** , not an afterthought.

<br />

### 4. What’s your strategy for optimizing third-party scripts within a performance budget?

**Answer:**

1. **Audit necessity** of each script (use only what adds value).
2. Load scripts **asynchronously** or **defer** :

```html
<script src="chat.js" async></script>
```

1. Move to **server-side integrations** (e.g., analytics proxy) when possible.
2. Use **Web Workers** to isolate heavy work off the main thread.
3. Leverage **Subresource Integrity (SRI)** and **preconnect** for faster delivery.

<br />

### 5. How do you integrate performance budget checks into a CI/CD pipeline for a React app?

**Answer:**

- Use **`lighthouse-ci`** in CI to fail builds when budgets are breached:

```yaml
lighthouse-ci:
  script: lhci autorun
```

- Add **bundle size checks** with tools like `size-limit`:

```json

"scripts": {
  "size-check": "size-limit"
}

```

- Integrate **Web Vitals tracking** (via `@vercel/web-vitals`) into production for post-deploy insights.

<br />

### 6. What’s your approach to optimizing images and assets in a React app to meet a performance budget?

**Answer:**

- Serve images in **modern formats** like WebP or AVIF.
- Use **responsive `srcset`** for varying device resolutions.
- Lazy-load images with `loading="lazy"` or `react-lazyload`.
- Use tools like `image-webpack-loader`:

```

{
  loader: 'image-webpack-loader',
  options: { mozjpeg: { progressive: true } }
}

```

- For static sites, tools like **Next.js Image Component** handle most of this automatically.

<br />

### 7. How do you measure and improve Time to Interactive (TTI) in a React application?

**Answer:**

- Measure with **Lighthouse** , **WebPageTest** , and **Real User Monitoring (RUM)** .
- Improve via:
  - **Code splitting** with React.lazy/Suspense
  - **Defer non-critical JS**
  - **Prioritize above-the-fold content**
  - Use **server-side rendering (SSR)** + **hydration delay** techniques

```

const HeavyComponent = React.lazy(() => import('./Heavy'));

```

<br />

### 8. What’s your process for identifying and eliminating render-blocking resources in a React app?

**Answer:**

1. **Audit with Lighthouse/DevTools** for blocked critical paths.
2. Defer or preload fonts and styles:

```html
<link rel="preload" as="style" href="styles.css" />
```

1. Minimize and inline **critical CSS** .
2. Load **non-critical JS** with `defer` or `async`.

Webpack plugins like `mini-css-extract-plugin` + critical CSS extractors help automate this.

<br />

### 9. How do you educate a team on adhering to a performance budget in a React codebase?

**Answer:**

- Create a **shared performance playbook** (tools, budgets, best practices).
- Embed **Lighthouse reports and bundle size** in PR checks.
- Run **performance pairing sessions** and **code walkthroughs** .
- Highlight regressions in retros.
- Showcase wins (e.g., TTI improvements) to build a performance-first culture.

<br />

### 10. What’s your approach to maintaining a performance budget as a React app scales over time?

- Treat performance as a **living contract** , revisiting budgets quarterly.
- Use **RUM tools** (e.g., SpeedCurve, NewRelic) to monitor real-world performance.
- Version and **track regressions** via CI dashboards.
- Set rules for **tech debt sprints** and periodic **bundle audits** .
- Keep a dedicated **performance backlog** to fix budget drift.
