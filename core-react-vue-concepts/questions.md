### **What is the Virtual DOM, and how does React use it to improve performance?**

The Virtual DOM (VDOM) is a lightweight, in-memory representation of the real DOM. React uses it to optimize updates and rendering.

When state or props change:

- React renders a new VDOM.
- It diffs the new VDOM with the previous one (using a **diffing algorithm** ).
- It calculates the minimal set of changes (patches).
- Then it updates the real DOM efficiently in a **batched** way.

This reduces direct DOM manipulations, which are expensive, improving performance.

### Explain the difference between functional components and class components in React.

| Aspect        | Class Component           | Functional Component |
| ------------- | ------------------------- | -------------------- |
| Syntax        | ES6 classes               | Plain JS functions   |
| State         | `this.state`              | `useState`Hook       |
| Lifecycle     | `componentDidMount`, etc. | `useEffect`          |
| `this`context | Required                  | Not used             |

### What are React Hooks? Explain `useState`, `useEffect`, and `useContext` with examples.

Hooks let functional components manage **state** , side effects, and context — features previously only available in class components.

- **`useState`** : Manages local state.
- **`useEffect`** : Handles side effects (data fetching, subscriptions).
- **`useContext`** : Accesses context values.

### How does React handle component lifecycle methods in functional components?

Functional components use **Hooks** to mimic lifecycle behavior:

| Class Lifecycle        | Hook Equivalent                     |
| ---------------------- | ----------------------------------- |
| `componentDidMount`    | `useEffect(..., [])`                |
| `componentDidUpdate`   | `useEffect(..., [deps])`            |
| `componentWillUnmount` | `return () => {}`inside `useEffect` |

### What is the significance of keys in React lists, and what happens if you don’t use them correctly?

**Keys** help React identify which items in a list have changed, been added, or removed. They must be **unique and stable** .

If you use indexes as keys or omit them:

- React may incorrectly reuse components.
- It can cause rendering bugs or poor performance.

### What are the key differences between React.js and Vue.js in terms of component architecture and reactivity?

| Feature         | React                           | Vue                           |
| --------------- | ------------------------------- | ----------------------------- |
| Language        | JSX + JS                        | HTML templates + JS           |
| Reactivity      | Immutable state, manual updates | Reactive refs and proxies     |
| Component Model | Functional-first with Hooks     | Options API / Composition API |
| Data Binding    | One-way                         | Two-way via `v-model`         |
| State Mgmt      | Context, Redux, Zustand         | Vuex, Pinia                   |

React is more **unopinionated** (freedom, flexibility), while Vue offers a more **batteries-included** developer experience.

### How do you decide whether to use React.js or Vue.js for a new project? What factors influence your choice?

**Factors:**

- **Team experience** (React is more common in enterprise).
- **Ecosystem needs** (e.g., Next.js vs Nuxt).
- **Project complexity** (Vue’s simplicity suits small-to-mid apps).
- **Tooling preference** (React: flexible, Vue: more integrated).
- **Community support** and **job market** (React has broader adoption).

**Decision rule of thumb:**

- For large-scale, long-term, customizable UIs → React.
- For fast-to-market apps or teams with less JS depth → Vue.

### How would you migrate a legacy React application from class components to functional components with Hooks?

**Step-by-step approach:**

1. Start with isolated, low-risk components.
2. Replace `state` with `useState`.
3. Replace lifecycle methods with `useEffect`.
4. Handle bindings by removing `this`.
5. Migrate context consumers using `useContext`.

### What are Higher-Order Components (HOCs)? Provide an example of when you’ve used one.

An HOC is a **function that takes a component and returns a new component** with enhanced behavior. It's a pattern for code reuse.

**Use Case:**
I’ve used HOCs to:

- Inject global logic (e.g., logging, error boundaries).
- Add authentication checks to routes.
- Connect components to Redux (e.g., `connect()`).

### How do you handle complex component composition in React.js or Vue.js to avoid tightly coupled code?

**Strategies:**

- Use **compound components** for shared context and slot-like behavior.
- Apply **render props** or **custom Hooks** for logic reuse.
- Split UI and logic (smart/dumb component pattern).
- Avoid prop drilling via **context** or **provide/inject** in Vue.

### What is React Reconciliation, and how does it work under the hood?

Reconciliation is the process React uses to update the DOM when a component’s state or props change.

Steps:

1. A state change triggers a **new VDOM tree** .
2. React **diffs** it with the previous tree using a **heuristic algorithm** (O(n)).
3. It identifies minimal changes and updates the real DOM.

**Key optimizations:**

- Uses **keys** to detect list changes.
- Assumes components with the same type will produce similar output.
- Uses **Fiber** architecture for async rendering and prioritization.

This makes updates predictable and performant, especially in large UIs.
