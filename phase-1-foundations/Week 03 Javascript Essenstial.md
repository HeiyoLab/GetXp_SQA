# Week 03 — JavaScript Essentials for QA

**Phase:** 1 — Foundations Reset  
**Tool focus:** General (JavaScript / Node.js)  
**Estimated time:** 3 hours

---

## Overview

Playwright is JavaScript-first. To write maintainable tests and understand what your code is doing, you need a solid grip on modern JS fundamentals. This week focuses on the specific JS concepts that come up constantly in QA automation — not a full JS course, just the 20% that covers 80% of test code.

---

## Topics

### 1. async/await & Promises

- Why async matters: browser interactions, API calls, file I/O are all async
- The Promise model: pending → fulfilled / rejected
- `async function` and `await` keywords
- Error handling with `try/catch`

```javascript
// Example: awaiting a fetch call
async function getUser(id) {
  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/users/${id}`,
    );
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Failed to fetch user:", error);
  }
}
```

### 2. Array Methods

These appear constantly in test utilities and data handling:

| Method                 | Use case                          |
| ---------------------- | --------------------------------- |
| `.map()`               | Transform every item in an array  |
| `.filter()`            | Keep items that match a condition |
| `.find()`              | Get the first item that matches   |
| `.reduce()`            | Aggregate into a single value     |
| `.forEach()`           | Iterate with side effects         |
| `.some()` / `.every()` | Check if any/all items match      |

### 3. JSON Parsing & Validation

- `JSON.parse()` and `JSON.stringify()`
- Accessing nested properties safely
- Optional chaining (`?.`) and nullish coalescing (`??`)

```javascript
const data = JSON.parse(responseText);
const city = data?.address?.city ?? "Unknown";
```

### 4. ES6+ Syntax

- Destructuring: `const { name, email } = user`
- Spread operator: `const merged = { ...defaults, ...overrides }`
- Template literals: `` `Hello, ${name}!` ``
- Arrow functions: `const double = x => x * 2`
- `const` vs `let` — know when to use each

### 5. Basic Node.js & npm

- Running scripts: `node script.js`
- `package.json` — what it is and how to read it
- Installing packages: `npm install`, `npm install --save-dev`
- Writing and importing modules with `require` / `import`
- Environment variables via `process.env`

---

## Resources

- [javascript.info](https://javascript.info/) — best free JS reference, focus on: Promises, async/await, destructuring
- [Node.js official docs — Getting Started](https://nodejs.org/en/docs/guides/getting-started-guide)
- [MDN Web Docs — Array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) — free fake REST API for practice

---

## Deliverable

> Write a **Node.js validation script** that fetches a public API, validates the response, and reports pass/fail per field.

Requirements:

1. Use `fetch` (or `axios`) to call `https://jsonplaceholder.typicode.com/users/1`
2. Validate at least **5 fields** from the response (e.g. `id`, `name`, `email`, `phone`, `address.city`)
3. For each field, log `[PASS]` or `[FAIL]` with a reason
4. Handle the case where a field is `null`, `undefined`, or the wrong type
5. Use `async/await` — no raw `.then()` chains

**Expected output format:**

```
[PASS] id: 1 (type: number)
[PASS] name: "Leanne Graham" (type: string, non-empty)
[FAIL] website: expected string, got undefined
[PASS] email: "Sincere@april.biz" (contains @)
[PASS] address.city: "Gwenborough" (type: string, non-empty)
```

Save as `deliverables/week-03-api-validator.js`

---

## Evaluation Criteria

- [ ] Script runs with `node week-03-api-validator.js` — zero errors
- [ ] Validates at least 5 distinct fields
- [ ] Each field check logs `[PASS]` or `[FAIL]` with a descriptive reason
- [ ] Handles `null` / `undefined` values without crashing
- [ ] Uses `async/await` syntax correctly
- [ ] At least one check validates a **nested property** (e.g. `address.city`)
- [ ] Code is readable — variable names make the intent clear

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) with your deliverable link and reflection.

Ask Claude to evaluate: _"I am ready to submit my Week 3 deliverable. Please evaluate my Node.js validation script."_

---

_← [Week 02](./week-02-manual-testing-bug-reporting.md) · Back to [README](../README.md) · Next: [Week 04 →](./week-04-dom-locators-devtools.md)_
