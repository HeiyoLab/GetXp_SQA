# Week 13 — Test Framework Design

**Phase:** 4 — Framework Architecture  
**Tool focus:** Playwright + Selenium (Both)  
**Estimated time:** 3–4 hours

---

## Overview

Senior QA engineers don't just write tests — they design the system that tests run in. This week you step back from writing individual tests and think about framework architecture: folder structure, config management, reusable utilities, and the patterns that make a test suite maintainable at scale. You'll produce both a design document and a scaffolded framework.

---

## Topics

### 1. Framework Types

| Type               | How it works                           | Best for                             |
| ------------------ | -------------------------------------- | ------------------------------------ |
| **Linear**         | Scripts with no abstraction            | Prototyping, quick demos             |
| **Modular**        | Functions/methods grouped by feature   | Small projects                       |
| **Data-Driven**    | Logic fixed, data external             | Form-heavy apps, parameterised tests |
| **Keyword-Driven** | Actions expressed as readable keywords | Non-technical teams, BDD light       |
| **Hybrid**         | Combination of data-driven + modular   | Most real-world projects             |
| **BDD (Gherkin)**  | `Given/When/Then` scenarios            | Business-facing tests                |

Most modern Playwright frameworks are **hybrid** — POM for structure, external data, and fixture-based composition.

### 2. Folder Structure Best Practices

A well-organised Playwright framework:

```
project/
├── .github/
│   └── workflows/
│       └── playwright.yml
├── pages/                    # Page object classes
│   ├── BasePage.js           # Shared page methods
│   ├── LoginPage.js
│   └── DashboardPage.js
├── fixtures/                 # Playwright test fixtures
│   └── index.js
├── helpers/                  # Reusable utility functions
│   ├── api-helper.js         # API setup/teardown
│   ├── date-helper.js
│   └── db-helper.js          # If DB access needed
├── test-data/                # External test data
│   ├── users.json
│   └── products.json
├── tests/                    # Test files grouped by feature
│   ├── auth/
│   │   └── login.spec.js
│   ├── cart/
│   │   └── cart.spec.js
│   └── checkout/
│       └── checkout.spec.js
├── auth/                     # Saved auth states (gitignored)
│   └── user.json
├── .env.example              # Env variable template
├── playwright.config.js
└── package.json
```

### 3. Config Management

```javascript
// playwright.config.js — environment-aware
const ENV = process.env.TEST_ENV || 'staging';

const envConfig = {
  staging: { baseURL: 'https://staging.app.com' },
  prod:    { baseURL: 'https://app.com' },
  local:   { baseURL: 'http://localhost:3000' },
};

export default defineConfig({
  use: {
    baseURL: envConfig[ENV].baseURL,
    ...
  }
});
```

Run against different environments:

```bash
TEST_ENV=staging npx playwright test
TEST_ENV=prod npx playwright test --grep @smoke
```

### 4. Base Page Pattern

```javascript
// pages/BasePage.js
class BasePage {
  constructor(page) {
    this.page = page;
  }

  async navigate(path = "/") {
    await this.page.goto(path);
  }

  async waitForPageLoad() {
    await this.page.waitForLoadState("networkidle");
  }

  async getTitle() {
    return this.page.title();
  }

  async takeScreenshot(name) {
    await this.page.screenshot({
      path: `screenshots/${name}.png`,
      fullPage: true,
    });
  }
}

export { BasePage };
```

### 5. Reusable Utilities

```javascript
// helpers/api-helper.js
async function createTestUser(request, userData) {
  const response = await request.post("/api/users", { data: userData });
  expect(response.status()).toBe(201);
  return response.json();
}

async function deleteTestUser(request, userId) {
  await request.delete(`/api/users/${userId}`);
}

export { createTestUser, deleteTestUser };
```

---

## Resources

- [Playwright docs — Best Practices](https://playwright.dev/docs/best-practices)
- [Martin Fowler — Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [Google Testing Blog — Test Architecture](https://testing.googleblog.com/)

---

## Deliverable

> Design and scaffold a hybrid test framework from scratch, and write an **Architecture Decision Document (ADD)**.

**Part 1 — Framework Scaffold:**
Create a new repo (or folder) with the full folder structure above. Include:

- `BasePage.js` with at least 5 shared methods
- 2 page objects that extend `BasePage`
- `playwright.config.js` with multi-environment support
- A fixture file that wires up page objects
- A helper utility (API or date/data helper)
- `.env.example` with all required variables documented
- At least 1 passing test to prove the scaffold works

**Part 2 — Architecture Decision Document:**
Write `ARCHITECTURE.md` covering:

1. Framework type chosen and why
2. Folder structure explanation — what lives where and why
3. How to add a new page object (step-by-step)
4. How to add a new test (step-by-step)
5. How environment switching works
6. Known limitations and trade-offs

---

## Evaluation Criteria

- [ ] Framework scaffold runs — at least 1 test passes
- [ ] `BasePage` has shared utilities that child pages actually use
- [ ] Config supports at least 2 environments via env variable
- [ ] `ARCHITECTURE.md` covers all 6 sections
- [ ] "How to add a page object" steps are clear enough for a new team member to follow
- [ ] `.env.example` documents every required variable with a description
- [ ] `auth/` folder is in `.gitignore`

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 13 deliverable."_

---

_← [Week 12](../phase-3-selenium/week-12-selenium-crossbrowser-grid.md) · Back to [README](../README.md) · Next: [Week 14 →](./week-14-api-testing-deep-dive.md)_
