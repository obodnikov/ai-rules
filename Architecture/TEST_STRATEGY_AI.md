# TEST_STRATEGY_AI.md

*Instruction guide for AI coder generating tests*

This document describes **how tests MUST be designed and implemented** for the *photo restoration webpage* project (FastAPI backend, React+TS frontend, HuggingFace Inference API, Docker + nginx).

Whenever you write code for this project, you should also update tests following this strategy.

---

## 1. Scope of Testing

You MUST cover:

1. **Backend (FastAPI)**

   * Auth (JWT, login, protected endpoints)
   * Health check endpoints
   * Image upload & validation
   * AI model routing & HuggingFace API integration
   * Database (SQLite via SQLAlchemy)
   * Configuration (env vars, MODELS_CONFIG parsing) 

2. **Frontend (React + TypeScript + Vite)**

   * Auth flows and protected routes
   * Image upload UX
   * Model selection / configuration
   * Before/After viewer
   * Error & loading states

3. **Integration & E2E**

   * End-to-end flow: login → upload image → select model → restore → see result.
   * nginx reverse proxy routing (`/` → frontend, `/api` → backend) in Docker environment.

4. **Non-functional**

   * Basic performance constraints (request timeouts & file size limits)
   * Security (auth, input validation)
   * Reliability (graceful failure of external services: HuggingFace, OwnCloud in future)

---

## 2. Test Levels & Tools

### 2.1 Backend Tests

**Tools (assume / enforce this stack):**

* `pytest`
* `pytest-asyncio`
* `httpx` or `fastapi.testclient` for API tests
* `pytest-mock` or `unittest.mock` for mocking
* `sqlite` in-memory DB for tests

**Types of tests:**

1. **Unit Tests**

   * Pure functions in `services/`, `utils/`, model config parsing, image validation.
   * HF client wrapper logic with mocked HTTP responses.
   * JWT utilities: token creation, validation, expiration handling.

2. **Integration Tests (API-level)**

   * Spin up FastAPI app with test settings.
   * Use a temp SQLite DB.
   * Test real routing + dependency injection stack (WITHOUT real HF / OwnCloud calls – mock those).

3. **Contract/API Tests**

   * Ensure stable request/response shapes for:

     * `/api/v1/auth/login`
     * `/api/v1/auth/me`
     * `/api/v1/restore`
     * `/health`, `/api/health`
   * If response schema changes, tests MUST fail loudly.

> **Backend tests live in:** `backend/tests/` with a structure mirroring `app/`.

---

### 2.2 Frontend Tests

**Tools (assume / enforce this stack):**

* `Vitest` (or Jest if already chosen in project)
* `@testing-library/react`
* `@testing-library/user-event`
* Optional: `Playwright`/`Cypress` for E2E UI (see §2.3)

**Types of tests:**

1. **Unit / Component Tests**

   * Components in `src/components/` and `src/features/`:

     * Model selector
     * Image uploader
     * Before/After viewer
     * Auth forms & protected route guards

2. **Integration Tests (Frontend)**

   * With React Router:

     * Redirect to `/login` when unauthenticated.
     * Grant access to protected routes when JWT present.
   * With mocked API client:

     * Simulate login success/failure.
     * Simulate restore request flow (upload → result).

---

### 2.3 End-to-End Tests (E2E)

**Tools (preferred):** Playwright (or Cypress).

Run against **Docker-compose dev stack** (frontend + backend + nginx).

E2E scenarios MUST avoid calling real HuggingFace API:

* Use **test-mode backend** (env flag) where HF calls are mocked or use a local “fake model” that just returns a transformed test image.

---

## 3. Environments & Configuration

### 3.1 Test Environment Rules

* NEVER use production HF keys in tests.

* For backend tests, set env vars via `.env.test`:

  * `HF_API_KEY="test-key"`
  * `DATABASE_URL="sqlite+aiosqlite:///:memory:"` or a temp file DB.
  * `MODELS_CONFIG` = minimal JSON with 1–2 test models.

* For frontend tests:

  * `VITE_API_BASE_URL="http://localhost:8000/api/v1"` (or mocked).

### 3.2 Docker-Based Test Setup

Add one `docker-compose.test.yml` (or reuse dev with overrides):

* Backend runs with:

  * Test DB volume (throwaway)
  * `APP_ENV=test`
* Frontend can be tested with Playwright inside a separate container OR against dev server.

---

## 4. External Dependencies & Mocking

You MUST NOT hit external network in unit/integration tests.

### 4.1 HuggingFace Inference API

Wrap HF calls in a dedicated service, e.g. `app/services/hf_inference.py`.

For tests:

* Patch the service methods (e.g. `HFInferenceService.upscale_swin2sr`, `.enhance_qwen`) to:

  * Return pre-generated binary image content (e.g. from `/tests/data/sample_output.png`).
  * Or return a simple identifiable transformation (e.g. echo input for unit tests).

Test cases:

* Success path (200, valid image bytes)
* Rate limit / 429
* 5xx response from HF
* Invalid/malformed response

### 4.2 OwnCloud (Future Phases)

Wrap OwnCloud in `app/services/owncloud_client.py`.

For now, specify tests but mark them `@pytest.mark.xfail` or skip if OWNCloud is not yet implemented:

* Successful list/read/write
* Auth errors
* Network failures

### 4.3 JWT & Auth

* Use fixed `SECRET_KEY` in tests for deterministic tokens.
* Provide helper for creating tokens for test users.
* Tests MUST verify:

  * Token expiration
  * Invalid signature
  * Missing / malformed `Authorization` header

---

## 5. Test Data

Create the following in `backend/tests/data/` and `frontend/src/test-data/`:

* `old_photo_small.jpg` – small low-resolution photo.
* `old_photo_large.jpg` – large (near limit) photo.
* `invalid_file.txt` – non-image file.
* `corrupted_image.jpg` – truncated/invalid bytes.

Use them for:

* File-size and type validation tests.
* Error handling when image cannot be decoded.
* HF integration mocking (input + expected generated output).

---

## 6. Concrete Test Suites (Checklist)

The following lists describe specific tests AI coder MUST implement.

### 6.1 Backend – Health & Config

**File:** `backend/tests/test_health.py`

* [ ] `/health` returns 200 and JSON with status `"ok"`.
* [ ] `/api/health` (if exists) returns 200 and backend status.
* [ ] App fails to start (or logs clear error) if:

  * `HF_API_KEY` missing.
  * `SECRET_KEY` shorter than 32 chars.
* [ ] `MODELS_CONFIG` parsing:

  * Valid JSON → models loaded.
  * Invalid JSON → startup failure with meaningful error.
  * Missing required fields (id/model) → rejected.

---

### 6.2 Backend – Auth

**File:** `backend/tests/test_auth.py`

* [ ] `POST /api/v1/auth/login`:

  * Valid credentials → 200, returns access token, expiry, token type.
  * Invalid username or password → 401.
* [ ] Token generation:

  * Token contains expected `sub` claim (username).
  * Expiry (`exp`) set correctly (e.g. configurable).
* [ ] Protected endpoint (e.g. `/api/v1/restore`):

  * No token → 401.
  * Malformed token → 401.
  * Expired token → 401.
* [ ] Auto-logout behavior from backend perspective:

  * Token past `exp` is rejected.

---

### 6.3 Backend – Image Upload & Validation

**File:** `backend/tests/test_restore_validation.py`

* [ ] Upload valid JPEG image within size limit:

  * 200 response.
  * Response contains expected fields: `job_id` or processed image.
* [ ] Upload PNG, JPG, WEBP – accepted formats.
* [ ] Upload unsupported format (e.g. `bmp`, `txt`) → 400 with error message.
* [ ] Exceed `MAX_UPLOAD_SIZE` → 413 or 400 with clear error.
* [ ] Corrupted image → 400 with error `"Invalid image data"`.

---

### 6.4 Backend – Model Selection & HF Integration

**File:** `backend/tests/test_restore_models.py`

Use mocked `HFInferenceService`.

* [ ] Request with valid `model_id` from `MODELS_CONFIG` → calls correct HF model.
* [ ] Unknown `model_id` → 400 with `"Unknown model"` error.
* [ ] If HF service returns binary image:

  * Response is valid image (e.g. `image/jpeg` or `image/png`).
* [ ] HF 429 (rate limit) → backend converts to 503 or 429 with retry-after header.
* [ ] HF network error → backend returns 502/503 with generic `"Model service unavailable"` message.
* [ ] Timeouts: backend enforces timeout and returns error if HF call exceeds limit.

---

### 6.5 Backend – History / Sessions (when implemented)

**File:** `backend/tests/test_history.py`

* [ ] After successful restore, entry is saved in DB:

  * user id
  * original image path
  * processed image path
  * model id
  * timestamp
* [ ] `GET /api/v1/history` returns only authenticated user’s items.
* [ ] Pagination works if implemented.

---

### 6.6 Frontend – Auth & Routing

**File:** `frontend/src/__tests__/auth.test.tsx`

With mocked API client:

* [ ] Login form:

  * Submitting valid credentials → stores token (Zustand / localStorage) and redirects to main page.
  * Invalid credentials → shows error message.
* [ ] Remember me:

  * Checkbox enabled → token persists after reload.
  * Without remember me → token cleared on browser restart (simulate).
* [ ] Protected routes:

  * If no token → redirect to `/login`.
  * With token → renders protected content.
* [ ] Auto-logout:

  * If token in state is expired (mock) → user is logged out and redirected; banner shown.

---

### 6.7 Frontend – Image Workflow

**File:** `frontend/src/__tests__/image_workflow.test.tsx`

Mock API calls.

* [ ] Upload component:

  * Drag & drop or file picker sets preview.
  * Disallow non-image types (UX error message).
* [ ] Model selector:

  * Renders list based on `MODELS_CONFIG` from API (or local config).
  * Can switch between different models.
* [ ] Restore flow:

  * User selects image + model → clicks “Restore”.
  * Show loading indicator while waiting.
  * On success: displays Before/After viewer with two images.
  * On error: shows friendly error and allows retry.
* [ ] Edge cases:

  * Click “Restore” without image → validation error.
  * Click “Restore” without model → validation error.

---

### 6.8 Frontend – UI/UX Basics

**File:** `frontend/src/__tests__/layout.test.tsx`

* [ ] sqowe brand elements present (logo, primary color usage, typographic scale). 
* [ ] Responsive layout:

  * Main layout renders correctly at mobile & desktop breakpoints (Playwright or component tests with viewport override).
* [ ] Error boundary (if implemented) shows fallback UI, not blank screen.

---

### 6.9 E2E Flows

**File examples:** `tests/e2e/restore.spec.ts`

Using Playwright:

* [ ] `Happy path`

  1. Navigate to `/`
  2. Redirect to `/login`
  3. Login with test credentials
  4. Upload test image
  5. Select default model
  6. Click “Restore”
  7. See processed image and “Download” button

* [ ] `Invalid file`

  * Upload a `.txt` file → inline error, no request sent.

* [ ] `HF failure (simulated by backend)`

  * Backend in test mode returns error → UI shows clear message and does not crash.

---

## 7. Test Structure & Naming Conventions

### 7.1 Backend

* Tests dir: `backend/tests/`

* Use matching folder structure:

  ```text
  backend/app/
    api/v1/restore.py      → backend/tests/api/v1/test_restore.py
    api/v1/auth.py         → backend/tests/api/v1/test_auth.py
    services/hf_inference.py → backend/tests/services/test_hf_inference.py
    services/history.py    → backend/tests/services/test_history.py
  ```

* Test file naming: `test_*.py`

* Each test function name MUST describe behavior:

  ```python
  def test_restore_returns_400_for_unsupported_file_type(): ...
  ```

### 7.2 Frontend

* Tests dir: `frontend/src/__tests__/`

* Group per feature:

  ```text
  frontend/src/features/auth/        → auth.test.tsx
  frontend/src/features/restore/     → restore_flow.test.tsx
  frontend/src/components/           → uploader.test.tsx
  ```

* Use `describe` blocks per component/feature.

---

## 8. Non-Functional Tests (Minimum)

### 8.1 Performance (Backend)

* Add simple performance checks (can be marked “slow”):

  * [ ] Typical restore request (with HF mocked) completes < X ms (configurable threshold).
  * [ ] Large image near `MAX_UPLOAD_SIZE` still processed within acceptable time (with mocked HF).

### 8.2 Security

* [ ] No sensitive information in error messages (no HF key, no stack traces in prod mode).
* [ ] CORS configuration tested: frontend origin allowed, others blocked (if applicable).
* [ ] Auth: tokens not logged; `SECRET_KEY` never exposed.

---

## 9. Workflow for AI Coder

When you, as AI coder, generate or modify code for this project, follow this checklist:

1. **Before coding**

   * Read `README.md` and relevant `AI*.md` guidelines. 
   * Identify which module you change (backend or frontend) and which tests must be added/updated.

2. **When adding a new feature**

   * Define the API / component behavior.
   * Add/extend tests FIRST (or in same patch):

     * Backend: new route/service → new test file or new test case.
     * Frontend: new component/flow → new test or update existing.

3. **When fixing a bug**

   * Reproduce bug in a failing test.
   * Fix the code.
   * Ensure the new test passes and add comment `# regression test for bug <short id>`.

4. **Quality rules**

   * All new code MUST have tests; untested code is not allowed.
   * Keep test files < 800 lines; split into multiple files if needed (follow same principle as main codebase).
   * Tests must be deterministic:

     * No real network calls.
     * No randomness without seeding.
   * Prefer small, focused test cases over huge scenario monsters.

5. **Output format for patches**

   * When asked, generate **unified diff patches** that include:

     * New/modified test files.
     * Minimal code changes for features/fixes.

---

## 10. Minimal Test Matrix for MVP Completion (Phase 1.3)

Before we can consider Phase 1 (MVP) “DONE”, ensure:

* [ ] Backend:

  * [ ] Health & config tests green.
  * [ ] Auth tests green.
  * [ ] Restore validation tests green.
  * [ ] HF integration mocked tests green.
* [ ] Frontend:

  * [ ] Auth + routing tests green.
  * [ ] Image upload + restore flow tests green.
  * [ ] Basic layout tests green.
* [ ] E2E:

  * [ ] At least 1 happy-path test.
  * [ ] At least 1 error-path test (HF failure / invalid file).

Once all of the above are implemented and passing, the core photo restoration flow is considered **test-covered** for MVP.

---
