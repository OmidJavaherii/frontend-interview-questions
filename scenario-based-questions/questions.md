### 1. Design a reusable modal component in React that can be triggered from anywhere in the app.

**Approach** : Use context and portals.

```tsx
// ModalContext.tsx
const ModalContext = createContext(null);
export const useModal = () => useContext(ModalContext);

export function ModalProvider({ children }) {
  const [content, setContent] = useState(null);

  return (
    <ModalContext.Provider value={{ setContent }}>
      {children}
      {content &&
        ReactDOM.createPortal(
          <div className="modal-backdrop">{content}</div>,
          document.body
        )}
    </ModalContext.Provider>
  );
}
```

**Usage** :

```tsx
const { setContent } = useModal();
setContent(<MyModalContent onClose={() => setContent(null)} />);
```

<br />

### 2. How would you handle a situation where a React component needs to fetch data from multiple APIs and combine the results?

**Approach** :

- Use `Promise.all` or `async/await`.
- Combine results in `useEffect`.

```tsx
useEffect(() => {
  const fetchData = async () => {
    const [user, posts] = await Promise.all([
      fetch("/api/user").then((res) => res.json()),
      fetch("/api/posts").then((res) => res.json()),
    ]);
    setData({ user, posts });
  };
  fetchData();
}, []);
```

**Bonus** : Add loading/error states and use `AbortController` for cleanup.

<br />

### 3. Imagine you’re tasked with refactoring a large, poorly maintained React codebase. Where would you start?

**Approach** :

1. **Audit** the codebase: structure, linting, duplicate logic, dead code.
2. **Introduce linting/formatting** : ESLint, Prettier.
3. **Isolate reusable components** into a `shared/` directory.
4. **Add tests** for critical paths before refactoring.
5. **Convert class components to hooks** where possible.
6. **Progressively type with TypeScript** , starting from utils and shared components.

<br />

### 4. How would you implement a feature where a user can drag and drop items in a list in a React app?

**Tool** : Use `@dnd-kit/core` or `react-beautiful-dnd`.

**Example with `react-beautiful-dnd`** :

```tsx
<DragDropContext onDragEnd={handleDragEnd}>
  <Droppable droppableId="list">
    {(provided) => (
      <ul {...provided.droppableProps} ref={provided.innerRef}>
        {items.map((item, index) => (
          <Draggable key={item.id} draggableId={item.id} index={index}>
            {(provided) => (
              <li
                ref={provided.innerRef}
                {...provided.draggableProps}
                {...provided.dragHandleProps}
              >
                {item.name}
              </li>
            )}
          </Draggable>
        ))}
        {provided.placeholder}
      </ul>
    )}
  </Droppable>
</DragDropContext>
```

<br />

### 5. You’re tasked with leading a team to build a dashboard with real-time data updates in React. How would you approach it?

**Architecture** :

- Use **WebSockets** or **SSE** for real-time updates.
- **Redux/Context/Zustand** to manage shared state.
- **Polling fallback** if needed.

  **Component strategy** :

- Split widgets into isolated components.
- Debounce updates to reduce re-renders.
- Show loading/skeleton states.

  **Infrastructure** :

- Use WebSocket manager and reconnection logic.
- Rate limit updates for performance

<br />

### 6. How would you handle a situation where a stakeholder requests a feature that conflicts with performance or accessibility goals?

**Approach** :

- **Empathize** : Acknowledge their goals.
- **Educate** : Share impact (e.g., slow load, broken keyboard nav).
- **Demonstrate** : Build a small POC showing the tradeoff.
- **Compromise** : Propose an alternative that meets both business and technical needs.
- **Document** : Log decisions and revisit if data shows it's worth iterating.

<br />

### 7. Design a reusable data table component in React with sorting, filtering, and pagination features.

**High-level API** :

```tsx
<DataTable
  data={data}
  columns={[
    { key: "name", label: "Name", sortable: true },
    { key: "email", label: "Email" },
  ]}
  pagination={{ pageSize: 10 }}
  onSort={(key, direction) => {}}
  onFilter={(query) => {}}
/>
```

**Implementation considerations** :

- Handle local vs. server-side pagination/sorting.
- Use memoization (`useMemo`) for derived data.
- Extract row rendering via render prop.

<br />

### 8. A React app you inherited has no tests and poor documentation. How would you lead the effort to improve it?

**Plan** :

1. **Inventory critical flows** (e.g., login, checkout).
2. Write **integration tests first** using React Testing Library.
3. Introduce **unit tests** for utility logic and shared components.
4. Use **JSDoc** and Storybook to improve developer documentation.
5. Establish CI pipeline with coverage tracking and lint checks.
6. Create a **contribution guide** to enforce test-driven contributions.

<br />

### 9. How would you integrate a third-party API with rate limits into a React application without degrading user experience?

**Strategies** :

- **Throttle** or **debounce** calls (e.g., using Lodash).
- Use **caching** (e.g., `SWR`, `React Query`) to avoid duplicate requests.
- **Backoff and retry** on 429 responses.
- Provide optimistic UI or loading skeletons to reduce perceived latency.
- If needed, proxy through backend to better manage rate limits globally.

```tsx
const fetcher = async (url) => {
  const res = await fetch(url);
  if (res.status === 429) {
    await new Promise((r) => setTimeout(r, 1000)); // simple backoff
    return fetcher(url);
  }
  return res.json();
};
```
