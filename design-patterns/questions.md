### 1. How do you implement the Singleton pattern in a React application, and when would you use it?

**Concept:** Ensures only one instance of a class or module exists.

**When:** Shared services (e.g. config, logging, API clients).

```tsx
// singleton.js
let instance;
class Logger {
  constructor() {
    if (instance) return instance;
    instance = this;
  }
  log(msg) {
    console.log(msg);
  }
}
export const logger = new Logger();
```

**In React:** Use in service layers outside the component tree.

<br />

### 2. What’s your approach to using the Factory pattern in JavaScript for creating React components dynamically?

**Concept:** Create components based on input type.

```
const componentFactory = (type) => {
  switch (type) {
    case 'text': return <TextInput />;
    case 'select': return <SelectInput />;
    default: return <DefaultInput />;
  }
};
```

**When:** Dynamic forms or dashboards with config-driven UIs.

<br />

### 3. How do you apply the Builder pattern in a React app to construct complex UI components?

**Concept:** Step-by-step construction of a UI element.

```
class CardBuilder {
  constructor() {
    this.card = {};
  }
  setTitle(title) { this.card.title = title; return this; }
  setContent(content) { this.card.content = content; return this; }
  build() {
    return <Card {...this.card} />;
  }
}

```

**When:** UI components with many optional parts (e.g. modals).

<br />

### 4. What’s the role of the Prototype pattern in JavaScript, and how can it be used in a React context?

**Concept:** Share behavior via prototypes instead of classes.

**In React:** Rare directly, but used under-the-hood in JS. Useful for extending objects (e.g. custom event systems or polyfills).

```
const proto = {
  greet() { return `Hello ${this.name}`; }
};
const user = Object.create(proto);
user.name = "Alice";
```

<br />

### 5. How do you implement the Decorator pattern in a React application to enhance component functionality?

**Concept:** Add behavior without modifying the original.

**React Example (HOC):**

```
const withLogger = (Component) => (props) => {
  console.log('Rendered with props:', props);
  return <Component {...props} />;
};
```

**When:** Cross-cutting concerns (auth, logging, metrics).

<br />

### 6. What’s your approach to using the Adapter pattern in a React app to integrate with legacy APIs or third-party libraries?

**Concept:** Translate one interface to another.

```
const legacyApi = { fetchData: () => fetch('/old-endpoint') };
const apiAdapter = {
  getData: () => legacyApi.fetchData().then(res => res.json())
};
```

**In React:** Wrap third-party APIs in standardized service modules.

<br />

### 7. How do you apply the Composite pattern in React to manage hierarchical component structures (e.g., a tree view)?

**Concept:** Treat individual and composite components uniformly.

```jsx
const TreeNode = ({ node }) => (
  <li>
    {node.label}
    {node.children && (
      <ul>
        {node.children.map((child) => (
          <TreeNode node={child} />
        ))}
      </ul>
    )}
  </li>
);
```

<br />

### 8. What’s your strategy for using the Facade pattern in a React app to simplify complex subsystem interactions?

**Concept:** Provide a unified interface to complex subsystems.

```
// apiFacade.js
export const api = {
  getUser: () => fetch('/user'),
  getSettings: () => fetch('/settings')
};
```

**React Use:** Centralized service modules for clean component logic.

<br />

### 9. How do you implement the Proxy pattern in JavaScript for a React app to control access to resources?

**Concept:** Control access to an object (e.g., caching, validation).

```
const api = new Proxy(fetch, {
  apply(target, thisArg, args) {
    console.log('Fetching:', args[0]);
    return target(...args);
  }
});
```

**When:** Logging, caching, lazy-loading.

<br />

### 10. What’s your approach to using the Module pattern in a React codebase to encapsulate logic?

**Concept:** Encapsulate logic in a self-contained unit.

```
// counterModule.js
let count = 0;
export const counter = {
  increment: () => ++count,
  get: () => count
};
```

**React Use:** Shared utility logic or stateful services.

<br />

### 11. How do you implement the Observer pattern in a React application for event-driven communication?

**Concept:** A subject notifies observers on state change.

```
class EventBus {
  observers = [];
  subscribe(fn) { this.observers.push(fn); }
  notify(data) { this.observers.forEach(fn => fn(data)); }
}
```

**React Use:** Pub/sub for state sharing across micro-frontends or tabs.


<br />

### 12. What’s your approach to applying the Strategy pattern in React to switch between algorithms or behaviors dynamically?

**Concept:** Swap algorithms/strategies at runtime.

```
const sortStrategies = {
  byName: (a, b) => a.name.localeCompare(b.name),
  byAge: (a, b) => a.age - b.age
};
const sortData = (data, strategy) => data.sort(sortStrategies[strategy]);
```

**React Use:** Dynamic UI logic (filters, layouts, validations).

<br />

### 13. How do you use the Command pattern in a React app to encapsulate actions (e.g., undo/redo functionality)?

**Concept:** Encapsulate user commands (undo/redo).

```
class Command {
  execute() {}
  undo() {}
}
class AddItemCommand extends Command {
  execute() { /* add */ }
  undo() { /* remove */ }
}
```

**React Use:** Form builders, canvas editors.

<br />

### 14. What’s your strategy for implementing the Mediator pattern in a React app to coordinate multiple components?

**Concept:** Central object coordinates communication.

```
const mediator = {
  notify: (sender, event) => {
    if (event === 'save') console.log(`${sender} triggered save`);
  }
};
```

**React Use:** Dialog systems or loosely-coupled dashboards.

<br />

### 15. How do you apply the Chain of Responsibility pattern in a React app for handling sequential tasks?

**Concept:** Pass request along a chain until handled.

```
const handlerA = (req, next) => req.type === 'A' ? 'Handled A' : next(req);
const handlerB = (req) => req.type === 'B' ? 'Handled B' : 'Unhandled';

const chain = (req) => handlerA(req, handlerB);
```

**React Use:** Middleware-like UI logic, form validation chains.

<br />

### 16. How do you use the Higher-Order Component (HOC) pattern in React, and what are its advantages and drawbacks?

**Concept:** Function that adds behavior to a component.

```
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading ? <Spinner /> : <Component {...rest} />;
```

**Pros:** Reusability, separation of concerns

**Cons:** Prop collision, nesting complexity

<br />

### 17. What’s your approach to implementing the Render Props pattern in React for sharing logic between components?

**Concept:** Share logic via a function-as-child.

```jsx
<MouseTracker render={({ x, y }) => <Cursor x={x} y={y} />} />
```

**Use:** Reusable logic without HOC pitfalls.

<br />

### 18. How do you design a Provider pattern (e.g., Context API) in React for global state or configuration?

**Concept:** Use React Context to inject global values.

```
const ThemeContext = React.createContext();

const ThemeProvider = ({ children }) => {
  const [theme] = useState("dark");
  return <ThemeContext.Provider value={theme}>{children}</ThemeContext.Provider>;
};
```

**Use:** Auth, theming, config, localization.

<br />

### 19. What’s your strategy for applying the Container/Presentational pattern in a React app?

**Concept:** Separate logic (container) from view (presentational).

```jsx
// Container
const UserContainer = () => {
  const [user, setUser] = useState(null);
  return <UserProfile user={user} />;
};

// Presentational
const UserProfile = ({ user }) => <div>{user?.name}</div>;
```

**Use:** Testing, reuse, separation of concerns.

<br />

### 20. How do you educate a team on selecting and implementing design patterns in a React codebase?

- Use **real-world examples** and **code walkthroughs** .
- Document reusable patterns in a **shared guide or design system** .
- Encourage **pattern discussions in code reviews** .
- Use **pair programming** to reinforce design thinking.

**Goal:** Help devs choose patterns based on intent, not just familiarity.
