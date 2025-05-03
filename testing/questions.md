### 1. How do you approach testing React components? What tools or libraries do you use (e.g., Jest, React Testing Library)?

I focus on testing **behavior over implementation** using:

- **Jest** for unit and integration tests (fast, reliable runner).
- **React Testing Library (RTL)** for DOM-based interaction testing that mimics real user behavior.
- **MSW (Mock Service Worker)** to mock API responses.
- **Cypress or Playwright** for end-to-end (E2E) tests simulating real browser interactions.

My philosophy: test components as users interact with them — clicking, typing, and observing outputs — rather than internal state.

<br />

### 2. Write a simple test case for a React component that renders a button and updates a counter on click.

```jsx
// Counter.test.tsx
import { render, screen, fireEvent } from "@testing-library/react";
import Counter from "./Counter";

test("increments count on click", () => {
  render(<Counter />);
  fireEvent.click(screen.getByText(/increment/i));
  expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
});
```

<br />

### 3. What’s the difference between unit testing and integration testing in the context of React applications?

- **Unit Testing** : Tests a single component in isolation (e.g., logic, rendering, user events).
  _Example: testing a `Button` component’s click handler._
- **Integration Testing** : Tests how multiple components work together.
  _Example: testing a form that submits data via API and renders a success message._

Unit tests are faster and more focused; integration tests provide higher confidence for real-world flows.

<br />

### 4. How do you mock an API call in a React component test?

I use **MSW (Mock Service Worker)** for realistic API mocking at the network level.

**Example:**

```tsx
import { rest } from "msw";
import { setupServer } from "msw/node";

const server = setupServer(
  rest.get("/api/user", (req, res, ctx) => res(ctx.json({ name: "Alice" })))
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

This keeps tests close to how the app behaves in production.

<br />

### 5. How do you structure unit tests for a React component that relies on an external API?

I isolate the API logic from the UI component and test both:

1. **Unit test the API client** separately (e.g., `getUser()`).
2. In the component test:
   - Mock the API layer (e.g., using MSW or Jest mock).
   - Test UI behavior (e.g., loading spinner, error message, data rendering).

This separation improves test reliability and clarity.

<br />

### 6. Write an integration test using Cypress or Playwright for a React form submission flow.

```tsx
// cypress/e2e/form_spec.cy.js
describe("Contact Form", () => {
  it("submits the form successfully", () => {
    cy.visit("/contact");
    cy.get('input[name="email"]').type("test@example.com");
    cy.get('textarea[name="message"]').type("Hello!");
    cy.intercept("POST", "/api/contact", { statusCode: 200 }).as("submitForm");
    cy.get('button[type="submit"]').click();
    cy.wait("@submitForm");
    cy.contains("Thank you for contacting us").should("be.visible");
  });
});
```

<br />

### 7. What’s your approach to end-to-end testing in a React application with multiple user roles?

I simulate user behavior based on roles (e.g., admin vs. user):

- Use **Cypress** or **Playwright** to log in as different roles via UI or programmatic session setup.
- Test role-based flows:
  - Admin: dashboard access, user management
  - User: limited access, restricted routes
- Stub auth responses or use seeded test accounts in test environments.
- Keep role-based test cases organized (e.g., `admin/`, `user/` folders).

<br />

### 8. How do you ensure test coverage remains meaningful and not just a metric to hit?

I focus on **testing value** , not just numbers:

- Aim for high **assertion density** — not just "covered" lines, but validated outcomes.
- Prioritize **critical paths** : form submission, state updates, conditional rendering.
- Use **code coverage tools** (e.g., `-coverage` in Jest) to identify untested branches — then decide if they warrant a test.
- Avoid overtesting internal implementation details (e.g., `setState` calls).

Regular **test reviews** and **manual QA** help reinforce quality beyond the metrics.
