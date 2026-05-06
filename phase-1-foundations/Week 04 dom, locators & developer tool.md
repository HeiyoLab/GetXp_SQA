# Week 04 — DOM, Locators & Developer Tools

**Phase:** 1 — Foundations Reset  
**Tool focus:** General (Browser DevTools)  
**Estimated time:** 2–3 hours

---

## Overview

Writing stable automated tests starts with choosing the right locators. Brittle selectors are the number one cause of flaky tests. This week builds your ability to inspect any web application and write locators that survive UI changes — using CSS, XPath, ARIA roles, and text.

---

## Topics

### 1. DOM Structure

- HTML is a tree of nodes — understand parent, child, sibling relationships
- How browsers parse and render HTML into the DOM
- The difference between the HTML source and the live DOM
- How JavaScript can modify the DOM after page load (why static selectors sometimes break)

### 2. CSS Selectors

These are your primary locator strategy in Playwright:

| Selector        | Example                         | Notes                                  |
| --------------- | ------------------------------- | -------------------------------------- |
| ID              | `#submit-btn`                   | Unique, fast, but often auto-generated |
| Class           | `.login-form`                   | Often shared across elements           |
| Attribute       | `[data-testid="submit"]`        | Best practice for test-specific hooks  |
| Tag + attribute | `input[type="email"]`           | Semantic and stable                    |
| Descendant      | `.form input`                   | Space between ancestors                |
| Child           | `.form > input`                 | Direct child only                      |
| Pseudo-classes  | `li:first-child`                | Positional — fragile                   |
| Multiple        | `button.primary[type="submit"]` | Combine for specificity                |

### 3. XPath Basics

- When CSS can't reach an element (e.g. text-based selection, parent traversal)
- Absolute vs relative XPath — always use relative (`//`)
- Useful XPath patterns:

```xpath
//button[text()='Submit']
//input[@placeholder='Email address']
//div[@class='card']//span[contains(text(),'Error')]
//label[text()='Username']/following-sibling::input
```

### 4. Playwright Locator API

Playwright adds semantic locators on top of CSS/XPath:

```javascript
// Preferred — resilient to layout changes
page.getByRole("button", { name: "Submit" });
page.getByLabel("Email address");
page.getByPlaceholder("Enter your email");
page.getByText("Welcome back");
page.getByTestId("submit-btn"); // data-testid attribute

// Still valid but less preferred
page.locator("#submit-btn");
page.locator('[data-testid="submit"]');
```

### 5. DevTools for QA

Master these panels:

- **Elements** — inspect and copy selectors, find `data-testid` attributes
- **Console** — run JS to test selectors live: `document.querySelector('.btn')`
- **Network** — watch API calls, check request/response payloads, find endpoints
- **Application** — inspect cookies, localStorage, sessionStorage
- **Accessibility tree** — find ARIA roles and accessible names for role-based locators

**Quick DevTools selector testing:**

```javascript
// In DevTools Console — test your selectors before putting them in code
document.querySelector('[data-testid="add-to-cart"]');
document.querySelectorAll(".product-card"); // how many matches?
$x('//button[text()="Checkout"]'); // test XPath
```

---

## Resources

- [MDN — CSS Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors)
- [XPath cheatsheet](https://devhints.io/xpath)
- [Playwright — Locators documentation](https://playwright.dev/docs/locators)
- [Chrome DevTools overview](https://developer.chrome.com/docs/devtools/)

---

## Deliverable

> Build a **locator catalog** — document 10 different element types from a demo site using multiple selector strategies.

Target site: [SauceDemo](https://www.saucedemo.com) or [DemoBlaze](https://www.demoblaze.com)

For each of the 10 elements, document:

- What the element is (e.g. "Login button")
- A **CSS selector** for it
- An **XPath** for it
- A **Playwright role/label locator** (where applicable)
- A brief note on which strategy is most stable and why

**Template:**

```markdown
## Element 1 — [Element description]

**CSS selector:** `[data-testid="login-button"]`  
**XPath:** `//input[@id='login-button']`  
**Playwright locator:** `page.getByRole('button', { name: 'Login' })`  
**Best strategy:** Playwright role locator — semantic, survives class/ID changes
```

Save as `deliverables/week-04-locator-catalog.md`

---

## Evaluation Criteria

- [ ] 10 elements documented with all 3 selector types each
- [ ] No two elements use the exact same strategy approach (variety shown)
- [ ] Notes explain _why_ a strategy is preferred — not just what it is
- [ ] At least 2 XPath expressions use axes (e.g. `following-sibling`, `parent`, `contains`)
- [ ] At least 2 elements use `data-testid` or ARIA-based selectors
- [ ] All selectors are verified to actually find the element (tested in DevTools console)

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your deliverable link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 4 deliverable. Please evaluate my locator catalog."_

---

_← [Week 03](./week-03-javascript-essentials.md) · Back to [README](../README.md) · Next: [Week 05 →](../phase-2-playwright/week-05-playwright-setup-first-tests.md)_
