# Week 05 — Playwright Setup & First Tests

**Phase:** 2 — Playwright Deep Dive  
**Tool focus:** Playwright (JavaScript)  
**Estimated time:** 3 hours

---

## Overview

Time to get hands-on. This week you set up a real Playwright project from scratch and write your first 5 tests. Focus on writing tests that are readable and use Playwright's modern locator API — not legacy CSS hacks. By the end, you should have a working test suite you can run with a single command.

---

## Topics

### 1. Project Initialisation & Config

```bash
npm init playwright@latest
```

- Understand `playwright.config.ts` / `playwright.config.js`:
  - `baseURL` — set this so tests use relative paths
  - `testDir` — where your test files live
  - `use.headless` — `false` while developing, `true` in CI
  - `reporter` — start with `'html'`
  - `projects` — browser configurations (chromium, firefox, webkit)

### 2. `test()` and `expect()`

```javascript
import { test, expect } from "@playwright/test";

test("page title is correct", async ({ page }) => {
  await page.goto("/");
  await expect(page).toHaveTitle(/Swag Labs/);
});
```

- `test.describe()` — group related tests
- `test.only()` — run a single test during development
- `test.skip()` — skip with a reason
- `test.fail()` — mark expected failures

### 3. Locator API (revisit with Playwright context)

- Always prefer `getByRole`, `getByLabel`, `getByTestId`
- Chaining locators: `page.locator('.cart').getByRole('button')`
- Filtering: `page.getByRole('listitem').filter({ hasText: 'Sauce Labs Backpack' })`

### 4. Core Actions & Assertions

```javascript
// Actions
await page.goto("/inventory.html");
await page.getByRole("button", { name: "Add to cart" }).click();
await page.getByLabel("Username").fill("standard_user");
await page.keyboard.press("Enter");
await page.selectOption("select.sort", "za");

// Assertions
await expect(page).toHaveURL(/inventory/);
await expect(page.getByText("Products")).toBeVisible();
await expect(page.getByTestId("shopping-cart-badge")).toHaveText("1");
await expect(page.getByRole("button", { name: "Remove" })).toBeEnabled();
```

### 5. Screenshots, Videos & Traces

In `playwright.config.js`:

```javascript
use: {
  screenshot: 'only-on-failure',
  video: 'retain-on-failure',
  trace: 'on-first-retry',
}
```

- Run `npx playwright show-trace trace.zip` to inspect failures

---

## Resources

- [Playwright docs — Getting Started](https://playwright.dev/docs/intro)
- [Playwright docs — Writing Tests](https://playwright.dev/docs/writing-tests)
- [Playwright docs — Locators](https://playwright.dev/docs/locators)
- [SauceDemo](https://www.saucedemo.com) — recommended target app
  - Login: `standard_user` / `secret_sauce`

---

## Deliverable

> Set up a Playwright project and write **5 end-to-end tests** on SauceDemo covering the flows below.

Required test scenarios:

1. **Login — valid credentials** → lands on inventory page
2. **Login — invalid credentials** → error message shown
3. **Add item to cart** → cart badge updates to `1`
4. **Sort products** → products reorder correctly (A→Z or Z→A)
5. **Checkout flow** → complete a full purchase from cart to order confirmation

Requirements:

- Project uses `playwright.config.js` with `baseURL` set
- All locators use semantic Playwright API (`getByRole`, `getByLabel`, `getByTestId`, `getByText`)
- Zero use of raw CSS selectors (`.class`, `#id`) unless absolutely necessary
- Tests are in a `tests/` folder
- Run with `npx playwright test` — all 5 must pass

Push to GitHub: `deliverables/week-05-playwright-basics/`

---

## Evaluation Criteria

- [ ] All 5 tests pass on first run
- [ ] `playwright.config.js` has `baseURL`, reporter, and screenshot/video settings
- [ ] No raw CSS/XPath locators — uses Playwright semantic API throughout
- [ ] Tests are independent — no shared state between them
- [ ] Each test has a clear, descriptive name
- [ ] At least one test uses a soft assertion (`expect.soft`)
- [ ] Trace is configured — can be opened after a failure

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your repo link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 5 deliverable. Please evaluate my Playwright test suite."_

---

_← [Week 04](../phase-1-foundations/week-04-dom-locators-devtools.md) · Back to [README](../README.md) · Next: [Week 06 →](./week-06-playwright-page-object-model.md)_
