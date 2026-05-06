# Week 19 — Portfolio & GitHub Optimization

**Phase:** 5 — Career Ready  
**Tool focus:** GitHub · LinkedIn  
**Estimated time:** 3 hours

---

## Overview

Everything you've built over 18 weeks needs to be visible and legible to a hiring manager who spends 90 seconds on your profile. This week you package your work for maximum impact — polished READMEs, live CI badges, demo GIFs, and a GitHub profile that shows you're the kind of engineer who cares about craft.

---

## Topics

### 1. GitHub Profile README

Create a `README.md` in a repo named exactly the same as your username (`aerryasmani/aerryasmani`):

```markdown
# Aerry Asmani — QA Automation Engineer

> Building reliable test systems with Playwright and Selenium

**Specialised in:** Test automation architecture · CI/CD pipelines · Open source contributions

## Tech Stack

![Playwright](https://img.shields.io/badge/Playwright-JS-45ba4b?logo=playwright)
![Selenium](https://img.shields.io/badge/Selenium-WebDriver-43B02A?logo=selenium)
![GitHub Actions](https://img.shields.io/badge/CI-GitHub_Actions-2088FF?logo=github-actions)
![Allure](https://img.shields.io/badge/Reports-Allure-orange)

## Featured Projects

| Project                         | Description                     | Stack              |
| ------------------------------- | ------------------------------- | ------------------ |
| [QA_SauceDemo_Playwright](link) | E2E suite with POM, CI, Allure  | Playwright · GHA   |
| [QA_SauceDemo_Selenium](link)   | Cross-browser suite, Grid ready | Selenium · TestNG  |
| [QA_RestfulBooker_API](link)    | Full CRUD API test suite        | Playwright request |

## This Week

_Currently working on: [Jitu — QA test case generation tool](https://jitu.one)_
```

### 2. Project README Structure

Every portfolio repo should have:

````markdown
# Project Name

![Status Badge](CI badge URL)

One-sentence description of what this tests and why.

## What's Tested

- List of test scenarios (business language, not code)

## Tech Stack

- Playwright 1.x / Selenium 4.x
- Node.js / Java / Python
- GitHub Actions
- Allure Reports

## Running Locally

### Prerequisites

- Node.js >= 20
- (Any other requirements)

### Setup

```bash
git clone https://github.com/your-repo
cd your-repo
npm install
npx playwright install
cp .env.example .env  # fill in your values
```
````

### Run all tests

```bash
npx playwright test
```

### Run smoke tests only

```bash
npx playwright test --grep @smoke
```

### View report

```bash
npx allure serve allure-results
```

## CI/CD

Tests run automatically on push to `main` and on all pull requests.
[View latest run](link to GitHub Actions)

## Project Structure

[Brief description of folder structure]

````

### 3. Demo GIFs

A GIF showing your tests running is worth a thousand words on a portfolio. Tools:
- **LICEcap** (Windows) — free, lightweight screen recorder → GIF
- **Kap** (Mac) — similar
- **ScreenToGif** (Windows) — more control

Tips for a good demo GIF:
- Keep it under 30 seconds
- Run in headed mode at normal speed
- Focus on 1–2 key test flows
- Keep terminal/report visible alongside browser

Add to README:
```markdown
## Demo

![Test Run Demo](demo.gif)
````

### 4. LinkedIn QA Automation Keywords

Recruiters search these terms — make sure your profile contains them naturally:

**Must have:** Playwright · Selenium WebDriver · Test Automation · CI/CD · GitHub Actions · Page Object Model · API Testing · Allure · Agile · SDLC

**Strong additions:** JavaScript · Python · Java · TestNG · pytest · Docker · REST API · Cross-browser testing · Performance testing · Accessibility testing

**Malaysian market specific:** Manual Testing · QA Engineer · SQA · UAT · Regression Testing

**For senior roles:** Test Architecture · Framework Design · Shift-left testing · Contract Testing

### 5. LinkedIn Profile Checklist

- [ ] Headline: "QA Automation Engineer | Playwright · Selenium · CI/CD" (not just job title)
- [ ] About section: 3–4 sentences on what you do, what you care about, what you've built
- [ ] Featured section: Pin your best GitHub repo or Allure demo
- [ ] Skills section: 15+ skills, QA ones at the top
- [ ] Each role: 3–4 bullets using STAR format with metrics where possible
- [ ] Open to work banner: on if actively looking

---

## Resources

- [GitHub Profile README generator](https://rahuldkjain.github.io/gh-profile-readme-generator/)
- [Shields.io — GitHub badges](https://shields.io/)
- [LICEcap — screen recorder](https://www.cockos.com/licecap/)

---

## Deliverable

> Publish **3 portfolio projects** to GitHub with full documentation.

Required for each project:

1. `README.md` using the template above
2. CI badge from GitHub Actions (passing)
3. A `demo.gif` showing tests running (headed, 15–30 seconds)
4. `.env.example` with documented variables
5. All tests passing in CI

The 3 projects:

1. **Playwright E2E suite** — from Phase 2 (SauceDemo or equivalent)
2. **Selenium suite** — from Phase 3 (same app, cross-browser)
3. **API test suite** — from Week 14 (Restful Booker)

Also update:

- GitHub profile README (create `your-username/your-username` repo)
- LinkedIn profile using keyword checklist above

---

## Evaluation Criteria

- [ ] 3 repos published with complete READMEs
- [ ] All 3 repos have passing CI badges
- [ ] Demo GIFs present and show tests actually running
- [ ] `.env.example` present and documents every required variable
- [ ] GitHub profile README created — features the 3 projects
- [ ] LinkedIn profile has at least 10 QA automation keywords
- [ ] LinkedIn headline is descriptive (not just job title)

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 19 deliverable."_

---

_← [Week 18](./week-18-open-source-contribution.md) · Back to [README](../README.md) · Next: [Week 20 →](./week-20-mock-interview-assessment.md)_
