# Week 16 — Reporting, Metrics & Dashboards

**Phase:** 4 — Framework Architecture  
**Tool focus:** Allure · GitHub Actions · Reporting patterns  
**Estimated time:** 3 hours

---

## Overview

Test results only create value if they reach the right people in a form they understand. This week you configure Allure dashboards for both your Playwright and Selenium suites, and write a QA metrics summary as if presenting to a non-technical manager. Communicating quality is a skill as important as measuring it.

---

## Topics

### 1. Allure Advanced Configuration

**Categories (group failures by type):**

```json
// allure-results/categories.json
[
  {
    "name": "Ignored tests",
    "matchedStatuses": ["skipped"]
  },
  {
    "name": "Product defects",
    "matchedStatuses": ["failed"],
    "messageRegex": ".*Expected.*"
  },
  {
    "name": "Test defects",
    "matchedStatuses": ["broken"],
    "messageRegex": ".*NullPointer.*|.*Timeout.*"
  }
]
```

**Environment info:**

```properties
# allure-results/environment.properties
Browser=Chrome 120
Environment=Staging
App_Version=2.4.1
Test_Run_Date=2025-05-01
```

**Executor info:**

```json
// allure-results/executor.json
{
  "name": "GitHub Actions",
  "type": "github",
  "buildName": "Run #42",
  "buildUrl": "https://github.com/your-repo/actions/runs/123"
}
```

### 2. Allure Annotations (Playwright)

```javascript
import { allure } from "allure-playwright";

test("checkout — happy path", async ({ page }) => {
  allure.epic("E-Commerce");
  allure.feature("Checkout");
  allure.story("Guest checkout");
  allure.severity("critical");
  allure.owner("Aerry");
  allure.tag("regression");
  allure.tag("smoke");

  await allure.step("Navigate to product page", async () => {
    await page.goto("/inventory.html");
  });

  await allure.step("Add item to cart", async () => {
    await page.getByRole("button", { name: "Add to cart" }).first().click();
  });

  // Attach screenshot at key step
  await allure.attachment("Cart state", await page.screenshot(), "image/png");
});
```

### 3. Metrics That Matter

Avoid vanity metrics. Focus on metrics that inform decisions:

| Metric                         | Why it matters                                                    |
| ------------------------------ | ----------------------------------------------------------------- |
| **Pass rate (%)**              | Overall health — trend over time is more useful than single value |
| **Failure rate by category**   | Product defects vs test defects vs infrastructure                 |
| **Flaky test rate**            | Tests that sometimes pass, sometimes fail — reliability debt      |
| **Test execution time**        | Slow suites → developers stop waiting → CI ignored                |
| **Coverage by feature**        | Which features have zero test coverage?                           |
| **Mean Time to Detect (MTTD)** | How quickly do tests catch a regression?                          |
| **Automation coverage %**      | Automated vs manual test cases ratio                              |

### 4. Flaky Test Management

A test is flaky if it passes and fails on the same code without any changes. Causes:

- Race conditions — element not ready
- Time dependencies — date/time assertions
- Environment pollution — shared state between tests
- External service instability — third-party API

Track flaky tests with Allure's **Retries** view. In Playwright:

```javascript
// playwright.config.js
retries: process.env.CI ? 2 : 0,
```

Tag known flaky tests:

```javascript
test('flaky payment gateway test', { annotation: { type: 'flaky', description: 'Payment gateway timeout in CI' } }, async () => {
  ...
});
```

### 5. Writing for Non-Technical Stakeholders

A QA report to management should answer:

1. **Are we safe to release?** Yes/No with confidence level
2. **What is broken right now?** 3-5 bullet points max
3. **What is the risk of proceeding?** Which user journeys are affected?
4. **What was tested?** Scope in plain English
5. **What was not tested?** Known gaps

Avoid: test counts, pass percentages in isolation, technical jargon, tool names.
Use: business feature names, user journey language, severity in business impact terms.

---

## Resources

- [Allure docs — Configuration](https://allurereport.org/docs/configuration/)
- [Allure GitHub Pages Action](https://github.com/simple-elf/allure-report-action)
- [Google Testing Blog — Flaky tests](https://testing.googleblog.com/2020/12/test-flakiness-one-of-main-challenges.html)

---

## Deliverable

> Set up Allure dashboards for your test suites and write a 1-page QA metrics summary.

**Part 1 — Allure Dashboard:**

1. Configure `categories.json` and `environment.properties` in your Playwright suite
2. Add Allure annotations (epic/feature/story/severity) to at least 5 tests
3. Generate the report and take a screenshot of: Overview, Suites, Behaviors tabs
4. Do the same for your Selenium suite (Allure + TestNG or Allure + pytest)

**Part 2 — QA Metrics Summary (`week-16-metrics-summary.md`):**
Write a 1-page summary as if presenting to a QA Manager after a sprint. Include:

- Sprint/run summary (what was tested, when, environment)
- Pass/fail breakdown with trend (make up plausible numbers if needed)
- Top 3 defects found this run
- Flaky tests identified
- Risk assessment: safe to release? What are the open risks?

Use **business language** — no tool names, no test framework jargon.

---

## Evaluation Criteria

- [ ] `categories.json` and `environment.properties` present in Allure config
- [ ] At least 5 tests have full epic/feature/story/severity annotations
- [ ] Report screenshot shows all 3 tabs: Overview, Suites, Behaviors
- [ ] Selenium suite also has Allure integrated
- [ ] `week-16-metrics-summary.md` is max 1 page
- [ ] Summary uses business language — no Playwright/Selenium/TestNG mentioned
- [ ] Summary includes a clear release recommendation with rationale

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 16 deliverable."_

---

_← [Week 15](./week-15-performance-accessibility-security.md) · Back to [README](../README.md) · Next: [Week 17 →](../phase-5-career-ready/week-17-mobile-modern-app-testing.md)_
