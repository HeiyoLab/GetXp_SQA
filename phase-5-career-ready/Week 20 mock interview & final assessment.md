# Week 20 — Mock Interview & Final Assessment

**Phase:** 5 — Career Ready  
**Tool focus:** Both  
**Estimated time:** 3–4 hours

---

## Overview

The final week is an evaluation — not just of your technical skills, but of your ability to explain them clearly under pressure. You'll complete a test case design challenge, explain your framework architecture, and go through a mock interview with Claude. By the end, you'll know exactly where you're strong and where to keep practising.

---

## Topics

### 1. Common QA Automation Interview Questions

Be ready to answer these fluently:

**Fundamentals:**

- What is the difference between a test plan and a test strategy?
- Explain the test pyramid. Where does each type of test fit?
- What makes a good test case? What makes a bad one?
- What is the difference between severity and priority?
- What is regression testing and when do you do it?

**Automation:**

- Why did you choose Playwright over Selenium (or vice versa) for a given project?
- What is the Page Object Model and what problem does it solve?
- How do you handle dynamic elements / timing issues in automation?
- What is flaky test and how do you debug one?
- How do you ensure test independence? Why does it matter?
- What is a fixture in Playwright/pytest? How is it different from `beforeEach`?
- How does `storageState` work in Playwright?

**CI/CD:**

- Walk me through your GitHub Actions pipeline.
- How do you manage secrets in CI?
- What is parallel test execution and what are the trade-offs?
- How do you decide what runs in CI vs locally?

**Framework & Architecture:**

- How do you structure a test framework from scratch?
- How do you handle test data management?
- What is a hybrid test framework?
- How do you handle environment switching in your framework?

**Behavioural (STAR format):**

- Tell me about a bug you found that was particularly hard to reproduce.
- Tell me about a time your automation caught a production issue before release.
- Tell me about a disagreement with a developer about a defect. How did you handle it?
- What would you do if you joined a team with zero automation and a 3-month deadline?

### 2. Live Coding Test Patterns

Common QA coding challenges:

**Test case design:**

> "Write test cases for a login form."

- Think: positive, negative, boundary, edge cases
- Think: functional (wrong credentials), non-functional (what if service is down?), security (SQL injection)
- Think: UX (what does the error message say?)

**Framework question:**

> "How would you automate adding a product to a cart?"

- Walk through: locator strategy, wait strategy, assertion strategy
- Mention: what could go wrong (race condition, stale element), how you'd handle it

**Debugging question:**

> "This test is failing intermittently. What do you do?"

- Step 1: Run it 10 times and confirm flakiness
- Step 2: Look at screenshots/traces from failed runs
- Step 3: Check wait strategy — is there a timing issue?
- Step 4: Check test isolation — is there shared state?
- Step 5: Check environment — CI vs local difference?

### 3. Architecture Explanation Template

When asked "walk me through your test framework":

1. **Start with the goal:** "Our goal was to have a maintainable suite that any team member could extend."
2. **Explain the structure:** Folder by feature/layer, page objects, fixtures, helpers
3. **Explain key decisions:** Why POM, why fixtures over `beforeEach`, why external test data
4. **Explain CI integration:** What runs on push, what runs on PR, what runs nightly
5. **Explain trade-offs:** What you'd change if you had more time

### 4. Salary Negotiation for QA Automation (Malaysia)

Market benchmarks (Klang Valley, 2025):

- Junior QA (0–2 yrs): RM 2,500 – RM 4,000
- Mid QA with automation (3–5 yrs): RM 4,500 – RM 7,500
- Senior QA Automation (5+ yrs): RM 7,000 – RM 12,000
- Lead / Principal QA: RM 10,000 – RM 18,000+

Your differentiators for negotiation:

- Dual Playwright + Selenium proficiency
- Open source Playwright contributions
- CI/CD pipeline experience
- Framework design and architecture

Tips:

- Never give the first number — ask what their budget is
- Always negotiate from market data, not personal need
- Counter with a range, not a fixed number
- Factor in: EPF, medical, annual leave, WFH policy

---

## Deliverable

> Complete the **final assessment** below and submit for evaluation.

**Part 1 — Test Case Design Challenge:**

Write test cases for the following scenario:

> _A "Forgot Password" flow for a web application. The user enters their email address, receives a reset link, and uses it to set a new password._

Cover:

- Functional test cases (happy path + negative)
- Boundary/edge cases
- Security considerations
- At least 1 automation scenario (describe how you'd automate it, not code)

**Part 2 — Architecture Explanation:**

Write a 200–300 word explanation of your test framework architecture (from Week 13) as if explaining it in a job interview. Answer:

- What pattern did you use and why?
- How do you add a new test?
- How do you handle environments?
- What would you improve with more time?

**Part 3 — Reflection:**

Write a `FINAL-REFLECTION.md`:

- What are your 3 strongest skills after this roadmap?
- What are 2 areas you still want to improve?
- What is 1 thing that surprised you during this journey?
- What is your next step after completing this roadmap?

---

## Evaluation Criteria

- [ ] Test case design covers all 4 required areas (functional, negative, boundary, security)
- [ ] Test cases are in a standard format with steps and expected results
- [ ] Architecture explanation is fluent and covers all 4 questions
- [ ] No jargon without explanation — assumes a semi-technical interviewer
- [ ] `FINAL-REFLECTION.md` is honest and specific — not vague platitudes
- [ ] You can answer follow-up questions about any of your deliverables from Weeks 1–19

---

## Congratulations

Completing this roadmap means you have:

- A working Playwright suite with CI/CD on GitHub
- A working Selenium suite with cross-browser support
- A full API test suite
- An architecture document you can discuss in interviews
- A Lighthouse/accessibility audit on your record
- An open source contribution
- 3 polished portfolio projects
- A practiced answer to every common QA interview question

You are career-ready. Go get the role. 🎯

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready for my Week 20 final assessment. Let's do the mock interview."_

---

_← [Week 19](./week-19-portfolio-github.md) · Back to [README](../README.md)_
