# Week 07 — Playwright API Testing & Advanced

**Phase:** 2 — Playwright Deep Dive  
**Tool focus:** Playwright (JavaScript)  
**Estimated time:** 3 hours

---

## Overview

Real-world test suites don't just click buttons — they set up test data via API, mock network calls, and handle authentication flows cleanly. This week takes your Playwright skills beyond the UI layer and into hybrid testing patterns that are far more reliable than UI-only setups.

---

## Topics

### 1. `request` Context — API Testing in Playwright

```javascript
import { test, expect } from "@playwright/test";

test("GET /users returns 200 with correct schema", async ({ request }) => {
  const response = await request.get(
    "https://jsonplaceholder.typicode.com/users/1",
  );

  expect(response.status()).toBe(200);

  const body = await response.json();
  expect(body).toMatchObject({
    id: expect.any(Number),
    name: expect.any(String),
    email: expect.stringContaining("@"),
  });
});
```

Key methods:

- `request.get(url, options)`
- `request.post(url, { data: {...} })`
- `request.put()`, `request.delete()`
- `request.storageState()` — save auth tokens

### 2. API Setup → UI Verification Pattern

Use the API to create test data, then verify it in the UI. This avoids slow UI setup flows:

```javascript
test("newly created booking appears in UI", async ({ page, request }) => {
  // Fast API setup
  const booking = await request.post("/api/bookings", {
    data: { name: "Aerry", date: "2025-12-01", roomId: 1 },
  });
  const { id } = await booking.json();

  // UI verification
  await page.goto(`/bookings/${id}`);
  await expect(page.getByText("Aerry")).toBeVisible();
});
```

### 3. Network Interception & Mocking

```javascript
// Intercept and mock an API response
await page.route("**/api/products", async (route) => {
  await route.fulfill({
    status: 200,
    contentType: "application/json",
    body: JSON.stringify([{ id: 1, name: "Test Product", price: 10 }]),
  });
});

// Abort a specific request (e.g. analytics)
await page.route("**/analytics/**", (route) => route.abort());

// Modify a response in-flight
await page.route("**/api/user", async (route) => {
  const response = await route.fetch();
  const json = await response.json();
  json.role = "admin"; // inject a field
  await route.fulfill({ response, json });
});
```

### 4. Storage State & Auth

Save an authenticated session once and reuse across tests — no repeated login flows:

```javascript
// global-setup.js — run once before all tests
import { chromium } from "@playwright/test";

async function globalSetup() {
  const browser = await chromium.launch();
  const page = await browser.newPage();

  await page.goto("https://your-app.com/login");
  await page.getByLabel("Username").fill(process.env.TEST_USER);
  await page.getByLabel("Password").fill(process.env.TEST_PASS);
  await page.getByRole("button", { name: "Login" }).click();
  await page.waitForURL("/dashboard");

  // Save session to disk
  await page.context().storageState({ path: "auth/user.json" });
  await browser.close();
}

export default globalSetup;
```

In `playwright.config.js`:

```javascript
globalSetup: './global-setup.js',
use: {
  storageState: 'auth/user.json',
}
```

### 5. Custom Matchers

Extend Playwright's `expect` for domain-specific assertions:

```javascript
// In your fixture or setup file
expect.extend({
  toBeWithinRange(received, floor, ceiling) {
    const pass = received >= floor && received <= ceiling;
    return {
      pass,
      message: () =>
        `expected ${received} to be between ${floor} and ${ceiling}`,
    };
  },
});

// Usage
expect(productCount).toBeWithinRange(1, 100);
```

---

## Resources

- [Playwright docs — API Testing](https://playwright.dev/docs/api-testing)
- [Playwright docs — Network](https://playwright.dev/docs/network)
- [Playwright docs — Authentication](https://playwright.dev/docs/auth)
- [Restful Booker API](https://restful-booker.herokuapp.com/apidoc/) — good practice target

---

## Deliverable

> Write a **hybrid test** that uses API to set up state, verifies it in the UI, and includes 2 pure API tests.

Requirements:

1. **Hybrid test:** Use `request` context to create a record via API, then navigate to the UI and assert the record is visible
2. **API test 1:** `GET` a resource — assert status `200`, response time < 1000ms, and at least 3 body fields
3. **API test 2:** `POST` a new resource — assert `201`, response body contains the sent data, and a returned `id` field
4. **Network mock:** At least one test uses `page.route()` to mock or modify an API response
5. `storageState` configured so tests don't re-login on every run

---

## Evaluation Criteria

- [ ] Hybrid test creates data via API — no UI form submission for setup
- [ ] Both pure API tests pass with meaningful assertions
- [ ] Response time assertion present on at least one test
- [ ] Network mock test demonstrates actual request interception
- [ ] Auth handled via `storageState` — no `beforeEach` login steps
- [ ] No hardcoded credentials — uses `process.env`

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your repo link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 7 deliverable. Please evaluate my Playwright API and advanced tests."_

---

_← [Week 06](./week-06-playwright-page-object-model.md) · Back to [README](../README.md) · Next: [Week 08 →](./week-08-playwright-cicd-reporting.md)_
