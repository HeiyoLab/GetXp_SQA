# Week 14 — API Testing Deep Dive

**Phase:** 4 — Framework Architecture  
**Tool focus:** Playwright `request` context · Axios · REST Assured  
**Estimated time:** 3 hours

---

## Overview

UI tests are slow, brittle, and expensive. For business logic validation, API tests are faster, more reliable, and closer to the real behaviour. This week you build a full API test suite covering authentication, CRUD, schema validation, and error handling — skills that most automation engineers don't develop deeply enough.

---

## Topics

### 1. REST API Fundamentals for Testers

What to test in every API:

| Concern            | What to assert                                                |
| ------------------ | ------------------------------------------------------------- |
| **Status code**    | Correct code for the operation (200, 201, 400, 401, 404, 500) |
| **Response body**  | Expected fields present, correct types, correct values        |
| **Schema**         | Response shape matches contract                               |
| **Headers**        | `Content-Type`, `Authorization`, cache headers                |
| **Response time**  | Under SLA threshold                                           |
| **Error messages** | Meaningful, consistent, no stack traces in production         |
| **Idempotency**    | Repeated requests behave correctly                            |

### 2. Auth Flows

```javascript
// Bearer token auth
const response = await request.post("/api/auth/login", {
  data: { username: "admin", password: "password123" },
});
const { token } = await response.json();

// Use token in subsequent requests
const users = await request.get("/api/users", {
  headers: { Authorization: `Bearer ${token}` },
});
```

### 3. JSON Schema Validation

```javascript
// Using zod for schema validation in Playwright/Axios
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string().min(1),
  email: z.string().email(),
  address: z
    .object({
      city: z.string(),
    })
    .optional(),
});

const body = await response.json();
const result = UserSchema.safeParse(body);
expect(result.success).toBeTruthy();
```

Or use `toMatchObject` in Playwright for lighter validation:

```javascript
expect(body).toMatchObject({
  id: expect.any(Number),
  name: expect.stringMatching(/\w+/),
  email: expect.stringContaining("@"),
});
```

### 4. CRUD Test Pattern

```javascript
let createdId;

test("POST /bookings — creates a booking", async ({ request }) => {
  const response = await request.post("/api/bookings", {
    data: { firstname: "Aerry", lastname: "Asmani", totalprice: 200 },
  });
  expect(response.status()).toBe(201);
  const body = await response.json();
  expect(body.bookingid).toBeDefined();
  createdId = body.bookingid;
});

test("GET /bookings/:id — returns created booking", async ({ request }) => {
  const response = await request.get(`/api/bookings/${createdId}`);
  expect(response.status()).toBe(200);
  expect((await response.json()).firstname).toBe("Aerry");
});

test("PUT /bookings/:id — updates the booking", async ({ request }) => {
  const response = await request.put(`/api/bookings/${createdId}`, {
    data: { firstname: "Updated", lastname: "Asmani", totalprice: 300 },
  });
  expect(response.status()).toBe(200);
});

test("DELETE /bookings/:id — removes the booking", async ({ request }) => {
  const response = await request.delete(`/api/bookings/${createdId}`);
  expect(response.status()).toBe(201);
});
```

### 5. Negative Testing & Edge Cases

Don't just test the happy path:

- Missing required fields → 400 Bad Request
- Invalid token → 401 Unauthorized
- Non-existent resource → 404 Not Found
- Malformed JSON body → 400 or 422
- Extremely large payloads → validate response (don't let server explode)
- SQL injection strings in text fields → should return 400, not 500

---

## Resources

- [Restful Booker API](https://restful-booker.herokuapp.com/apidoc/)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/)
- [Petstore API](https://petstore.swagger.io/)
- [Playwright docs — API Testing](https://playwright.dev/docs/api-testing)
- [Zod — schema validation](https://zod.dev/)

---

## Deliverable

> Build a full API test suite for [Restful Booker](https://restful-booker.herokuapp.com/).

Required test coverage:

1. **Auth** — login returns a token; invalid credentials return 200 with no token (this API's quirk — document it)
2. **GET all bookings** — status 200, response is an array, each item has `bookingid`
3. **GET booking by ID** — status 200, correct schema, response time < 2000ms
4. **POST booking** — status 201 (or 200), returned `bookingid` is a number, body reflects sent data
5. **PUT booking** — status 200, response body contains updated values
6. **DELETE booking** — status 201 (API quirk), subsequent GET returns 404
7. **Negative: GET non-existent ID** — 404 response
8. **Negative: POST missing required field** — non-200 response or error in body

Schema validation on at least 2 responses.

---

## Evaluation Criteria

- [ ] All 8 test scenarios covered
- [ ] Schema validation on at least 2 endpoints
- [ ] Response time assertion on at least 1 test
- [ ] Negative tests assert meaningful error responses
- [ ] Auth token stored once and reused — not re-requested each test
- [ ] Tests are independent (CRUD test deletes what it creates)
- [ ] `README` in deliverable folder documents the API quirks found

---

## Submission

Update [`PROGRESS.md`](../PROGRESS.md) and ask Claude: _"I am ready to submit my Week 14 deliverable."_

---

_← [Week 13](./week-13-framework-design.md) · Back to [README](../README.md) · Next: [Week 15 →](./week-15-performance-accessibility-security.md)_
