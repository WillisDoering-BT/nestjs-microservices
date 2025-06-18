# Workshop: Supercharge Your NestJS Development with Copilot, TypeScript & Modern Testing

Welcome to this hands-on workshop! In the next few hours, you’ll learn how to leverage GitHub Copilot to boost your productivity in NestJS backend development, with a strong focus on unit, integration, and end-to-end testing (including Playwright). This guide is tailored for developers already comfortable with TypeScript, React, and NestJS.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Project Setup](#project-setup)
3. [Developing New Functionality with Copilot](#developing-new-functionality-with-copilot)
4. [Adding New Microservices](#adding-new-microservices)
5. [Integrating a Real Database](#integrating-a-real-database)
6. [Implementing Advanced Authentication](#implementing-advanced-authentication)
7. [Expanding REST Endpoints & Test Cases](#expanding-rest-endpoints--test-cases)
8. [Unit Testing with Copilot](#unit-testing-with-copilot)
9. [Integration Testing with Copilot](#integration-testing-with-copilot)
10. [Functional & End-to-End Testing with Playwright and Copilot](#functional--end-to-end-testing-with-playwright-and-copilot)
11. [Best Practices & Copilot Prompting Tips](#best-practices--copilot-prompting-tips)
12. [Q&A and Wrap-Up](#qa-and-wrap-up)

---

## Introduction

- **Goal:** Learn to use GitHub Copilot for rapid feature development and testing in NestJS.
- **Agenda:** 
  - Set up the project and Copilot
  - Develop new microservices
  - Integrate a real database
  - Implement JWT/OAuth authentication
  - Expand REST endpoints and test cases
  - Add unit, integration, and E2E tests (including Playwright)
  - Learn prompting and best practices

---

## Project Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/im-juma-training-sb/nestjs-microservices.git
cd nestjs-microservices
```

### Step 2: Install Dependencies

```bash
npm install
# or
yarn install
```

### Step 3: Open in VSCode & Enable Copilot

- Open the project in [Visual Studio Code](https://code.visualstudio.com/).
- Make sure the [GitHub Copilot extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) is installed and enabled.
- Sign in with your GitHub account if prompted.

### Step 4: Explore the Project Structure

- Familiarize yourself with the `src/` directory.
- Identify existing modules, controllers, and services.

---

## Developing New Functionality with Copilot

**Goal:** Create a new NestJS module (e.g., "Tasks") using Copilot.

### Step 1: Scaffold a New Module

```bash
# Use Nest CLI (if installed)
nest g module tasks
nest g controller tasks
nest g service tasks
```
> If you don’t have the CLI, create the files manually:  
> `src/tasks/tasks.module.ts`, `src/tasks/tasks.controller.ts`, `src/tasks/tasks.service.ts`.

### Step 2: Use Copilot to Write DTOs

Open `src/tasks/dto/create-task.dto.ts` and type:

```typescript
export class CreateTaskDto {
  // Copilot suggestion: title: string; description: string; dueDate?: Date;
}
```
> Let Copilot suggest fields. Tab to accept, or edit as needed.

### Step 3: Add Endpoints with Copilot

In `tasks.controller.ts`, start typing method signatures and let Copilot complete them:

```typescript
@Post()
create(@Body() createTaskDto: CreateTaskDto) {
  // Copilot can suggest a call to tasksService.create(createTaskDto);
}
```

> Try prompts like:
> - “Write a GET endpoint to list all tasks”
> - “Add a DELETE endpoint for a task by id”

### Step 4: Implement Logic in Service

In `tasks.service.ts`, type function stubs and allow Copilot to fill in logic, e.g., storing tasks in memory or using a database.

### Step 5: Refactor with Copilot Suggestions

- Highlight code blocks and use Copilot’s inline suggestions for improvements.
- Try “// Refactor this function to use async/await” or “// Add error handling”.

---

## Adding New Microservices

**Goal:** Expand the architecture to include additional microservices such as User Profile and Tasks.

### Step 1: Scaffold New Microservices

```bash
nx g @nx/nest:app user-profile-microservice
nx g @nx/nest:app tasks-microservice
```

### Step 2: Define Microservice Responsibilities

- **User Profile Microservice:** Manage user profile data (bio, avatar, etc.).
- **Tasks Microservice:** Manage task CRUD operations.

### Step 3: Create DTOs and Entities (with Copilot)

- Use shared libraries for DTOs and entities.
- Prompt Copilot:  
  - “Create DTO and entity for UserProfile with fields: userId, bio, avatarUrl.”
  - “Create DTO and entity for Task with fields: id, title, description, status, userId.”

### Step 4: Set Up Message Patterns

- Use `@MessagePattern` in each microservice controller to handle actions such as `get_profile`, `update_profile`, `create_task`, `delete_task`.

### Step 5: Register Microservices in API Gateway

- Use `ClientsModule.register()` in the API Gateway to communicate with new microservices.

### Step 6: Expose REST Endpoints

- Add new routes in the API Gateway (e.g., `/profile`, `/tasks`) to forward requests to the respective microservices.

---

## Integrating a Real Database

**Goal:** Replace in-memory data with a real relational or NoSQL database.

### Step 1: Choose and Install an ORM

- Recommended: [TypeORM](https://typeorm.io/) or [Prisma](https://www.prisma.io/).
- Example for PostgreSQL + TypeORM:
  ```bash
  npm install @nestjs/typeorm typeorm pg
  ```

### Step 2: Configure Database Module

- In each microservice, initialize the ORM connection in the module (e.g., `TypeOrmModule.forRoot({...})`).

### Step 3: Create Database Entities

- Refactor entity classes to use ORM decorators.
- Prompt Copilot:  
  - “Refactor User entity for TypeORM with id, username, password, and unique constraint on username.”

### Step 4: Inject Repositories

- Use injected repositories/services for CRUD instead of in-memory arrays.

### Step 5: Update Services and Tests

- Replace array operations with database queries.
- Update or add integration tests to verify persistence.

---

## Implementing Advanced Authentication

**Goal:** Move from basic password checks to JWT/OAuth authentication.

### Step 1: Install Auth Packages

```bash
npm install @nestjs/jwt passport passport-jwt
```

### Step 2: Implement JWT Auth Flow

- In Auth Microservice:
  - On successful login, sign a JWT token and return it.
  - Prompt Copilot:  
    - “Implement JWT strategy for NestJS.”
    - “Create a function to sign and verify JWTs.”

- In API Gateway:
  - Protect endpoints using JWT Guard.
  - Prompt Copilot:  
    - “Add AuthGuard to protect /tasks endpoints.”

### Step 3: (Optional) Add OAuth Support

- For OAuth2 (e.g., Google), install relevant passport strategies and configure.
- Prompt Copilot:  
  - “Set up Google OAuth strategy in NestJS.”

### Step 4: Update Frontend/Clients

- Ensure clients store and send JWT tokens with requests.

---

## Expanding REST Endpoints & Test Cases

**Goal:** Add more endpoints and corresponding test cases for richer functionality.

### Step 1: New Endpoints

- Example endpoints:
  - `/tasks/assign` (POST): Assign a task to a user.
  - `/profile/avatar` (PUT): Update user avatar.
  - `/tasks/:id/complete` (PATCH): Mark a task as complete.

- Use Copilot prompts:  
  - “Write a PATCH endpoint to mark a task as complete.”

### Step 2: Add Test Cases

- Use Jest/Supertest for unit and integration tests.
- For each new endpoint, cover:
  - Success scenarios
  - Validation errors
  - Authorization errors
  - Edge cases (e.g., task not found, unauthorized user)

---

## Unit Testing with Copilot

**Goal:** Use Copilot to scaffold and expand unit test coverage.

### Step 1: Locate/Create Test Files

Open or create `src/tasks/tasks.service.spec.ts`.

### Step 2: Write Test Cases

Start typing a test case and let Copilot suggest completions:

```typescript
describe('TasksService', () => {
  let service: TasksService;

  beforeEach(() => {
    service = new TasksService();
  });

  it('should create a task', () => {
    // Copilot will suggest test logic here
  });
});
```

> Prompt Copilot: “Write a test that ensures a task is added to the list.”

### Step 3: Run Tests

```bash
npm run test
```

### Step 4: Expand Coverage

- Ask Copilot to generate tests for edge cases.
- Review and edit suggestions for accuracy.

---

## Integration Testing with Copilot

**Goal:** Test endpoints with real HTTP requests.

### Step 1: Create Integration Test File

Create `src/tasks/tasks.controller.spec.ts`.

### Step 2: Scaffold Test Setup

Start with:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { TasksModule } from './tasks.module';

describe('TasksController (e2e)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [TasksModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  // Copilot will suggest integration tests here

  afterAll(async () => {
    await app.close();
  });
});
```

### Step 3: Add Endpoint Tests

Prompt Copilot: “Test POST /tasks creates a new task” or start typing and let Copilot complete:

```typescript
it('/tasks (POST)', () => {
  return request(app.getHttpServer())
    .post('/tasks')
    .send({ title: 'Test task', description: 'Test' })
    .expect(201);
});
```

### Step 4: Run Integration Tests

```bash
npm run test:e2e
```

---

## Functional & End-to-End Testing with Playwright and Copilot

**Goal:** Use Playwright to test your NestJS backend from the outside as a user would.

### Step 1: Install Playwright

```bash
npm install --save-dev playwright @playwright/test
npx playwright install
```

### Step 2: Set Up Playwright Config

Create `playwright.config.ts` or use the command:

```bash
npx playwright codegen
```

### Step 3: Write Your First E2E Test

Create `tests/tasks.e2e.spec.ts`:

```typescript
import { test, expect } from '@playwright/test';

test('POST /tasks creates a new task', async ({ request }) => {
  const response = await request.post('/tasks', {
    data: { title: 'Playwright Task', description: 'E2E test' }
  });
  expect(response.ok()).toBeTruthy();
  const body = await response.json();
  expect(body.title).toBe('Playwright Task');
});
```

> Let Copilot suggest test logic and assertions.

### Step 4: Run Playwright Tests

```bash
npx playwright test
```

### Step 5: Advanced—Chain Requests & Test Workflows

Prompt Copilot: “Write a test that creates a task, fetches it, and then deletes it.”

---

## Best Practices & Copilot Prompting Tips

- **Prompt Examples:**
  - “Write a function to…”
  - “Add a test for…”
  - “Refactor to use async/await”

- **Edit and Review:** Copilot is a helper, not a replacement for your brain—always verify what it suggests.
- **Iterate:** Accept suggestions, run tests, tweak as needed.
- **Copilot for Refactoring:** Highlight code, use Copilot for suggestions.

---

## Q&A and Wrap-Up

- What worked well? What was confusing?
- How could Copilot help you more?
- Check out advanced Copilot [docs](https://docs.github.com/copilot), [NestJS docs](https://docs.nestjs.com/), and Playwright guides.
- Consider contributing your new module or tests to the repo!

---

> **Feedback Welcome!**  
> Please share your experience and suggestions for improvement.
