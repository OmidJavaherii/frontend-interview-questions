### 1. What steps do you take to ensure a React application meets WCAG 2.1 accessibility standards?

To meet WCAG 2.1, I focus on **semantic HTML** , **keyboard support** , **screen reader compatibility** , and **color contrast** .

Key steps:

- Use semantic elements: `<button>`, `<nav>`, `<header>`, etc.
- Provide **text alternatives** : `alt` on images, `aria-label` where needed.
- Ensure **color contrast** ≥ 4.5:1 using tools like Contrast Checker.
- Ensure full **keyboard operability** (Tab, Enter, Escape).
- Use **ARIA roles** and properties only when semantic HTML isn't enough.
- Test with screen readers (e.g. NVDA, VoiceOver).
- Regular audits with **axe** , **Lighthouse** , and **manual testing** .

<br />

### 2. How do you handle keyboard navigation and focus management in a complex React component like a modal or dropdown?

I ensure:

1. **Focus trap** : Keep focus inside the component using libraries like [`focus-trap-react`](https://github.com/focus-trap/focus-trap-react).
2. **Initial focus** : Move focus to the first interactive element on open.
3. **Return focus** : Restore focus to the trigger on close.
4. **Keyboard handlers** : Support Tab/Shift+Tab, Escape, Arrow keys.

<br />

### 3. Describe a time when you improved the accessibility of a frontend feature. What tools did you use?

At my last role, we had a custom dropdown with mouse-only interaction. I:

- Added **keyboard support** : Arrow keys, Enter, Escape.
- Used `aria-expanded`, `aria-activedescendant`, and `role="listbox"`.
- Managed **focus state** properly on open/close.
- Validated changes using **axe DevTools** , **Lighthouse** , and **NVDA** .

Result: WCAG 2.1 AA compliance and improved experience for keyboard and screen reader users.

<br />

### 4. How do you test for accessibility in a React app, and what role do tools like Axe or Lighthouse play?

I use a combination of **automated** , **manual** , and **assistive tech testing** .

Tools:

- **axe-core/axe DevTools** : Detects common a11y issues in-browser or in tests.
- **Jest + jest-axe** : Accessibility regression testing.
- **Lighthouse** : Baseline audits (e.g. color contrast, labels).
- **Screen readers** : VoiceOver, NVDA for real-world behavior.
- **Keyboard testing** : Navigate UI using Tab, Shift+Tab, Enter, and Esc.

<br />

### 5. What are some common accessibility mistakes in React applications, and how do you prevent them?

**Common issues** and solutions:

- ❌ **Non-semantic elements** (e.g. `<div onClick>`):
  ✅ Use `<button>` or add `role="button"` and keyboard handlers.
- ❌ **Missing labels** on inputs:
  ✅ Use `<label for="id">` or `aria-label`.
- ❌ **Focus not managed** in modals/menus:
  ✅ Trap and restore focus properly.
- ❌ **Inaccessible custom components** (tabs, accordions):
  ✅ Implement WAI-ARIA patterns correctly.
- ❌ **Color contrast failures** :
  ✅ Test with automated tools + design with contrast in mind.
