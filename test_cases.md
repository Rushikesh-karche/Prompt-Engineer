# Automated Test Suite & QA Test Matrix: Task Tracker REST API

**Test Suite:** Unit & Integration End-to-End API Testing  
**Framework:** Jest & Supertest  
**Author:** QA Automation Lead (Abhishek Nikam)  

---

## 1. QA Test Matrix Table

| Test ID | User Story Reference | Endpoint | HTTP Method | Test Scenario Description | Expected Status | Evaluation / Pass Criteria |
|:---:|:---:|:---:|:---:|---|:---:|---|
| **TC-01** | User Story 2: Task Creation | `/api/v1/tasks` | `POST` | Valid payload with user_id, title, description, and status | **201 Created** | Response has `success: true`, returns created task with assigned integer `id`. |
| **TC-02** | User Story 2: Task Creation | `/api/v1/tasks` | `POST` | Missing required `title` and `user_id` fields | **400 Bad Request** | Returns `success: false` and descriptive validation error message. |
| **TC-03** | User Story 2: Task Creation | `/api/v1/tasks` | `POST` | Invalid status enum (e.g., `status: "ARCHIVED"`) | **400 Bad Request** | Rejects invalid status; validates against allowed enums `[OPEN, IN_PROGRESS, COMPLETED]`. |
| **TC-04** | User Story 3: Task Retrieval | `/api/v1/tasks/:id` | `GET` | Retrieve existing task by ID (`id: 101`) | **200 OK** | Returns `success: true` and matching task record object. |
| **TC-05** | User Story 3: Task Retrieval | `/api/v1/tasks/:id` | `GET` | Retrieve non-existent task by ID (`id: 999999`) | **404 Not Found** | Returns `success: false` with `"Task with ID 999999 not found."` |
| **TC-06** | User Story 3: Task Retrieval | `/api/v1/tasks/:id` | `GET` | Non-numeric task ID parameter (`id: "abc"`) | **400 Bad Request** | Returns validation error stating ID must be a positive integer. |
| **TC-07** | User Story 1: User Login | `/api/v1/login` | `POST` | Valid corporate credentials | **200 OK** | Returns `token` and sanitized user object without password hash. |
| **TC-08** | User Story 1: User Login | `/api/v1/login` | `POST` | Invalid password attempt | **401 Unauthorized** | Returns generic authentication error. |
| **TC-09** | Security Penetration | `/api/v1/login` | `POST` | SQL injection attack string: `' OR '1'='1` | **401 Unauthorized** | Parameterized query treats input as literal string; attack fails safely. |

---

## 2. Automated Test Code (`tests/task.test.js`)

```javascript
const request = require("supertest");
const app = require("../app");

describe("B2B Task Tracker REST API - Test Suite", () => {
  // Test 1: Successful Task Creation (asserts status 201)
  describe("POST /api/v1/tasks", () => {
    it("should successfully create a new task and return HTTP 201 Created", async () => {
      const payload = {
        user_id: 1,
        title: "Deploy Production Kubernetes Cluster",
        description: "Configure Terraform scripts and deploy to us-east-1.",
        status: "OPEN",
        due_date: "2026-10-15T00:00:00.000Z"
      };

      const response = await request(app)
        .post("/api/v1/tasks")
        .send(payload)
        .expect("Content-Type", /json/)
        .expect(201);

      expect(response.body.success).toBe(true);
      expect(response.body.data).toHaveProperty("id");
      expect(response.body.data.title).toBe(payload.title);
      expect(response.body.data.status).toBe("OPEN");
    });

    it("should return HTTP 400 Bad Request when required fields are missing", async () => {
      const invalidPayload = {
        description: "Missing user_id and title"
      };

      const response = await request(app)
        .post("/api/v1/tasks")
        .send(invalidPayload)
        .expect(400);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain("Validation Error");
    });
  });

  // Test 2: Fetching Task by ID (asserts status 404 for invalid/missing ID)
  describe("GET /api/v1/tasks/:id", () => {
    it("should return HTTP 200 OK and task object for a valid existing ID", async () => {
      const response = await request(app)
        .get("/api/v1/tasks/101")
        .expect("Content-Type", /json/)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.id).toBe(101);
      expect(response.body.data.title).toBe("Quarterly Financial Audit");
    });

    it("should return HTTP 404 Not Found when querying a non-existent task ID", async () => {
      const nonExistentId = 999999;

      const response = await request(app)
        .get(`/api/v1/tasks/${nonExistentId}`)
        .expect("Content-Type", /json/)
        .expect(404);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain("not found");
    });

    it("should return HTTP 400 Bad Request when task ID is non-numeric", async () => {
      const response = await request(app)
        .get("/api/v1/tasks/abc-invalid")
        .expect(400);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain("must be a positive integer");
    });
  });

  // Test 3: Authentication Security & SQL Injection Neutralization
  describe("POST /api/v1/login", () => {
    it("should return HTTP 200 and an auth token for valid credentials", async () => {
      const validCreds = {
        email: "admin@acme.corp",
        password: "SecurePass123!"
      };

      const response = await request(app)
        .post("/api/v1/login")
        .send(validCreds)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body).toHaveProperty("token");
      expect(response.body.user.email).toBe(validCreds.email);
    });

    it("should return HTTP 401 Unauthorized for invalid password", async () => {
      const invalidCreds = {
        email: "admin@acme.corp",
        password: "WrongPassword!"
      };

      const response = await request(app)
        .post("/api/v1/login")
        .send(invalidCreds)
        .expect(401);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain("Authentication Failed");
    });

    it("should neutralize SQL injection attack payloads and return HTTP 401 Unauthorized", async () => {
      const sqlInjectionPayload = {
        email: "admin@acme.corp' OR '1'='1",
        password: "' OR '1'='1"
      };

      const response = await request(app)
        .post("/api/v1/login")
        .send(sqlInjectionPayload)
        .expect(401);

      expect(response.body.success).toBe(false);
      expect(response.body.error).toContain("Authentication Failed");
    });
  });
});
```

---

## 3. How to Run the Tests

```bash
cd code_repository
npm install
npm test
```
