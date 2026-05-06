# Week 06 — Playwright Page Object Model

**Phase:** 2 — Playwright Deep Dive  
**Tool focus:** Playwright (JavaScript)  
**Estimated time:** 3 hours

---

## Overview

Tests that work but are hard to maintain are a liability. The Page Object Model (POM) is the industry-standard pattern for keeping your test code clean, reusable, and easy to update when the UI changes. This week you refactor last week's tests into a proper POM structure using Playwright fixtures.

---

## Topics

### 1. Why POM?

Without POM, every test knows _how_ to interact with the UI. Change one locator and you update it in 20 places. With POM, tests describe _what_ to do — pages describe _how_.

```
Without POM:                          With POM:
test('login', async ({ page }) => {   test('login', async ({ loginPage }) => {
  await page.goto('/');                 await loginPage.login('user', 'pass');
  await page.fill('#user', 'user');     await expect(loginPage.errorMsg)
  await page.fill('#pass', 'pass');       .toBeVisible();
  await page.click('#login-btn');     });
  ...
```

### 2. Page Object Class Structure

```javascript
// pages/LoginPage.js
class LoginPage {
  constructor(page) {
    this.page = page;

    // Locators as properties — defined once, used everywhere
    this.usernameInput = page.getByLabel("Username");
    this.passwordInput = page.getByLabel("Password");
    this.loginButton = page.getByRole("button", { name: "Login" });
    this.errorMessage = page.getByTestId("error");
  }

  async goto() {
    await this.page.goto("/");
  }

  async login(username, password) {
    await this.usernameInput.fill(username);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}

export { LoginPage };
```

### 3. Fixtures

Playwright fixtures inject dependencies into tests automatically:

```javascript
// fixtures/pages.js
import { test as base } from "@playwright/test";
import { LoginPage } from "../pages/LoginPage";
import { InventoryPage } from "../pages/InventoryPage";

export const test = base.extend({
  loginPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await use(loginPage);
  },
  inventoryPage: async ({ page }, use) => {
    await use(new InventoryPage(page));
  },
});
```

### 4. `beforeEach` and `afterEach`

- Use `beforeEach` for test setup (login, navigate to starting state)
- Use `afterEach` for teardown (logout, reset data)
- Prefer fixtures over `beforeEach` for page objects — more composable

### 5. Test Data Separation

- Never hardcode test data in test files
- Store in a `test-data/` folder as JSON or JS objects

```javascript
// test-data/users.js
export const users = {
  standard: { username: "standard_user", password: "secret_sauce" },
  locked: { username: "locked_out_user", password: "secret_sauce" },
  invalid: { username: "bad_user", password: "wrong_pass" },
};
```

### 6. Parameterised Tests

```javascript
const loginCases = [
  { desc: "valid user", ...users.standard, expectSuccess: true },
  { desc: "locked user", ...users.locked, expectSuccess: false },
  { desc: "invalid creds", ...users.invalid, expectSuccess: false },
];

for (const { desc, username, password, expectSuccess } of loginCases) {
  test(`login: ${desc}`, async ({ loginPage }) => {
    await loginPage.login(username, password);
    if (expectSuccess) {
      await expect(loginPage.page).toHaveURL(/inventory/);
    } else {
      await expect(loginPage.errorMessage).toBeVisible();
    }
  });
}
```

---

## Resources

- [Playwright docs — Page Object Models](https://playwright.dev/docs/pom)
- [Playwright docs — Fixtures](https://playwright.dev/docs/test-fixtures)
- [Martin Fowler — PageObject pattern](https://martinfowler.com/bliki/PageObject.html)

---

## Deliverable

> Refactor your Week 5 test suite into a full POM structure.

Required structure:

```
project/
├── pages/
│   ├── LoginPage.js
│   ├── InventoryPage.js
│   └── CheckoutPage.js
├── fixtures/
│   └── pages.js
├── test-data/
│   └── users.js
├── tests/
│   ├── login.spec.js
│   ├── cart.spec.js
│   └── checkout.spec.js
└── playwright.config.js
```

Requirements:

- All 5 tests from Week 5 must still pass after refactoring
- No locators duplicated across files
- Tests use fixtures — not `new PageObject(page)` inline
- `test-data/users.js` used for all credentials
- At least the login test is parameterised with 3 user types

---

## Evaluation Criteria

- [ ] All 5 original tests still pass
- [ ] No locator string appears more than once across the codebase
- [ ] Page classes have action methods — tests don't call `.fill()` or `.click()` directly
- [ ] Tests read like plain English — no implementation detail leaking into test file
- [ ] Fixtures used for page object injection
- [ ] Test data is external — no hardcoded strings in test files
- [ ] Login test is parameterised across at least 3 user scenarios

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your repo link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 6 deliverable. Please evaluate my POM refactor."_

---

_← [Week 05](./week-05-playwright-setup-first-tests.md) · Back to [README](../README.md) · Next: [Week 07 →](./week-07-playwright-api-advanced.md)_
