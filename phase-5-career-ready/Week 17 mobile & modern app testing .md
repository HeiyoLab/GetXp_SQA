# Week 17 — Mobile & Modern App Testing

**Phase:** 5 — Career Ready  
**Tool focus:** General · Appium · Strategy  
**Estimated time:** 3 hours

---

## Overview

Modern applications are rarely just "a website." They're microservices, mobile apps, third-party integrations, and event-driven backends. This week you expand your QA thinking beyond the browser and produce a test strategy that covers the full stack — the kind of thinking expected at senior level.

---

## Topics

### 1. Mobile Testing Fundamentals

**Types of mobile apps:**

- **Native** — built with Swift/Kotlin, full device access, tested with Appium/XCUITest/Espresso
- **Web** — mobile browser, same tools as desktop (Playwright supports mobile viewports)
- **Hybrid** — native shell + web content (e.g. WebView), mixed testing approach

**Appium basics:**

```javascript
// Playwright mobile emulation (good for hybrid/web)
const iPhone = devices["iPhone 13"];
const browser = await chromium.launch();
const context = await browser.newContext({ ...iPhone });
const page = await context.newPage();
await page.goto("https://your-app.com");
```

For native app testing, Appium is the standard:

```bash
npm install -g appium
appium driver install uiautomator2
```

**Mobile-specific test considerations:**

- Orientation changes (portrait/landscape)
- Network conditions (3G, offline mode)
- Interruptions (incoming call, push notification)
- Device permissions (location, camera, notifications)
- Gesture interactions (swipe, pinch, long press)
- Different screen sizes and densities

### 2. Microservices Testing Strategy

Microservices change how you test. You can no longer test the "application" as one thing:

| Layer           | Testing approach                                             |
| --------------- | ------------------------------------------------------------ |
| **Unit**        | Each service tested in isolation with mocked dependencies    |
| **Contract**    | Verify service A's API matches what service B expects (Pact) |
| **Integration** | Services communicate correctly in a shared environment       |
| **E2E**         | Full user journey across all services                        |
| **Chaos**       | What happens when service X goes down?                       |

### 3. Contract Testing with Pact

Contract testing fills the gap between unit tests and full integration tests:

```javascript
// Consumer test (the service that calls the API)
const { PactV3 } = require("@pact-foundation/pact");

const provider = new PactV3({
  consumer: "BookingUI",
  provider: "BookingAPI",
});

await provider.addInteraction({
  uponReceiving: "a request for booking 1",
  withRequest: { method: "GET", path: "/api/bookings/1" },
  willRespondWith: {
    status: 200,
    body: { id: 1, name: "Aerry", date: "2025-12-01" },
  },
});
```

### 4. Shift-Left & Shift-Right

**Shift-left** — test earlier in the development cycle:

- Write test cases during requirement grooming
- Review designs for testability
- Unit and API tests written alongside feature code
- QA involved in sprint planning, not just sprint review

**Shift-right** — test in production:

- Feature flags — test new features with a subset of real users
- A/B testing — validate business hypotheses with real traffic
- Observability — logs, metrics, traces to detect issues post-release
- Synthetic monitoring — run smoke tests against production continuously

---

## Resources

- [Appium docs](https://appium.io/docs/en/latest/)
- [Pact Foundation docs](https://docs.pact.io/)
- [Martin Fowler — Microservices Testing](https://martinfowler.com/articles/microservice-testing/)
- [Google SRE book — Testing for Reliability](https://sre.google/sre-book/testing-reliability/)

---

## Deliverable

> Write a **test strategy document for a hypothetical ride-hailing app** (think Grab/inDrive).

The app has:

- Mobile app (Android + iOS) for passengers and drivers
- REST API backend
- Real-time WebSocket for trip tracking
- Payment gateway integration (FPX, card, e-wallet)
- Admin web dashboard

Your strategy must cover:

1. **Scope** — what is in / out of scope
2. **Test layers** — unit, integration, contract, E2E, mobile
3. **Mobile testing approach** — native vs web, tools, device matrix
4. **API testing approach** — which services, what coverage
5. **Third-party integration testing** — payment gateway, maps
6. **Release criteria** — what must be green before going live
7. **Monitoring strategy** — how do you know it's working in production?
8. **Team ownership** — who owns which layer

Save as `deliverables/week-17-ridehailing-test-strategy.md`

---

## Evaluation Criteria

- [ ] All 8 sections present and substantive
- [ ] Mobile section specifies tools and justifies choices
- [ ] API section identifies which services are highest priority
- [ ] Third-party integration testing section addresses risk (what if payment gateway is down?)
- [ ] Release criteria are measurable
- [ ] Monitoring strategy goes beyond "Allure report" — includes production observability
- [ ] Strategy is realistic — not trying to test everything, prioritises by risk

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 17 deliverable."_

---

_← [Week 16](../phase-4-framework-architecture/week-16-reporting-metrics-dashboards.md) · Back to [README](../README.md) · Next: [Week 18 →](./week-18-open-source-contribution.md)_
