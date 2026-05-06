# Week 15 — Performance, Accessibility & Security for QA

**Phase:** 4 — Framework Architecture  
**Tool focus:** Lighthouse · axe-core · Playwright accessibility  
**Estimated time:** 3 hours

---

## Overview

Quality is broader than "does it work." A complete QA engineer covers performance, accessibility, and basic security — not deeply, but enough to raise issues, add automated checks, and communicate findings to the team. These skills are increasingly listed in senior QA job descriptions.

---

## Topics

### 1. Core Web Vitals & Lighthouse

Key performance metrics:

| Metric                                | What it measures              | Good threshold |
| ------------------------------------- | ----------------------------- | -------------- |
| LCP (Largest Contentful Paint)        | Load time for main content    | < 2.5s         |
| FID / INP (Interaction to Next Paint) | Responsiveness to input       | < 200ms        |
| CLS (Cumulative Layout Shift)         | Visual stability              | < 0.1          |
| FCP (First Contentful Paint)          | Time to first visible content | < 1.8s         |
| TTFB (Time to First Byte)             | Server response speed         | < 600ms        |

Run Lighthouse via CLI:

```bash
npx lighthouse https://your-app.com --output html --output-path ./reports/lighthouse.html
```

Or programmatically:

```javascript
import lighthouse from "lighthouse";
import * as chromeLauncher from "chrome-launcher";

const chrome = await chromeLauncher.launch({ chromeFlags: ["--headless"] });
const result = await lighthouse("https://your-app.com", { port: chrome.port });
const scores = result.lhr.categories;
console.log("Performance:", scores.performance.score * 100);
console.log("Accessibility:", scores.accessibility.score * 100);
await chrome.kill();
```

### 2. Accessibility Testing with axe-core

WCAG (Web Content Accessibility Guidelines) 2.1 is the standard. Common violations:

- Missing `alt` text on images
- Form inputs without associated labels
- Insufficient colour contrast (4.5:1 minimum for text)
- Keyboard navigation broken — focus traps
- Missing ARIA landmarks

**Playwright + axe-core:**

```bash
npm install --save-dev @axe-core/playwright
```

```javascript
import { checkA11y, injectAxe } from "axe-playwright";

test("homepage has no critical accessibility violations", async ({ page }) => {
  await page.goto("/");
  await injectAxe(page);

  await checkA11y(page, null, {
    detailedReport: true,
    detailedReportOptions: { html: true },
    axeOptions: {
      runOnly: {
        type: "tag",
        values: ["wcag2a", "wcag2aa"],
      },
    },
  });
});
```

### 3. OWASP Top 10 — What QA Should Know

You don't need to be a security engineer, but know these exist and write test cases for them:

| Risk                          | What to test as QA                                                                     |
| ----------------------------- | -------------------------------------------------------------------------------------- |
| **Broken Access Control**     | Can user A access user B's data? Can a regular user hit admin endpoints?               |
| **Injection (SQL/XSS)**       | Input fields accept `<script>alert(1)</script>` — does the app sanitise?               |
| **Security Misconfiguration** | Are sensitive headers present? (`X-Content-Type-Options`, `Strict-Transport-Security`) |
| **Sensitive Data Exposure**   | Does the API return passwords, tokens, PII in responses it shouldn't?                  |
| **Broken Authentication**     | Can you brute-force login? Does logout invalidate the token?                           |

### 4. Security Header Check in Playwright

```javascript
test("security headers are present", async ({ request }) => {
  const response = await request.get("/");

  const headers = response.headers();
  expect(headers["x-content-type-options"]).toBe("nosniff");
  expect(headers["x-frame-options"]).toBeDefined();
  expect(headers["strict-transport-security"]).toBeDefined();
});
```

### 5. Basic XSS Test

```javascript
const xssPayloads = [
  "<script>alert(1)</script>",
  '"><img src=x onerror=alert(1)>',
  "'; DROP TABLE users; --",
];

for (const payload of xssPayloads) {
  test(`input field sanitises: ${payload.slice(0, 20)}`, async ({ page }) => {
    await page.goto("/search");
    await page.getByRole("searchbox").fill(payload);
    await page.keyboard.press("Enter");

    // Assert no alert dialog appeared (would indicate XSS)
    page.on("dialog", async (dialog) => {
      await dialog.dismiss();
      throw new Error(
        `XSS vulnerability: dialog appeared with message: ${dialog.message()}`,
      );
    });

    // Assert payload is not reflected raw in the DOM
    const content = await page.content();
    expect(content).not.toContain("<script>alert(1)</script>");
  });
}
```

---

## Resources

- [web.dev — Core Web Vitals](https://web.dev/vitals/)
- [Lighthouse docs](https://developer.chrome.com/docs/lighthouse/overview/)
- [axe-core rules](https://dequeuniversity.com/rules/axe/4.6/)
- [WCAG 2.1 quick reference](https://www.w3.org/WAI/WCAG21/quickref/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

---

## Deliverable

> Run a **Lighthouse audit + axe scan** on a demo site and write a findings report. Add 2 automated accessibility tests in Playwright.

**Part 1 — Audit Report (`week-15-audit-report.md`):**

1. Run Lighthouse on [SauceDemo](https://www.saucedemo.com) or your own Jitu app
2. Screenshot the four score dials (Performance, Accessibility, Best Practices, SEO)
3. Document at least **3 actionable findings** with: issue, severity, recommended fix
4. Run `axe-playwright` on the same page — document violations found

**Part 2 — Automated Accessibility Tests:**

1. `test: homepage has no critical a11y violations` — uses axe-core
2. `test: security headers are present` — checks at least 3 headers

---

## Evaluation Criteria

- [ ] Lighthouse report produced with scores for all 4 categories
- [ ] At least 3 findings documented with severity and actionable fix
- [ ] axe scan results included in report
- [ ] 2 automated tests added to Playwright suite
- [ ] a11y test uses WCAG 2.1 AA tag filter
- [ ] Security header test checks at least 3 headers
- [ ] Report is clearly structured — a manager could read and understand it

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 15 deliverable."_

---

_← [Week 14](./week-14-api-testing-deep-dive.md) · Back to [README](../README.md) · Next: [Week 16 →](./week-16-reporting-metrics-dashboards.md)_
