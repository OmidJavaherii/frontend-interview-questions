### How do you design a CI/CD pipeline for a React application to ensure fast feedback loops and reliable deployments?

- **CI:** Lint → Unit Tests → Build
- **CD:** Deploy to preview (e.g., Vercel/Netlify) → Run E2E tests → Promote to staging/production
- Use **GitHub Actions** , **CircleCI** , or **GitLab CI** .

```yaml
# GitHub Actions sample
jobs:
  build-test:
    steps:
      - run: npm ci
      - run: npm run lint && npm test
  deploy:
    steps:
      - run: npm run build
      - run: npm run deploy:staging
```

### What’s your approach to automating the deployment of a React app to multiple environments (e.g., dev, staging, prod)?

- Use **environment-specific config** (`.env.dev`, `.env.staging`).
- Use branches or tags (e.g., `main → prod`, `develop → dev`).
- Deployment scripts read environment context dynamically.

```bash

if [ "$ENV" == "staging" ]; then
  npm run deploy:staging
fi

```

### How do you integrate automated testing (unit, integration, E2E) into a CI/CD pipeline for a React project?

- **Unit tests:** Jest
- **Integration:** React Testing Library
- **E2E:** Cypress or Playwright (headless in CI)

```yaml
steps:
  - run: npm test -- --coverage
  - run: npx cypress run
```

### What’s your strategy for handling rollbacks in a CI/CD pipeline when a deployment fails in production?

- Use **immutable deployments** + **Git tags** or **release artifacts** .
- Store last known good build and automate rollback:

```bash

aws s3 cp s3://backups/last-good-build ./ && npm run deploy

```

### How do you optimize a CI/CD pipeline to reduce build times for a large React codebase?

- Enable **incremental builds** via TurboRepo or Nx
- **Cache node_modules** and **build artifacts**
- Parallelize jobs:

```yaml
- uses: actions/cache@v3
  with:
    path: node_modules
```

### What’s your approach to securing a CI/CD pipeline for a React application?

- Rotate and **encrypt secrets** (use GitHub Actions Secrets, Vault, etc.)
- **Restrict write access** to main branches
- Use **signed commits** , dependency checks (e.g., `npm audit`)

### How do you implement blue-green or canary deployments in a CI/CD pipeline for a React app?

- Use **feature flags** + gradual rollout (e.g., LaunchDarkly)
- Platform-level support (e.g., Vercel’s preview → promote)

```

if (user.isInCanaryGroup) {
  return <NewComponent />
}

```

### What’s your process for integrating static code analysis and linting into a CI/CD pipeline?

- Run ESLint, TypeScript, and Prettier in CI
- Block PRs on violations:

```yaml
- run: npm run lint && npm run type-check
```

### How do you handle dependency management and versioning in a CI/CD pipeline for a React project?

- Use **npm lockfile** (`package-lock.json`)
- Automate with **Dependabot**
- Track package diffs in CI

### How do you educate a team on adopting and maintaining a CI/CD pipeline for a React application?

- Pair sessions + documentation
- Visual workflows in PRs
- Champion internal demos + encourage shared ownership

### How do you design a monitoring system for a React application in production to track performance and errors?

- **Client monitoring:** Sentry, LogRocket, Datadog RUM
- **Error tracking:** Try/catch + boundary logging
- **Performance:** report via Web Vitals API

### What’s your approach to monitoring Core Web Vitals (e.g., LCP, CLS, FID) in a React app post-deployment?

- Use [`web-vitals`](https://github.com/GoogleChrome/web-vitals) package to send metrics to a backend or analytics provider:

```

import { onCLS, onFID, onLCP } from 'web-vitals';
onCLS(sendToAnalytics); onLCP(sendToAnalytics); onFID(sendToAnalytics);

```

### How do you set up real-time alerts for a React application to catch runtime errors or performance degradation?

- Integrate tools like Sentry or Datadog with Slack/Webhook alerts
- Set up thresholds for alerting (e.g., >1% error rate)

## What’s your strategy for monitoring API latency and availability in a React app that relies on a backend?

- Use frontend timing APIs (`performance.now()`)
- Add tracing headers (e.g., `x-request-id`)
- Report slowness:

```

const start = performance.now();
fetch('/api/data').finally(() => {
  const duration = performance.now() - start;
  if (duration > 1000) logSlowAPI('/api/data', duration);
});

```

### How do you use monitoring tools to track the health of a CI/CD pipeline itself (e.g., build success rates, deployment times)?

- Use built-in metrics from GitHub Actions / CircleCI
- Monitor:
  - Success/failure rates
  - Average build & deploy time
  - Flaky tests

### What’s your approach to logging in a React application, and how do you integrate it with monitoring tools?

- Use **console abstraction** :

```jsx
export const log = (msg) => {
  if (process.env.NODE_ENV === "production") {
    sendToLoggingService(msg);
  } else {
    console.log(msg);
  }
};
```

- Integrate with **Sentry breadcrumbs** or **Datadog logs** .

### How do you implement distributed tracing in a React app with a microservices backend?

- Inject trace headers from backend
- Use **OpenTelemetry** or **Datadog RUM**
- Propagate correlation IDs:

```jsx
fetch("/api", {
  headers: { "x-trace-id": currentTraceId },
});
```

### What’s your process for monitoring resource usage (e.g., CPU, memory) in a React app deployed on a serverless platform?

- Use platform-native tools (e.g., Vercel Analytics)
- Report client-side memory usage if needed:

```

if (performance.memory) {
  log(performance.memory.usedJSHeapSize);
}

```

### How do you leverage monitoring tools to support A/B testing or feature flag rollouts in a React app?

- Track usage via custom events (e.g., Segment, Amplitude)
- Tag sessions with active flags:

```jsx
analytics.track("feature_used", { variant: "B" });
```

### How do you train a team to use monitoring tools effectively for a React application in production?

- Create dashboards for visibility
- Run **"failure mode" drills**
- Rotate ownership of alerts
- Document SOPs for triage/escalation
