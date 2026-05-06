# Week 18 — Open Source Contribution

**Phase:** 5 — Career Ready  
**Tool focus:** Playwright · GitHub  
**Estimated time:** 4 hours

---

## Overview

An open source contribution is proof of skill that no CV bullet point can match. It shows you can read unfamiliar codebases, communicate with strangers professionally, and contribute something valuable. Given your existing Playwright experience, the Playwright repo or a Playwright-adjacent project is the ideal target.

---

## Topics

### 1. Reading an OSS Codebase

Before contributing, understand the project:

1. **Read `CONTRIBUTING.md`** — every serious OSS project has one
2. **Read `DEVELOPMENT.md`** or setup instructions
3. **Explore the folder structure** — understand what lives where
4. **Run the existing tests** — make sure your setup works before touching anything
5. **Read recent PRs** — learn the team's standards and tone

### 2. Finding Good First Issues

Where to look:

- GitHub label `good first issue` or `help wanted`
- Documentation improvements — always welcome
- Failing tests that need fixing
- Test coverage for reported bugs
- Typos, clarity improvements in README

```
https://github.com/microsoft/playwright/issues?q=is%3Aopen+label%3A%22help+wanted%22
https://github.com/microsoft/playwright/issues?q=is%3Aopen+label%3A%22good+first+issue%22
```

Other good Playwright-adjacent targets:

- [playwright-testing-library](https://github.com/testing-library/playwright-testing-library)
- [axe-playwright](https://github.com/abhinaba-ghosh/axe-playwright)
- [allure-playwright](https://github.com/allure-framework/allure-js)
- [expect-playwright](https://github.com/playwright-community/expect-playwright)

### 3. The PR Process

A clean PR:

1. Fork the repo
2. Create a branch from `main`: `git checkout -b fix/typo-in-getting-started`
3. Make your change — focused and minimal
4. Run existing tests — make sure nothing breaks
5. Write a clear PR description:
   - What problem does this solve?
   - What changed?
   - How to test the change?
   - Screenshots if UI changed

**PR description template:**

```markdown
## Summary

Brief description of what this PR does.

## Problem

What issue does this fix? Link to the issue: Fixes #1234

## Changes

- Changed X to Y because Z
- Added test for edge case W

## Testing

- [ ] Existing tests pass
- [ ] Added new test for the change
- [ ] Manual testing done on: [OS, browser version]
```

### 4. Handling Review Feedback

OSS maintainers give direct feedback — don't take it personally:

- Respond to every comment, even if just "Fixed" or "Agreed, updated"
- If you disagree, explain your reasoning calmly with evidence
- Make requested changes in the same branch — the PR updates automatically
- Don't squash commits until the maintainer asks you to
- Thank reviewers for their time

### 5. Types of Contribution (in order of difficulty)

| Type                      | Difficulty  | Value                            |
| ------------------------- | ----------- | -------------------------------- |
| Typo/grammar fix          | Easy        | Low, but builds familiarity      |
| Documentation improvement | Easy-Medium | High — often very needed         |
| Test for an existing bug  | Medium      | High — directly useful           |
| Bug fix                   | Medium-Hard | High                             |
| New feature               | Hard        | Very high, requires deep context |
| Performance improvement   | Hard        | High                             |

---

## Resources

- [GitHub — How to contribute to open source](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)
- [First Contributions](https://github.com/firstcontributions/first-contributions)
- [Playwright GitHub](https://github.com/microsoft/playwright)
- [Open Source Guide](https://opensource.guide/how-to-contribute/)

---

## Deliverable

> Submit a genuine contribution to an open-source QA or testing project.

The contribution must be:

- Real — submitted as an actual PR to a public repo
- Useful — not a trivial one-character change
- Clean — passes existing CI, follows project conventions

Acceptable contribution types:

- A failing test that reproduces a reported bug
- Documentation fix or addition
- New test case for an untested feature or edge case
- Small bug fix with an accompanying test

Save in `deliverables/week-18-oss-contribution.md`:

- Link to your PR
- Which project you contributed to and why you chose it
- What the contribution does
- What you learned from the codebase or review process

---

## Evaluation Criteria

- [ ] PR is submitted (link provided) — not just a draft
- [ ] Contribution is substantive — not a single character typo fix
- [ ] PR passes the project's CI pipeline
- [ ] PR description is clear, professional, and follows project template
- [ ] `week-18-oss-contribution.md` reflects genuinely on the process
- [ ] You can explain the code change in a conversation

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 18 deliverable."_

---

_← [Week 17](./week-17-mobile-modern-app-testing.md) · Back to [README](../README.md) · Next: [Week 19 →](./week-19-portfolio-github.md)_
