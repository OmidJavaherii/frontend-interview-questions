### 1. How do you design an authentication flow in a React application using OAuth 2.0 with a third-party provider (e.g., Google, Auth0)?

Use an OAuth provider (like Auth0 or Google) with **Authorization Code Flow + PKCE** for SPAs.

**Steps** :

1. Redirect user to provider's login URL.
2. Receive `code` in redirect URI.
3. Exchange `code` for access + refresh tokens via secure backend or SDK.
4. Store tokens securely in memory or `httpOnly` cookies.

<br />

### 2. What’s your approach to implementing JWT-based authentication in a React app, including token storage and refresh mechanisms?

- **Access Token** : Short-lived, stored in memory or `sessionStorage`.
- **Refresh Token** : Never in localStorage. Use `httpOnly` cookie for secure handling.

  **Flow** :

1. On login, receive access + refresh tokens.
2. Store access token in memory.
3. On 401, call refresh endpoint (if refresh token available).
4. Auto-refresh via background timer or interceptor.

<br />

### 3. How do you handle multi-factor authentication (MFA) in a React frontend, and what UX considerations do you prioritize?

**Flow** :

1. After successful username/password auth, check for MFA flag.
2. Show MFA input (TOTP, SMS).
3. On success, issue full access token.

**UX priorities** :

- Seamless transition from login → MFA.
- Support for fallback methods.
- Clear error messaging + retry.

<br />

### 4. What’s your strategy for managing user sessions in a single-page application (SPA) with React?

- Use **token expiration** and **inactivity timers** .
- Store minimal user metadata in global state (e.g., Redux, Zustand).
- Auto-refresh or prompt re-auth on token expiration.
- Persist auth state across refresh via secure cookies or encrypted `sessionStorage`.

<br />

### 5. How would you implement a role-based access control (RBAC) system in a React application using authentication data?

Use roles/permissions from the JWT or user metadata.

```jsx
const hasPermission = (user, permission) =>
  user?.permissions?.includes(permission);
```

Conditionally render components or routes:

```tsx
{
  hasPermission(user, "admin:view") && <AdminPanel />;
}
```

Can be enforced at route level using HOCs or custom route guards.

<br />

### 6. How do you design a logout mechanism in a React app that ensures all client-side state and tokens are securely cleared?

- Clear tokens (memory, storage).
- Invalidate refresh token on backend.
- Redirect to login page.

If using cookies, call a backend endpoint to clear server cookie.

<br />

### 7. What’s your approach to handling authentication in a server-side rendered (SSR) React application?

- Use `getServerSideProps` to check auth status via cookies.
- Tokens stored in **httpOnly cookies** for SSR compatibility.
- Redirect unauthenticated users before page render.

<br />

### 8. How do you integrate an authentication system with Redux or Zustand for global state management?

- Store user info (not tokens) in global state.
- Use middleware or effects to intercept auth actions (e.g., refresh token).

<br />

### 9. What’s your process for handling authentication errors (e.g., expired tokens, invalid credentials) in a React app?

- Use a central Axios/Fetch interceptor to catch 401s.
- Retry with refresh token or redirect to login.

<br />

### 10. How do you design a frontend authentication system to support cross-domain or cross-origin scenarios (e.g., micro-frontends)?

- Use **shared `httpOnly` cookies** (with CORS + `credentials: true`).
- Centralized auth service (e.g., identity provider).
- Use postMessage or shared iframe storage carefully.

<br />

### 11. What’s the most challenging security vulnerability you’ve encountered in a frontend application, and how did you address it?

> Encountered an XSS vulnerability where user input was injected into innerHTML without sanitization.

**Fix** : Used DOMPurify for client-side sanitization and refactored to never use `dangerouslySetInnerHTML`.

<br />

### 12. How do you protect a React application from Cross-Site Request Forgery (CSRF) attacks?

- Use `SameSite=Lax` or `Strict` for cookies.
- Implement CSRF tokens for state-changing requests.
- Prefer `httpOnly` cookies over localStorage to mitigate token theft.

<br />

### 13. What’s your approach to securing API calls from a React frontend to prevent man-in-the-middle (MITM) attacks?

- Use HTTPS exclusively.
- Validate SSL certs.
- Store tokens in `httpOnly` cookies, never expose them to JS.
- Enable HSTS headers.

<br />

### 14. How do you design a React application to prevent sensitive data exposure in the browser (e.g., in the DOM or console)?

- Never log tokens or PII.
- Avoid embedding secrets in the DOM or app bundle.
- Mask data in error messages and use error boundaries.

<br />

### 15. What’s your strategy for securing third-party scripts or libraries in a React application?

- Use Subresource Integrity (SRI) where possible.
- Audit dependencies via tools like `npm audit`, `Snyk`.
- Whitelist safe domains via Content Security Policy (CSP).

<br />

### 16. How do you implement input validation and sanitization in a React frontend to prevent injection attacks (e.g., XSS)?

- Use controlled components.
- Sanitize HTML output (e.g., DOMPurify).
- Escape all untrusted content.
- Validate inputs on both frontend and backend.

<br />

### 17. What’s your approach to designing a Content Security Policy (CSP) for a React application?

`Content-Security-Policy:`
`default-src 'self';`
`script-src 'self' [<https://trusted.cdn.com>](<https://trusted.cdn.com/>);`
`object-src 'none';`
Use nonces for inline scripts and report-only mode during testing.

<br />

### 18. How do you handle security in a React application deployed in a zero-trust environment?

- Authenticate every request (stateless JWT).
- Encrypt all communications (TLS).
- Least privilege principle for frontend-accessible resources.
- Use short-lived access tokens and strict CORS.

<br />

### 19. What’s your process for conducting a security audit of a React frontend before a major release?

- Static analysis with ESLint, Snyk, or SonarQube.
- Run `npm audit` and dependency checks.
- Pen test auth flows and sensitive areas.
- Check console logs and exposed dev tools.

<br />

### 20. How do you design a frontend system to comply with security standards like GDPR or HIPAA?

- Data minimization: Only collect what’s needed.
- Explicit consent for cookies + analytics.
- Provide user data download/delete mechanisms.
- Store consent in a verifiable, secure way.
- HIPAA: Avoid storing PHI in the frontend; encrypt all sensitive data at rest + in transit.
