### 1. How do you design a micro-frontend architecture for a large React application split across multiple teams?

**Answer:**

I start by dividing the app by **domain boundaries** (e.g., auth, dashboard, checkout) and assign ownership per team. Each team builds its micro-frontend (MFE) independently, often deployed separately.

**Approaches:**

- **Module Federation (Webpack 5)** for runtime integration.
- **Single-SPA** for routing and orchestration.
- **iframes** (rarely, for legacy isolation).

**Example (Module Federation):**

```

// Host (app-shell)
import('checkout/CheckoutApp');

// checkout webpack.config.js
new ModuleFederationPlugin({
  name: 'checkout',
  filename: 'remoteEntry.js',
  exposes: {
    './CheckoutApp': './src/CheckoutApp',
  },
});

```

<br />

### 2. What’s your approach to managing shared dependencies in a micro-frontend React ecosystem?

**Answer:**

Use **Webpack Module Federation’s `shared` config** to avoid duplication:

```

shared: {
  react: { singleton: true, requiredVersion: '^18.0.0' },
  'react-dom': { singleton: true },
}

```

Key considerations:

- **Pin versions** across repos (via `.npmrc` or a shared `package.json` base).
- Use **peerDependencies** when packaging components.
- Align dependency resolution during CI to catch conflicts early.

<br />

### 3. How do you handle state sharing between micro-frontends in a React app?

**Answer:**
Use one of the following based on isolation needs:

- **Global event bus** (e.g., `window.dispatchEvent`) for loose coupling.
- **Shared state container** (like `zustand`, `redux`, or `valtio`) exposed via Module Federation:

```

// host consumes remote store
import useCartStore from 'cart/store';

```

- **Custom pub/sub service** or **RxJS** for cross-app communication.

Keep shared state minimal to preserve MFE independence.

<br />

### 4. What’s your strategy for routing in a micro-frontend React application?

**Answer:**
Two common strategies:

- **Single-SPA** or host-level router delegates route ownership to MFEs:

```

<Route path="/checkout/*" element={<CheckoutApp />} />

```

- **Independent routers** inside MFEs that mount under specific base paths:

```

<BrowserRouter basename="/checkout">...</BrowserRouter>

```

Use URL prefixes and consistent route structure to avoid collisions.

<br />

### 5. How do you ensure consistent UI/UX across micro-frontends built by different teams?

**Answer:**

- Build and maintain a **design system** as a shared package (Storybook + npm).
- Enforce **linting, theming, spacing, and responsive rules** via CI.
- Use **CSS-in-JS** or **CSS Modules** to avoid style leakage.

Optional: Apply **runtime theming** or design tokens to unify branding.

<br />

### 6. What’s your approach to testing a micro-frontend React app end-to-end?

**Answer:**

- Each MFE should have **unit + integration tests** (Jest, RTL).
- For **E2E** , use tools like **Cypress** or **Playwright** against the **integrated shell** .
- Mock APIs via **MSW** or test backend environments.
- Run tests **in parallel** per MFE in CI to reduce build time.

<br />

### 7. How do you deploy micro-frontends in a CI/CD pipeline for a React project?

**Answer:**

- Each MFE is deployed independently, often to a CDN or S3.
- Expose MFEs via a `remoteEntry.js` (Module Federation).
- CI/CD Steps:
  1. Lint, test, build
  2. Upload artifacts (e.g., to S3)
  3. Update host manifest or config

Use **semantic versioning** and **env-specific remotes** to support multi-env testing.

<br />

### 8. What’s your process for debugging a micro-frontend React app when issues span multiple modules?

**Answer:**

1. Reproduce the issue in an integrated dev environment.
2. Check **browser console/network** for missing or incorrect `remoteEntry.js`.
3. Use **source maps** and **host logs** for traceability.
4. Add **version tags** to each MFE during build.
5. Use **dynamic imports** with logging to verify load behavior.

Add feature flags to selectively disable MFEs for quicker isolation.

<br />

### 9. How do you handle performance optimization in a micro-frontend architecture?

**Answer:**

- Use **code splitting** within each MFE.
- Leverage **tree shaking** and shared libraries to reduce bundle size.
- Enable **HTTP/2 or preloading** for remote chunks.
- Lazy-load MFEs:

```

const CheckoutApp = React.lazy(() => import('checkout/CheckoutApp'));

```

- Cache `remoteEntry.js` using long-lived headers with cache-busting versioning.

<br />

### 10. How do you onboard a team to a micro-frontend approach in a React codebase?

**Answer:**

1. Provide **architecture documentation** and high-level diagrams.
2. Set up **template repos** with tooling (e.g., Module Federation, testing).
3. Host **onboarding sessions** and walkthroughs.
4. Introduce a **developer portal** for discovering remotes, shared modules, and design system usage.
5. Pair programming + code reviews focused on boundaries and contract-based APIs.

Keep internal APIs simple and interface-driven to minimize tight coupling.
