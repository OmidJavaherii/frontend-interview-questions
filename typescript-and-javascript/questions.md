### How does the JavaScript event loop work, and how does it impact React’s rendering and state updates?

The event loop allows JavaScript (single-threaded) to handle **asynchronous operations** using a **call stack** , **task queue (macrotasks)** , and **microtask queue** .

In React:

- `setState` is **batched and asynchronous** .
- State updates and rendering happen **after the current call stack is cleared** (queued as microtasks or macrotasks depending on context).
- Understanding this helps prevent race conditions and ensures UI updates occur predictably.

### What’s your mental model for understanding JavaScript closures, and how do you use them effectively in a React application?

A closure is a function that **retains access to its lexical scope** , even when invoked outside that scope.

In React:

- Useful for **memoized callbacks** or managing **encapsulated state** in custom hooks.

Closures must be handled carefully in effects to avoid **stale state** issues.

### How do you explain prototypal inheritance in JavaScript, and how does it differ from classical inheritance?

Prototypal inheritance links objects to other objects via the **`[[Prototype]]` chain** , enabling property sharing.

Unlike classical inheritance:

- JS favors **object delegation over class hierarchies** .
- It's more **dynamic and flexible** , which aligns well with **composition patterns** in React.

### What’s the significance of the `this` keyword in JavaScript, and how do you manage its binding in React components?

`this` refers to the **execution context** . In React class components, `this` often refers to the **component instance** .

In class components:

`this.handleClick = this.handleClick.bind(this);`

In functional components:

- Avoid `this` entirely by using hooks and arrow functions.
- Arrow functions **lexically bind** `this`.

### How does JavaScript’s single-threaded nature influence your approach to handling computationally expensive tasks in a React app?

Since JS runs on a **single thread** , heavy tasks **block the UI** .

Strategies:

- **Web Workers** for offloading CPU-intensive work.
- **Debounce/throttle** inputs.
- Use **`requestIdleCallback`** or `setTimeout` for deferring work.
- Keep components **pure and fast-rendering** .

### How do you design a robust async/await workflow in a React application for handling multiple API calls?

Use `async/await` inside `useEffect`, encapsulated in `try/catch`, and optionally use `Promise.all`

### What’s the difference between promises and async/await under the hood, and how do you optimize their usage in React?

- **Promises** represent async values; `then` chains callbacks.
- `async/await` is **syntactic sugar** over Promises, improving readability and debugging.

Optimizations:

- Avoid nested `await` inside loops (use `Promise.all`).
- Memoize async functions with `useCallback`.
- Always **handle cleanup** in async effects to avoid race conditions.

### How do you implement a custom Promise-based utility (e.g., a retry mechanism) for a React app?

```jsx
const retry = (fn, retries = 3, delay = 1000) =>
  fn().catch((err) => {
    if (retries === 0) throw err;
    return new Promise((res) => setTimeout(res, delay)).then(() =>
      retry(fn, retries - 1, delay)
    );
  });
```

### What’s your approach to managing microtasks and macrotasks in a React application with heavy async operations?

- **Microtasks** : Promises, `queueMicrotask`.
- **Macrotasks** : `setTimeout`, `fetch`.

Use **microtasks for chaining logic** and **macrotasks for deferring** execution.

### How do you use the `async` iterator protocol (e.g., `for await...of`) in a React app for streaming data?

Used for **streaming APIs** (e.g., SSE, file chunks):

```
async function streamData(url) {
  const response = await fetch(url);
  for await (const chunk of response.body) {
    processChunk(chunk);
  }
}
```

In React:

- Run inside `useEffect`.
- Useful for **real-time feeds or long downloads** .

### How do you leverage ES6 modules in a large React codebase to ensure maintainability and tree-shaking?

- Use **named exports** for better tree-shaking.
- Structure features as **isolated folders** with an `index.ts` for barrel exports.
- Avoid default exports in shared libs to **help static analysis** .

### What’s your understanding of JavaScript scope and hoisting, and how do you prevent common pitfalls in React?

**Answer:**

- **Scope** : Lexical—determined at write time.
- **Hoisting** : `var`, function declarations are hoisted.

In React:

- Use `let`/`const` to avoid hoisting bugs.
- Avoid relying on hoisted variables in closures or hooks.
- Keep variable declarations **within the hook/component scope** .

### How do you design a JavaScript utility library for a React team, balancing encapsulation and reusability?

**Answer:**

- Use **pure functions** .
- Organize by **domain** (e.g., `formatters/`, `validators/`).
- Write clear **TypeScript types** and JSDoc.
- Avoid internal state or side effects.
- Provide **default configurations** but allow overrides.

### How do you apply functional programming principles (e.g., immutability, pure functions) in a React application?

- Use `map`, `filter`, and `reduce` for data transformation.
- Avoid mutating state (e.g., spread operators).
- Extract **pure helpers** for selectors and business logic.
- Libraries: Lodash/fp, Ramda (when needed).

### What’s your approach to memoizing expensive computations in JavaScript for a React app?

Use `useMemo`:

`const filtered = useMemo(() => heavyFilter(data), [data]);`

Avoid premature optimization; profile first. Ensure dependencies are correct to avoid stale or recomputed values.

### How do you optimize JavaScript performance in a React app by minimizing object mutations?

- Use **immutable updates** (e.g., spread operators, `immer`).
- Avoid in-place sorting or pushing to arrays.
- Helps React detect changes via **shallow comparison** (`React.memo`, `PureComponent`).

### How do you use JavaScript Proxies or Reflect in a React application for dynamic state management or debugging?

Use `Proxy` to track mutations or build reactive-like wrappers:

```jsx
const state = new Proxy(
  {},
  {
    set(target, key, value) {
      console.log(`${key} changed to ${value}`);
      target[key] = value;
      return true;
    },
  }
);
```

Good for **debugging** , **form libraries** , or **custom observables** .

### What’s your understanding of JavaScript’s garbage collection, and how do you prevent memory leaks in a React app?

JS uses **mark-and-sweep GC** . Memory leaks happen when objects remain referenced unintentionally.

In React:

- **Clean up** in `useEffect`.
- Avoid stale refs or retaining DOM nodes in closures.
- Monitor leaks using Chrome DevTools Memory tab.

### How do you handle JavaScript’s floating-point precision issues in a React app (e.g., financial calculations)?

Avoid native floats for money. Use:

- **`Intl.NumberFormat`** for display.
- **Big.js** , **decimal.js** , or `BigInt` for arithmetic.

### What’s your approach to polyfilling or shimming modern JavaScript features for older browsers in a React app?

- Use **Babel + core-js** for transpilation/polyfills.
- Use **browserslist** config to target supported browsers.
- For critical features (e.g., `fetch`, `Promise`), use conditional polyfills or dynamic imports.
