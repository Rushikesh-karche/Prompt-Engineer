# B2B Task Tracker REST API — OpenAPI / Swagger Specification

**API Version:** 1.0.0  
**Specification:** OpenAPI 3.0.3  
**Base URL:** `https://api.tasktracker.io/api/v1`  
**Contact:** engineering@tasktracker.io  

---

## 1. Interactive Swagger / OpenAPI Specification (YAML)

```yaml
openapi: 3.0.3
info:
  title: Task Tracker REST API
  description: Production-grade B2B Task and Status Management API built using AI-Driven SDLC Prompting.
  version: 1.0.0
servers:
  - url: https://api.tasktracker.io/api/v1
    description: Production Server
  - url: http://localhost:3000/api/v1
    description: Local Development Server

paths:
  /login:
    post:
      summary: Authenticate User and Obtain Token
      description: Validates corporate email and password hash via parameterized SQL; issues an authentication bearer token.
      tags:
        - Authentication
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Authentication successful
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginSuccessResponse'
        '400':
          description: Validation Error (Missing email or password)
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '401':
          description: Authentication Failed (Invalid credentials)
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '500':
          description: Internal Server Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

  /tasks:
    post:
      summary: Create a New Task
      description: Inserts a new task associated with an authorized user, enforcing relational integrity.
      tags:
        - Tasks
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateTaskRequest'
      responses:
        '201':
          description: Task created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CreateTaskResponse'
        '400':
          description: Validation Error (Missing user_id, title, or invalid status enum)
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '500':
          description: Internal Server Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

  /tasks/{id}:
    get:
      summary: Retrieve Task by ID
      description: Fetches full task metadata and current lifecycle status for a specific task ID.
      tags:
        - Tasks
      parameters:
        - name: id
          in: path
          required: true
          description: Positive integer primary key of the task
          schema:
            type: integer
            example: 101
      responses:
        '200':
          description: Task retrieved successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GetTaskResponse'
        '400':
          description: Validation Error (Non-numeric or non-positive ID)
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '404':
          description: Task Not Found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '500':
          description: Internal Server Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

components:
  schemas:
    LoginRequest:
      type: object
      required:
        - email
        - password
      properties:
        email:
          type: string
          format: email
          example: admin@acme.corp
        password:
          type: string
          format: password
          example: SecurePass123!

    LoginSuccessResponse:
      type: object
      properties:
        success:
          type: boolean
          example: true
        message:
          type: string
          example: Authentication successful.
        token:
          type: string
          example: bearer_token_usr_1_1727610000000
        user:
          type: object
          properties:
            id:
              type: integer
              example: 1
            name:
              type: string
              example: Acme Admin
            email:
              type: string
              example: admin@acme.corp

    CreateTaskRequest:
      type: object
      required:
        - user_id
        - title
      properties:
        user_id:
          type: integer
          example: 1
        title:
          type: string
          maxLength: 255
          example: Deploy Production Kubernetes Cluster
        description:
          type: string
          example: Configure Terraform scripts and deploy to us-east-1.
        status:
          type: string
          enum: [OPEN, IN_PROGRESS, COMPLETED]
          default: OPEN
          example: OPEN
        due_date:
          type: string
          format: date-time
          example: 2026-10-15T00:00:00.000Z

    CreateTaskResponse:
      type: object
      properties:
        success:
          type: boolean
          example: true
        message:
          type: string
          example: Task created successfully.
        data:
          $ref: '#/components/schemas/Task'

    GetTaskResponse:
      type: object
      properties:
        success:
          type: boolean
          example: true
        data:
          $ref: '#/components/schemas/Task'

    Task:
      type: object
      properties:
        id:
          type: integer
          example: 101
        user_id:
          type: integer
          example: 1
        title:
          type: string
          example: Quarterly Financial Audit
        description:
          type: string
          example: Prepare and reconcile Q3 ledger entries.
        status:
          type: string
          enum: [OPEN, IN_PROGRESS, COMPLETED]
          example: IN_PROGRESS
        due_date:
          type: string
          format: date-time
          example: 2026-10-06T12:00:00.000Z
        created_at:
          type: string
          format: date-time
          example: 2026-09-29T16:55:00.000Z

    ErrorResponse:
      type: object
      properties:
        success:
          type: boolean
          example: false
        error:
          type: string
          example: Validation Error / Resource Not Found
```
