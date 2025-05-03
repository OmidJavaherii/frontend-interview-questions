### How do you collaborate with backend teams to design APIs that meet the needs of a React frontend?

I start by aligning on **use cases and data needs** via API contracts or OpenAPI specs. I advocate for:

- **REST or GraphQL** depending on complexity
- Consistent resource structures
- Pagination, filtering, and sorting support

We use **Postman/Swagger** for early mocks and **shared API design docs** . I emphasize **frontend-first thinking** : minimize over-fetching and support real-time or lazy-loading if needed.

### What’s your approach to handling API versioning in a React app when the backend evolves?

**Answer:**

I prefer **URL-based versioning** (e.g., `/v1/users`) for clarity and cacheability. In React:

- Encapsulate API logic in services:

```jsx
// api/user.ts
export const fetchUser = () => axios.get("/v2/user");
```

- Use **feature flags** or environment-based toggles to migrate gracefully
- Track version drift with integration tests

### How do you design a React frontend to handle inconsistent or poorly documented backend APIs?

**Answer:**

To mitigate risk:

- Use **TypeScript** with runtime validators like `zod` or `io-ts`:

```jsx
const UserSchema = z.object({ id: z.string(), name: z.string() });
```

- Abstract API responses via adapters to normalize data:

```jsx
const mapUser = (data) => ({ id: data.user_id, name: data.full_name });
```

- Document and flag inconsistencies for backend refactoring

### What’s your strategy for optimizing API payloads for a React app’s performance?

**Answer:**

- Request only needed fields (`fields=name,email` or GraphQL selection sets)
- Paginate lists to reduce load:

```

axios.get('/users?page=2&limit=10');

```

- Compress responses (Gzip/Brotli) and cache where possible
- Use **server-driven sorting/filtering** to avoid client-side overhead

### How do you integrate real-time APIs (e.g., WebSockets, Server-Sent Events) into a React app?

**Answer:**

- Use **WebSocket** for bi-directional data:

```jsx
useEffect(() => {
  const socket = new WebSocket("wss://example.com/socket");
  socket.onmessage = (event) => updateState(JSON.parse(event.data));
}, []);
```

- Use context or Zustand to share state updates
- For SSE: use `EventSource`, but fall back to polling if needed
- Clean up sockets on unmount to prevent memory leaks

### What’s your approach to mocking APIs during React development when the backend isn’t ready?

**Answer:**

- Use **Mock Service Worker (MSW)** to intercept real `fetch`/`axios`:

```

rest.get('/api/user', (req, res, ctx) => res(ctx.json({ name: 'Test' })));

```

- Mirror backend contract using JSON mocks or Swagger
- Toggle mock mode via `NODE_ENV` or dev flag
- Mock at the network level to test integration, not logic

### How do you handle rate-limited APIs in a React app without degrading user experience?

**Answer:**

- Implement **exponential backoff** or `Retry-After` headers handling
- Queue or debounce high-frequency requests:

```

const debouncedSearch = debounce(fetchSuggestions, 300);

```

- Show UI feedback (e.g., spinners, retry buttons)
- Cache frequent responses locally when possible (e.g., `react-query`, SWR)

### What’s your process for debugging frontend-backend integration issues in a React app?

**Answer:**

1. **Check network tab** (status codes, headers, body)
2. Use `console.error` or `error boundaries` for visibility
3. Validate API types using schemas
4. Log and compare frontend request with backend expectations
5. Use `Postman` or `curl` to isolate backend issues

Collaborate closely with backend engineers, ideally in real-time debugging sessions.

### How do you advocate for frontend needs (e.g., pagination, filtering) in backend API design discussions?

**Answer:**

- Provide **user-centric scenarios** showing impact (e.g., slow loads, poor UX)
- Suggest standards like `limit`, `offset`, `sort`, `filter` query params
- Emphasize **mobile performance** and **data granularity**
- Use shared API specs or examples to bridge gaps

### How do you train a frontend team to work effectively with backend engineers on a React project?

**Answer:**

- Introduce **API design-first mindset** (e.g., OpenAPI, GraphQL schema-first)
- Define **clear contracts** and use tools like MSW for development
- Encourage joint reviews of API designs
- Set up consistent conventions (e.g., service abstraction, error handling)
- Promote **empathy-driven collaboration** by having both sides participate in each other’s demos and retros
