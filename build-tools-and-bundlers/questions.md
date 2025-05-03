### Compare Webpack, Vite, and ESBuild in terms of speed, configuration, and use cases. Which would you choose for a React project and why?

| Feature       | Webpack                                         | Vite                                               | ESBuild                                   |
| ------------- | ----------------------------------------------- | -------------------------------------------------- | ----------------------------------------- |
| **Speed**     | Slower (especially on rebuilds)                 | Lightning-fast dev server via native ESM + ESBuild | Fastest for bundling, minimal feature set |
| **Config**    | Highly configurable, verbose                    | Minimal config (sensible defaults)                 | Lightweight, but limited plugins          |
| **Use Cases** | Large, mature apps requiring deep customization | Modern SPAs needing fast DX                        | Tools, libraries, or pre-bundlers         |

**Which for React?**

> For modern React projects, Vite is my preferred choice. It offers blazing-fast startup using native ESM, integrates smoothly with React + TypeScript, and significantly improves DX over Webpack. For large legacy apps, Webpack still has broader plugin ecosystem and fine-grained control.

### How do you configure a custom Webpack setup for a React app with TypeScript and CSS preprocessing?

You’ll need:

- `webpack.config.js`
- Babel for JSX/TS transpilation
- Loaders for `.tsx`, `.css`, and preprocessors

```jsx
// webpack.config.js
module.exports = {
  entry: "./src/index.tsx",
  output: { filename: "bundle.js", path: __dirname + "/dist" },
  resolve: { extensions: [".ts", ".tsx", ".js"] },
  module: {
    rules: [
      { test: /\\.tsx?$/, use: "babel-loader", exclude: /node_modules/ },
      { test: /\\.s?css$/, use: ["style-loader", "css-loader", "sass-loader"] },
    ],
  },
  devServer: { historyApiFallback: true },
};
```

Install `babel-loader`, `@babel/preset-react`, `@babel/preset-typescript`, and configure `.babelrc`.

This setup supports React + TypeScript and SCSS, ideal for scalable frontend architectures.

### What are the benefits of using Vite over Webpack in a modern React project?

- **Faster startup** : No bundling during dev. Uses native ESM + ESBuild for instant HMR.
- **Simplified config** : Minimal boilerplate. React plugin via `@vitejs/plugin-react`.
- **Out-of-the-box support** : JSX, TypeScript, CSS Modules, PostCSS.
- **Production builds** : Still uses Rollup under the hood for optimal tree-shaking.

### How do you optimize a build process to reduce deployment time in a CI/CD pipeline?

- **Enable caching** :
- Use `webpack.cache` (in memory or filesystem).
- Leverage CI caching (e.g., `node_modules`, `dist`, `turbo`, `vite` caches).
- **Parallelization** :
- Use multi-threaded loaders (`thread-loader`, Vite does this by default).
- Split builds into smaller jobs (e.g., client vs. server).
- **Tree-shaking + Code splitting** :
- Remove dead code.
- Dynamically import routes/components.
- **Use lightweight tools** :
- Switch to **Vite** or **ESBuild** for faster dev and smaller output.
- **Prune large dependencies** :
- Audit bundle size with `webpack-bundle-analyzer` or `source-map-explorer`.
- **Incremental builds** :
- Tools like Nx or Turborepo allow caching and only rebuilding changed packages.

### Describe a time when you had to debug a bundler-related issue in a React application.

In a previous project, a React app using Webpack started throwing runtime errors in production — despite working in dev. After investigation, I found:

- The error was due to **tree-shaking a side-effectful module** that wasn’t marked properly.
- A third-party lib had a side effect during import, which was dropped in the minified build.

**Steps I took:**

1. Enabled `sideEffects: false` in `package.json` selectively.
2. Used `webpack-bundle-analyzer` to visualize the missing module.
3. Added `/*#__PURE__*/` hints for custom utility modules to aid tree-shaking safely.
4. Upgraded Webpack config to ensure deterministic chunking and source map generation for easier debugging.

**Outcome:** The production build became stable, smaller, and easier to debug moving forward.
