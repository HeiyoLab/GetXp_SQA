# Week 02 — Manual Testing & Bug Reporting

**Phase:** 1 — Foundations Reset  
**Tool focus:** General  
**Estimated time:** 2–3 hours

---

## Overview

Automation engineers who skip manual testing fundamentals eventually hit a wall — they automate the wrong things, miss edge cases, and write brittle tests. This week sharpens your eye for bugs and builds a habit of clear, reproducible reporting. Great bug reports are also a strong signal in interviews.

---

## Topics

### 1. Exploratory Testing

- Testing without a script — using skill, intuition, and curiosity
- Time-boxing exploration with session-based test management (SBTM)
- Note-taking techniques during exploratory sessions
- Knowing when to script vs when to explore

### 2. Boundary Value Analysis (BVA)

- Test at and just beyond the boundaries of valid input ranges
- Example: for age field accepting 18–65, test 17, 18, 19, 64, 65, 66
- Reduces the number of test cases while maximising coverage at likely failure points

### 3. Equivalence Partitioning (EP)

- Divide inputs into partitions where the system behaves the same way
- One representative test per partition is enough
- Combine with BVA for efficient coverage

### 4. Bug Lifecycle

- States: New → Assigned → In Progress → Fixed → Retest → Closed (or Reopened)
- Know who owns each state: QA, dev, PM
- Understand severity vs priority — they are NOT the same thing
  - **Severity:** how bad is the impact on the system?
  - **Priority:** how urgently does it need to be fixed?

### 5. Writing High-Quality Bug Reports

A good bug report is a gift to the developer. It should contain:

| Field              | Description                               |
| ------------------ | ----------------------------------------- |
| Title              | Short, specific, action-oriented          |
| Environment        | OS, browser, app version, device          |
| Preconditions      | What state is the app in before the steps |
| Steps to Reproduce | Numbered, precise, minimal                |
| Expected Result    | What _should_ happen                      |
| Actual Result      | What _did_ happen                         |
| Severity           | Critical / Major / Minor / Trivial        |
| Priority           | High / Medium / Low                       |
| Attachments        | Screenshot, video, log, network HAR       |

---

## Resources

- [Atlassian — How to write a good bug report](https://www.atlassian.com/agile/project-management/defects)
- [Ministry of Testing — Exploratory Testing guide](https://www.ministryoftesting.com/)
- [BBST Bug Advocacy course](http://www.testingeducation.org/BBST/) — Bug Advocacy module
- [SauceDemo](https://www.saucedemo.com) — good target for practice
- [DemoBlaze](https://www.demoblaze.com) — more complex UI to explore

---

## Deliverable

> Execute an exploratory testing session and file **5 detailed bug reports**.

Steps:

1. Pick a demo site (SauceDemo or DemoBlaze recommended)
2. Time-box 45–60 minutes of exploratory testing
3. Document each bug using the template below
4. Save your reports as `deliverables/week-02-bug-reports.md`

**Bug Report Template:**

```markdown
## Bug #[N] — [Short descriptive title]

**Severity:** [Critical / Major / Minor / Trivial]  
**Priority:** [High / Medium / Low]  
**Environment:** [Browser, OS, app URL]

### Preconditions

[What state is the app in before you start the steps]

### Steps to Reproduce

1.
2.
3.

### Expected Result

[What should happen]

### Actual Result

[What actually happens]

### Notes / Attachments

[Screenshot path, video, additional context]
```

---

## Evaluation Criteria

- [ ] 5 bug reports submitted, each using the full template
- [ ] Each bug has clear, numbered, reproducible steps
- [ ] Expected vs actual result are distinct and specific (not "it broke")
- [ ] Severity and priority are correctly assigned and can be justified
- [ ] At least 2 bugs are non-obvious (not just "login doesn't work")
- [ ] Environment information is complete

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your deliverable link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 2 deliverable. Please evaluate my bug reports."_

---

_← [Week 01](./week-01-qa-mindset-test-strategy.md) · Back to [README](../README.md) · Next: [Week 03 →](./week-03-javascript-essentials.md)_
