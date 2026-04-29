# Week 01 — QA Mindset & Test Strategy

**Phase:** 1 — Foundations Reset  
**Tool focus:** General  
**Estimated time:** 2–3 hours

---

## Overview

Before writing a single line of automation, you need to think like a QA engineer. This week is about understanding _why_ we test, _what_ we should test, and _how_ to communicate that plan. These fundamentals are what interviewers check first — and what separates testers from automation engineers.

---

## Topics

### 1. SDLC & STLC

- Understand where testing fits in the Software Development Life Cycle
- Know the stages of the Software Testing Life Cycle: planning → analysis → design → execution → closure
- Understand the difference between verification and validation

### 2. Test Levels & Types

- **Unit** — tests for individual functions/methods (usually done by devs)
- **Integration** — tests for how modules interact
- **System** — end-to-end tests across the full application
- **Acceptance (UAT)** — validates the business requirement is met
- Know the difference between: functional, non-functional, regression, smoke, sanity, exploratory

### 3. Risk-Based Testing

- Not everything can be tested — learn to prioritise by impact × probability
- Identify high-risk areas: frequently changed code, business-critical flows, third-party integrations
- Document your risk assessment clearly

### 4. Test Planning Basics

- What goes into a test plan: scope, approach, resources, schedule, entry/exit criteria
- The difference between a test plan and a test strategy
- How to write measurable exit criteria (not just "all tests pass")

### 5. Writing Good Test Cases

- Structure: Test ID, title, preconditions, steps, expected result
- Characteristics of a good test case: atomic, repeatable, independent, clear
- Positive, negative, boundary, and edge cases — know the difference

---

## Resources

- [ISTQB Foundation Level Syllabus](https://www.istqb.org/certifications/certified-tester-foundation-level) — skim Chapter 1–2
- [Guru99 — Software Testing Introduction](https://www.guru99.com/software-testing-introduction-importance.html)
- [Google Testing Blog](https://testing.googleblog.com/) — any introductory post
- [Ministry of Testing — Test Strategy vs Test Plan](https://www.ministryoftesting.com/)

---

## Deliverable

> Write a **Test Strategy document** for a simple demo application.

Use any of these as your target app:

- [SauceDemo](https://www.saucedemo.com) — e-commerce login/cart app
- [DemoBlaze](https://www.demoblaze.com) — product catalogue
- [Restful Booker](https://restful-booker.herokuapp.com) — hotel booking

Your document should cover:

1. **Scope** — what features are in scope / out of scope
2. **Test approach** — what test levels and types will be used and why
3. **Risk areas** — at least 3 identified risks with likelihood and impact
4. **Test types** — functional, regression, smoke, etc.
5. **Entry criteria** — what needs to be true before testing starts
6. **Exit criteria** — what measurable conditions mark testing as complete
7. **Tools** — what tools you plan to use

**Format:** Markdown file saved as `deliverables/week-01-test-strategy.md` in this repo.

---

## Evaluation Criteria

- [ ] Document covers all 7 sections above
- [ ] Uses correct QA terminology throughout (no mixing up terms)
- [ ] Identifies at least 3 distinct risk areas
- [ ] Exit criteria are measurable — not vague (e.g. "95% pass rate" not "all tests pass")
- [ ] Scope is realistic and clearly bounded
- [ ] Written clearly enough for a non-QA stakeholder to understand

---

## Submission

When ready, update [`PROGRESS.md`](../PROGRESS.md) with:

- Link to your deliverable
- Brief reflection: what was easy, what was hard, what surprised you

Then ask Claude to evaluate: _"I am ready to submit my Week 1 deliverable. Please evaluate my test strategy document."_

---

_← Back to [README](../README.md) · Next: [Week 02 →](./week-02-manual-testing-bug-reporting.md)_
