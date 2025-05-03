### 1. Describe a time when you led a frontend team through a challenging project. How did you ensure success?

At my previous role, we had to rebuild a legacy dashboard in React within 3 months to align with a product relaunch. The codebase was tightly coupled, poorly documented, and lacked test coverage.

**My approach:**

- **Audit** : Identified reusable components and pain points.
- **Modular architecture** : Introduced atomic design principles and a UI component library (Storybook-based).
- **Parallel workstreams** : Split the team into feature squads with clear interfaces and boundaries.
- **CI/CD setup** : Enforced ESLint, Prettier, and unit tests in the pipeline.
- **Daily standups and demo Fridays** : Kept visibility high across teams and stakeholders.

We delivered on time, with 90% unit test coverage and a shared component library that accelerated future development.

<br />

### 2. How do you approach mentoring junior developers in React.js or frontend best practices?

I focus on **contextual learning** , **pair programming** , and **code reviews** as mentoring tools.

**Key principles:**

- Break down complex concepts like `useEffect` dependencies or controlled components with **real examples** .
- Encourage them to ask “why,” not just “how,” to build deeper intuition.
- Set small, achievable goals (e.g., build a reusable modal) that reinforce component design and separation of concerns.
- Use PRs as teachable moments with inline explanations and links to resources.

I also rotate juniors through different parts of the codebase so they gain confidence across routing, state management, and UI architecture.

<br />

### 3. What strategies do you use to conduct effective code reviews for a team working on a React codebase?

My reviews focus on **readability** , **reusability** , **performance** , and **consistency** .

**Approach:**

- Check for unnecessary re-renders (e.g., memoization, useCallback).
- Enforce clear separation of concerns: container vs presentational components.
- Encourage proper use of hooks and discourage anti-patterns like side-effects inside render.
- Push for well-named components and clear prop types or TypeScript interfaces.
- Validate accessibility (a11y) practices, such as keyboard navigation and ARIA usage.

I aim to **teach, not gatekeep** , and balance suggestions with praise to maintain a strong feedback culture.

<br />

### 4. How do you balance technical leadership with hands-on coding in a leadership role?

I allocate 50–60% of my time to hands-on coding, focusing on foundational work (e.g., setting up the design system or build tools) and critical path features.

**To stay balanced:**

- I empower senior ICs to lead feature squads, enabling me to mentor and guide architecture without being a bottleneck.
- I block time for code reviews and 1:1s while preserving deep focus time for technical tasks.
- I use RFCs to communicate architectural direction, encouraging feedback and alignment.

This approach keeps me close to the code while creating space for team growth and scalability.

<br />

### 5. Tell us about a time you resolved a conflict between team members over a technical decision (e.g., choosing a state management library).

Two senior devs were debating whether to use Redux Toolkit or Zustand for a new feature. One valued Redux’s structure, the other preferred Zustand’s simplicity.

**My resolution process:**

- Facilitated a spike for both options with performance and DX comparisons.
- Held a decision-making meeting with the team using a lightweight decision matrix (complexity, scalability, learning curve).
- Aligned on goals: fast iteration, low boilerplate, future extensibility.

We chose Zustand for the MVP with a wrapper layer to abstract store access, allowing for future migration. The key was **neutral facilitation and evidence-based decision making** .

<br />

### 6. How do you align frontend development goals with business objectives when collaborating with product managers and stakeholders?

I make technical goals **visible** and tie them to **business outcomes** .

**Example:**
When advocating for refactoring to a component library, I framed it not as "technical debt cleanup," but as:

- "Faster time to market via reusability"
- "Improved brand consistency and UX"
- "Reduced design QA cycles by 30%"

I work closely with PMs to **prioritize tech tasks in the roadmap** , often bundling them with user-facing features (e.g., a UI refactor alongside a new settings page).

Transparency and shared language (not jargon) are key to alignment.

<br />

### 7. What’s your process for conducting performance reviews for engineers on your team?

I use a **360-degree** feedback approach aligned with clear competency matrices.

**Process:**

- Review goals set during the previous cycle (technical, collaborative, and personal development).
- Collect feedback from peers and cross-functional partners.
- Highlight strengths and areas for growth across technical execution, ownership, and communication.
- Create a development plan (e.g., lead a cross-team initiative, mentor a junior).

I also encourage **self-assessment** to drive introspection, and make sure feedback is actionable, not vague.

<br />

### 8. How have you contributed to hiring efforts, and what do you look for in a strong frontend candidate?

I’ve led hiring pipelines: wrote job descriptions, built take-home assessments, and conducted behavioral + technical interviews.

**What I look for:**

- **Core fundamentals** : JS, HTML/CSS, React/Vue internals.
- **Problem-solving** : Can they debug a broken component methodically?
- **Architecture thinking** : Can they break down a UI into clean components with reusable logic?
- **Communication** : Can they explain trade-offs clearly?

Cultural alignment is also key — curiosity, empathy, and a bias toward collaboration matter as much as technical skill.

<br />

### 9. How do you ensure knowledge sharing and documentation in a team working on a large React/Vue.js project?

**Tactics:**

- **Weekly "Dev Nuggets" sessions** : Short internal talks to share learnings.
- Enforce **README + usage examples** for any new shared component.
- Adopt **Storybook** as living documentation.
- Use architecture decision records (ADRs) to log key choices.
- Encourage documentation PRs as part of feature delivery (not afterthoughts).

I also champion code that’s **self-documenting** — clear naming, clean abstractions, and comments only when necessary.

<br />

### 10. Describe a situation where you advocated for a frontend technology or approach to non-technical stakeholders. How did you make your case?

I proposed introducing a design system using Tailwind + Storybook to replace inconsistent, duplicated styles across 4 products.

**Challenges:**
Non-technical stakeholders saw it as "extra work" without immediate ROI.

**How I made the case:**

- Demoed inconsistencies live (e.g., 6 versions of the same button).
- Quantified dev time wasted recreating UI components (~25%).
- Showed a working Storybook prototype and how it enables faster iteration and easier onboarding.
- Aligned benefits with product goals: faster feature delivery, consistent brand feel, reduced design debt.

Got buy-in and resourced a 2-sprint effort that paid off within a quarter.
