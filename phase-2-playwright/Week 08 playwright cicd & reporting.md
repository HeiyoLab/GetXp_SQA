# Week 08 — Playwright CI/CD & Reporting

**Phase:** 2 — Playwright Deep Dive  
**Tool focus:** Playwright (JavaScript) · GitHub Actions · Allure  
**Estimated time:** 3–4 hours

---

## Overview

A test suite that only runs on your laptop is not a safety net — it's a hobby. This week you wire your Playwright project into GitHub Actions so tests run on every push and pull request, and you set up Allure to produce a proper HTML report. This is the deliverable that impresses interviewers most.

---

## Topics

### 1. GitHub Actions Workflow

Create `.github/workflows/playwright.yml`:

```yaml
name: Playwright Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 30

    steps:
      - uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      - name: Run Playwright tests
        run: npx playwright test
        env:
          TEST_USER: ${{ secrets.TEST_USER }}
          TEST_PASS: ${{ secrets.TEST_PASS }}
          BASE_URL: ${{ secrets.BASE_URL }}

      - name: Upload Playwright report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 7
```

### 2. Allure Reporter

Install:

```bash
npm install --save-dev allure-playwright
```

In `playwright.config.js`:

```javascript
reporter: [["allure-playwright"], ["html", { open: "never" }], ["list"]];
```

Generate and open locally:

```bash
npx allure generate allure-results --clean -o allure-report
npx allure open allure-report
```

Allure annotations in tests:

```javascript
import { test, expect } from "@playwright/test";
import { allure } from "allure-playwright";

test("checkout flow", async ({ page }) => {
  allure.description("Full checkout from cart to confirmation");
  allure.tag("regression");
  allure.severity("critical");
  allure.owner("Aerry");
  // ... test steps
});
```

### 3. Parallel Execution

In `playwright.config.js`:

```javascript
workers: process.env.CI ? 2 : 4,
fullyParallel: true,
```

Rules for parallelisable tests:

- Each test must be fully independent — no shared state
- Each test creates its own data via API, not relying on previous tests
- No hardcoded IDs or sequenced steps across test files

### 4. Test Tagging & Filtering

Tag tests for selective runs:

```javascript
test('smoke: homepage loads', { tag: '@smoke' }, async ({ page }) => { ... });
test('regression: checkout', { tag: '@regression' }, async ({ page }) => { ... });
```

Run by tag:

```bash
npx playwright test --grep @smoke
npx playwright test --grep-invert @slow
```

In GitHub Actions — run only smoke tests on every push, full suite on PR to main:

```yaml
- name: Run smoke tests
  run: npx playwright test --grep @smoke
```

### 5. Secrets Management

Store sensitive values in GitHub Secrets (Settings → Secrets → Actions):

- `TEST_USER`, `TEST_PASS`, `BASE_URL`
- Reference in workflow as `${{ secrets.SECRET_NAME }}`
- Access in tests via `process.env.TEST_USER`
- Never commit `.env` files — add to `.gitignore`

---

## Resources

- [Playwright docs — CI](https://playwright.dev/docs/ci)
- [Playwright docs — GitHub Actions](https://playwright.dev/docs/ci-github-actions)
- [Allure Playwright docs](https://allurereport.org/docs/playwright/)
- [GitHub Actions docs](https://docs.github.com/en/actions)

---

## Deliverable

> Push your Playwright project to GitHub with a working CI pipeline and Allure reporting.

Requirements:

1. `.github/workflows/playwright.yml` triggers on push to `main` and on PRs
2. Workflow installs deps, installs Playwright browsers, runs tests, uploads report artifact
3. Allure reporter configured — report generated as part of CI
4. Tests run with at least `workers: 2` in CI
5. Secrets used for credentials — not hardcoded in workflow file
6. `README.md` in the project explains how to run tests locally and describes the CI setup
7. At least 2 tests tagged with `@smoke` that run independently

**Bonus:** Add a status badge to your `README.md`:

```markdown
![Playwright Tests](https://github.com/your-username/your-repo/actions/workflows/playwright.yml/badge.svg)
```

---

## Evaluation Criteria

- [ ] Workflow file exists and is valid YAML (no syntax errors)
- [ ] Pipeline runs green on GitHub — link to passing run provided
- [ ] Allure report artifact is available for download from CI run
- [ ] Tests run in parallel (2+ workers in CI)
- [ ] Credentials are in GitHub Secrets — no hardcoded values in workflow
- [ ] README documents: install, run, CI setup
- [ ] `@smoke` tag present on at least 2 tests
- [ ] CI badge in README

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your GitHub repo link, a link to a passing CI run, and your reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 8 deliverable. Please evaluate my Playwright CI/CD setup."_

---

_← [Week 07](./week-07-playwright-api-advanced.md) · Back to [README](../README.md) · Next: [Week 09 →](../phase-3-selenium/week-09-selenium-setup-core.md)_
