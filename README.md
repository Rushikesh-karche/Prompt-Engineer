# AI-Driven Software Development Lifecycle (SDLC) Prompting

## Project Information
- **Student Name:** Rushikesh Karche
- **Topic:** AI-Driven Software Development Lifecycle (SDLC) Prompting (Topic 6 Assessment)
- **Repository:** `prompt-engineering-developer`
- **Application Target:** B2B Task Tracker REST API (Node.js/Express & Parameterized SQL)

---

## Project Objectives

1. **Systematic SDLC Decomposition:** Apply **Least-to-Most Prompting** to deconstruct an abstract B2B API vision into modular software requirements, user stories with acceptance criteria, and relational SQL schemas before writing code.
2. **Context-Rich Code Generation:** Leverage the **CRISPE Framework** (`[Capacity]`, `[Recipient]`, `[Instruction]`, `[Style]`, `[Parameters]`, `[Evaluation]`) to generate production-grade Express route controllers following DRY and SOLID principles.
3. **Automated Secure Code Reflection:** Execute reflection prompting to audit legacy code for OWASP Top 10 vulnerabilities (specifically SQL Injection via string concatenation) and refactor to parameterized SQL with bcrypt password verification.
4. **Contract-Driven Testing & Documentation:** Generate complete OpenAPI 3.0 (Swagger) YAML specifications and an automated Jest/Supertest suite asserting HTTP status codes (201 Created, 404 Not Found, 401 Unauthorized).

---

## Deliverables & Repository Structure

```
prompt-engineering-developer/
├── README.md                           # Project overview, Loom video walkthrough, and reproduction guide
├── software_requirement_document.md    # Architecture scope, 3 B2B user stories, SQL schema, and controllers
├── api_documentation.md                # Complete OpenAPI 3.0 / Swagger YAML specification
├── test_cases.md                       # QA test matrix and automated Jest/Supertest test suite
├── prompt_library.md                   # Production prompt templates (Least-to-Most, CRISPE, Reflection, Tests)
└── code_repository/                    # Full executable backend source code
    ├── schema.sql                      # Relational DDL with Primary Keys, Foreign Keys, & Constraints
    ├── app.js                          # Express application entry point and routing
    ├── db.js                           # Parameterized database query interface
    ├── package.json                    # Project dependencies and test scripts
    ├── controllers/
    │   ├── taskController.js           # Parameterized SQL controllers for POST and GET tasks
    │   └── authController.js           # Secure login controller with documented vulnerable pre-reflection snippet
    └── tests/
        └── task.test.js                # Jest/Supertest integration test suite
```

### Deliverable Manifest

| File / Directory | Purpose |
|---|---|
| [`software_requirement_document.md`](software_requirement_document.md) | Comprehensive SRD containing system architecture, 3 Least-to-Most user stories with GIVEN/WHEN/THEN acceptance criteria, raw DDL schemas, and final controller code. |
| [`api_documentation.md`](api_documentation.md) | Standardized OpenAPI 3.0 specification covering `/api/v1/login`, `/api/v1/tasks`, and `/api/v1/tasks/:id` with request schemas and 200/201/400/401/404 response blocks. |
| [`test_cases.md`](test_cases.md) | QA test matrix table mapping 9 operational test scenarios to endpoints, coupled with executable Jest/Supertest automated tests. |
| [`prompt_library.md`](prompt_library.md) | Complete prompt templates documenting Least-to-Most decomposition, SQL DDL generation, the CRISPE coding prompt, Secure Reflection auditing, and test generation. |
| [`code_repository/`](code_repository/) | Fully functional backend codebase implementing parameterized SQL queries, bcrypt hashing, and input validation. |

---

## Public Loom Walkthrough Video

Watch the complete project walkthrough and live secure reflection demonstration here:  
**[Watch the Public Loom Video](YOUR_PUBLIC_LOOM_URL)**

### Structured Video Agenda (3–4 Minutes)
- **0:00–0:45 (Introduction & SDLC Overview):** Introduction by Abhishek Nikam, overview of the B2B Task Tracker project, and explanation of Least-to-Most decomposition.
- **0:45–2:00 (Local Code & CRISPE Walkthrough):** Display the local folder structure (`code_repository/` and Markdown files). Walk through [`prompt_library.md`](prompt_library.md) explaining the CRISPE parameters (`Capacity`, `Recipient`, `Instruction`, `Style`, `Parameters`, `Evaluation`).
- **2:00–3:15 (Live ChatGPT Demo — Secure Reflection):** In a live ChatGPT tab, paste the **Secure Reflection Prompt** with the vulnerable login endpoint (`SELECT * FROM users WHERE email = '...'`). Show ChatGPT identifying the SQL injection vulnerability, explaining the exploit risk, and refactoring to parameterized SQL (`SELECT ... WHERE email = ?`).
- **3:15–3:45 (Unit Tests & Concluding Remarks):** Review the automated Jest test suite in [`test_cases.md`](test_cases.md) and [`code_repository/tests/task.test.js`](code_repository/tests/task.test.js) asserting HTTP 201, 404, and 401 statuses. Conclude the presentation.

---

## Summary of Applied SDLC Prompt Frameworks

| Stage | Framework Applied | Core Mechanism & Architectural Benefit |
|---|---|---|
| **1. Requirements** | **Least-to-Most Prompting** | Prevents architectural over-complication by breaking the system down into atomic user stories and acceptance criteria prior to code generation. |
| **2. Data Modeling** | **Schema Constraint Prompting** | Generates strict DDL enforcing referential integrity, foreign key cascades, unique constraints, and check constraints (`status IN ('OPEN', 'IN_PROGRESS', 'COMPLETED')`). |
| **3. Implementation** | **CRISPE Framework** | Grounds the LLM in senior backend engineering persona, DRY/SOLID coding standards, and raw parameterized SQL. |
| **4. Security Audit** | **Reflection Prompting** | Acts as an automated security auditor, scanning for SQL injection risks and refactoring vulnerable string concatenations into parameterized statements. |
| **5. Testing & Docs** | **Declarative Specification** | Generates OpenAPI 3.0 YAML contracts and automated integration tests validating success (201/200) and edge-case failure modes (400/401/404). |
