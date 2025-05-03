### 1. How do you approach styling in React? Compare CSS-in-JS, Styled Components, Sass, and TailwindCSS.

I choose styling tools based on the project's complexity, team preferences, scalability, and performance needs.

| Technique                                       | Pros                                                      | Cons                                                             |
| ----------------------------------------------- | --------------------------------------------------------- | ---------------------------------------------------------------- |
| **CSS-in-JS**(e.g., Emotion, styled-components) | Scoped styles, dynamic theming, colocated with components | Runtime overhead, harder to extract critical CSS                 |
| **Styled Components**                           | Component-driven, theming support, autoscoping            | Bundle size, runtime parsing, toolchain complexity               |
| **Sass (SCSS)**                                 | Nesting, variables, mixins, mature tooling                | Global scope by default, manual BEM or conventions needed        |
| **TailwindCSS**                                 | Utility-first, fast prototyping, responsive by default    | Verbose class names, learning curve, poor separation of concerns |

**My default** : Tailwind for fast-moving teams or design systems; Styled Components for themeable component libraries.

<br />

### 2. What strategies do you use to ensure a React application is responsive across different devices?

- **Mobile-first design** using responsive units (`rem`, `%`, `vh/vw`) and media queries.
- **CSS Grid / Flexbox** for layout adaptability.
- **TailwindCSS** or utility classes (`sm:`, `md:`) for breakpoint-driven styling.
- Use the `useWindowSize` hook or libraries like `react-responsive` when needed for behavior, not just style.

<br />

### 3. How would you implement a dark mode toggle in a React application?

Use a `theme` state (or context), toggle a `class` on the `body` or `html`, and style conditionally.

**CSS-in-JS example** : Use a `ThemeProvider` with light/dark themes.

<br />

### 4. Design a reusable button component in React with Styled Components that supports multiple variants (e.g., primary, secondary).

```jsx
import styled from 'styled-components';

const Button = styled.button`
  padding: 0.5rem 1rem;
  border-radius: 4px;
  font-weight: bold;
  background: ${({ variant }) =>
    variant === 'primary' ? '#007bff' : '#6c757d'};
  color: white;
  border: none;
  cursor: pointer;

  &:hover {
    opacity: 0.9;
  }
`;

// Usage:
<Button variant="primary">Save</Button>
<Button variant="secondary">Cancel</Button>

```

<br />

### 5. What are the pros and cons of using TailwindCSS in a large React project with multiple developers?

**Pros:**

- Consistency through utility classes.
- Excellent for **design systems** and **responsive layouts** .
- Reduces context switching between CSS and markup.
- Great **developer velocity** .

**Cons:**

- Verbose class names can hurt readability.
- Custom components need plugin work or extra abstraction (e.g., `clsx`, `cva`).
- Styling logic embedded in JSX—harder to reuse if not abstracted properly.

  **Mitigation** : Use helper utilities (`clsx`, `tailwind-variants`) and design tokens to reduce duplication.

<br />

### 6. How do you manage CSS specificity conflicts in a React app with a mix of global and component-scoped styles?

- **Prefer component-scoped styles** (e.g., CSS Modules, styled-components).
- Namespace global styles (`body.my-app`) and avoid generic selectors.
- Use **BEM** or conventions when global styles are required.
- Avoid using `!important` unless absolutely necessary.
- Audit styles with DevTools → Specificity trace.

<br />

### 7. How do you implement a CSS animation in React that triggers based on a state change?

```jsx
import "./Fade.css"; // .fade { opacity: 0; transition: opacity 0.3s } .fade.show { opacity: 1; }

const FadeComponent = ({ visible }) => (
  <div className={`fade ${visible ? "show" : ""}`}>Hello</div>
);
```
