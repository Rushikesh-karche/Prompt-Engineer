# SDLC Prompt Engineering Library

A structured catalog of parameter-driven prompt templates engineered for the complete Software Development Lifecycle (SDLC): requirements gathering, schema architecture, controller implementation, security reflection audits, API documentation, and test automation.

---

## 1. Least-to-Most Requirements Prompt (Analysis & User Stories)

- **Framework:** Least-to-Most Cognitive Decomposition
- **Purpose:** Deconstruct an abstract software vision into modular system architecture and atomic user stories with acceptance criteria before writing code.

### Prompt Template
```markdown
Act as a Business Analyst. We want to design a backend REST API named "[Project Name]".

First, outline the system architecture scope and write 3 core B2B user stories (for User Signup/Authentication, Task Creation, and Task Status Updates) with explicit acceptance criteria (GIVEN, WHEN, THEN).

Do not write database schemas or backend code yet. Focus exclusively on clarifying business requirements and validation rules.
```

---

## 2. Relational Database SQL Schema Prompt (DDL Generation)

- **Framework:** Formal Schema Design with Constraint Grounding
- **Purpose:** Generate production-grade relational DDL SQL enforcing primary keys, foreign keys, unique constraints, and check constraints.

### Prompt Template
```markdown
Act as a Principal Database Administrator.

Based on the requirements established for [Project Name], design a relational SQL database schema in raw DDL.

Generate table definitions for:
1. `users` (id, name, email, password_hash, created_at)
2. `tasks` (id, user_id, title, description, status, due_date, created_at)

Strict Constraints:
- Implement PRIMARY KEY constraints with auto-increment/serial IDs.
- Enforce UNIQUE on user email.
- Enforce NOT NULL on essential attributes.
- Enforce a FOREIGN KEY linking `tasks.user_id` to `users.id` with `ON DELETE CASCADE`.
- Implement a CHECK constraint ensuring `tasks.status` is strictly one of ['OPEN', 'IN_PROGRESS', 'COMPLETED'].
- Add appropriate B-tree indexes for query optimization.
- Output raw DDL SQL only.
```

---

## 3. CRISPE Route Controller Prompt (Backend Implementation)

- **Framework:** CRISPE Framework (Capacity, Recipient, Instruction, Style, Parameters, Evaluation)
- **Purpose:** Direct the AI to generate production-quality backend controllers adhering to DRY/SOLID principles using raw parameterized SQL.

### Prompt Template
```markdown
[Capacity]:
You are a Senior Backend Software Engineer specializing in Node.js/Express and enterprise database security.

[Recipient]:
Your code will be integrated by the core API platform team and reviewed by a Principal Security Architect.

[Instruction]:
Write the route controller logic for two core endpoints:
1. POST /api/v1/tasks (Create a new task with validation)
2. GET /api/v1/tasks/:id (Retrieve a single task by ID with 404 handling)

[Style]:
Clean, idiomatic JavaScript/Node.js following DRY and SOLID principles. Use raw SQL with parameterized queries (no ORM). Include clear inline comments explaining execution flow and error handling.

[Parameters]:
- Framework: Node.js / Express.
- Database Interface: Parameterized SQL with '?' query placeholders.
- Status Codes: Return HTTP 201 for task creation, HTTP 200 for retrieval, HTTP 400 for validation errors, HTTP 404 for missing records, and HTTP 500 for internal errors.

[Evaluation]:
The code must be completely immune to SQL injection, handle asynchronous promises safely with try/catch blocks, and validate all incoming request parameters before query execution.
```

---

## 4. Secure Code Reflection & Vulnerability Audit Prompt

- **Framework:** Reflection & Automated Security Refactoring
- **Purpose:** Audit existing controller code for high-risk vulnerabilities (OWASP Top 10 SQL Injection, unhandled crashes) and produce a hardened refactored version.

### Prompt Template
```markdown
Act as a Security Auditor. 

Scan the code block below for security vulnerabilities (e.g., SQL Injection, unhandled crashes, credential leaks). 

Code to Audit:
"""
[Insert Vulnerable Code Snippet]
"""

Tasks:
1. Identify the specific security exploit and cite the OWASP vulnerability category.
2. Explain the risk scenario and demonstrate how an attacker could bypass authentication or extract sensitive data.
3. Write a refactored, production-hardened version of this endpoint using parameterized SQL queries, bcrypt password hashing, and comprehensive try/catch blocks.
```

---

## 5. OpenAPI / Swagger Documentation Prompt

- **Framework:** Declarative Contract Specification
- **Purpose:** Generate a complete OpenAPI 3.0 YAML specification for the API routes.

### Prompt Template
```markdown
Act as an API Architect.

Generate a comprehensive OpenAPI 3.0 / Swagger YAML documentation specification for the Task Tracker REST API endpoints:
- POST /api/v1/login
- POST /api/v1/tasks
- GET /api/v1/tasks/:id

Include:
- Detailed endpoint descriptions and tags.
- Request body schemas with data types, required fields, and realistic examples.
- Comprehensive response status blocks (200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 404 Not Found, 500 Internal Error).
- Component schemas for Task, User, and ErrorResponse.
```

---

## 6. Automated QA Unit Test Prompt

- **Framework:** Test-Driven Verification (TDD)
- **Purpose:** Generate automated unit and integration tests asserting status codes, response schemas, and attack neutralization.

### Prompt Template
```markdown
Act as a QA Automation Engineer.

Write an automated integration test suite in Jest and Supertest for the Task Tracker REST API endpoints.

Test Scenarios to Cover:
1. POST /api/v1/tasks: Successfully creates a task with valid payload (assert status 201 and verify response body properties).
2. POST /api/v1/tasks: Fails when required title or user_id is missing (assert status 400).
3. GET /api/v1/tasks/:id: Returns task object for existing ID (assert status 200).
4. GET /api/v1/tasks/:id: Returns error for non-existent ID (assert status 404).
5. POST /api/v1/login: Fails with HTTP 401 when given an SQL injection string (' OR '1'='1).

Provide executable JavaScript test code with descriptive describe() and it() blocks.
```
