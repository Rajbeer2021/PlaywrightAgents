# OrangeHRM Demo — Manual Test Case Analysis and Test Plan

## 1. Objective

Analyze the manual test workbook and produce a structured test plan for automating the OrangeHRM Open Source Demo login and dashboard flows using JavaScript, Playwright Test, and Page Object Model.

## 2. Source Workbook

| Property | Value |
|---|---|
| **Workbook path** | `ManualTests/HRM Manual tests.xlsx` |
| **Format** | `.xlsx` (Excel) |
| **Sheets found** | 2 |
| **Sheet 1** | `1. Login test cases` — 10 test cases |
| **Sheet 2** | `2. Dashboard test cases` — 10 test cases |
| **Total test cases** | 20 |
| **Columns** | Test ID, Test Name, Test Steps, Expected Results |

No Priority, Precondition, Test Data, or Status columns exist in the workbook. All test IDs are preserved as-is.

## 3. Application Details

| Property | Value |
|---|---|
| Application | OrangeHRM Open Source Demo |
| Login URL | `https://opensource-demo.orangehrmlive.com/web/index.php/auth/login` |
| Dashboard URL | `https://opensource-demo.orangehrmlive.com/web/index.php/dashboard/index` |
| Demo username | `Admin` |
| Demo password | `admin123` |
| Language | JavaScript |
| Test framework | `@playwright/test` |
| Design pattern | Page Object Model (POM) |

Credentials must be supplied through environment variables (`ORANGEHRM_USERNAME`, `ORANGEHRM_PASSWORD`) and never hardcoded or committed.

## 4. Scope

### In scope

- Login page loads and displays required elements
- Valid authentication redirects to dashboard
- Invalid credential handling (bad username, bad password, empty fields)
- Password masking
- Login recovery after failed attempt
- Login via Enter key
- Dashboard URL, heading, menu, widgets, and navigation
- Dashboard refresh behavior
- Direct dashboard access without login (access control)
- Browser back navigation after login

### Out of scope (first iteration)

- Full Admin, PIM, Leave, Recruitment, or other module testing
- API or performance testing
- Security penetration testing
- Cross-browser testing beyond the initially configured browser
- Mobile or responsive layout testing

## 5. Feature-wise Test Scenarios

### 5.1 Authentication — Login Page

| Test ID | Title | Automation Feasibility | Notes |
|---|---|---|---|
| LOGIN-001 | Verify login page loads | READY | Basic page load and element visibility |
| LOGIN-002 | Login with valid credentials | READY | Core positive auth flow |
| LOGIN-003 | Login with invalid username | READY | Negative — bad username |
| LOGIN-004 | Login with invalid password | READY | Negative — bad password |
| LOGIN-005 | Submit login with both fields empty | READY | Negative — empty required fields |
| LOGIN-006 | Submit login with empty username | READY | Negative — missing username |
| LOGIN-007 | Submit login with empty password | READY | Negative — missing password |
| LOGIN-008 | Verify password masking | READY | Visual/behavioral check |
| LOGIN-009 | Login after an unsuccessful attempt | READY | Recovery flow — retry with valid creds |
| LOGIN-010 | Login using Enter key | READY | Keyboard-driven submission |

### 5.2 Dashboard

| Test ID | Title | Automation Feasibility | Notes |
|---|---|---|---|
| DASH-001 | Verify dashboard opens after login | READY | Post-login redirect verification |
| DASH-002 | Verify dashboard heading | READY | Element visibility check |
| DASH-003 | Verify main navigation menu | READY | Menu item presence |
| DASH-004 | Verify dashboard widgets | READY | Widget/panel rendering |
| DASH-005 | Verify dashboard page refresh | READY | Session persistence on refresh |
| DASH-006 | Direct dashboard access without login | READY | Access control / redirect to login |
| DASH-007 | Dashboard navigation and return | READY | Navigate away and return to dashboard |
| DASH-008 | Browser back navigation | NEEDS_CLARIFICATION | Expected behavior not fully specified |
| DASH-009 | Verify dashboard URL after login | READY | URL assertion |
| DASH-010 | Dashboard is usable after loading | READY | Element visibility and usability check |

## 6. Test Case Traceability

### 6.1 Source: Sheet `1. Login test cases`

#### LOGIN-001 — Verify login page loads
- **Steps:** 1. Open the OrangeHRM login URL. 2. Wait for the page to load.
- **Expected:** Login page loads and username, password, and Login button are visible.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should display login page with username, password, and login button`

#### LOGIN-002 — Login with valid credentials
- **Steps:** 1. Open the login page. 2. Enter valid credentials. 3. Click Login.
- **Expected:** User is authenticated and redirected to the dashboard.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should authenticate with valid credentials and redirect to dashboard`

#### LOGIN-003 — Login with invalid username
- **Steps:** 1. Open the login page. 2. Enter an invalid username. 3. Enter a non-secret test password. 4. Click Login.
- **Expected:** Login is rejected and an appropriate error message is displayed.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should reject login with invalid username and show error message`
- **Note:** Requires a non-secret invalid test username; do not use the demo credentials.

#### LOGIN-004 — Login with invalid password
- **Steps:** 1. Open the login page. 2. Enter a valid username. 3. Enter an incorrect password. 4. Click Login.
- **Expected:** Login is rejected and the dashboard is not displayed.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should reject login with invalid password`
- **Note:** Requires a non-secret invalid password; do not use the demo credentials.

#### LOGIN-005 — Submit login with both fields empty
- **Steps:** 1. Open the login page. 2. Leave both fields empty. 3. Click Login.
- **Expected:** Login is prevented and appropriate validation is displayed.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should prevent login when both fields are empty`
- **Note:** The exact validation mechanism (inline validation vs. server-side error) should be verified against the live application.

#### LOGIN-006 — Submit login with empty username
- **Steps:** 1. Open the login page. 2. Leave username empty. 3. Enter a password. 4. Click Login.
- **Expected:** Login is prevented and appropriate validation is displayed.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should prevent login when username is empty`

#### LOGIN-007 — Submit login with empty password
- **Steps:** 1. Open the login page. 2. Enter a username. 3. Leave password empty. 4. Click Login.
- **Expected:** Login is prevented and appropriate validation is displayed.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should prevent login when password is empty`

#### LOGIN-008 — Verify password masking
- **Steps:** 1. Open the login page. 2. Enter a sample password. 3. Observe the password field.
- **Expected:** Password characters are masked and not displayed as readable text.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should mask password characters`
- **Note:** Verification approach: assert the password input `type` is `"password"` and/or programmatically check the field value is not visible as plain text.

#### LOGIN-009 — Verify login after an unsuccessful attempt
- **Steps:** 1. Submit invalid credentials. 2. Observe the error. 3. Enter valid credentials. 4. Click Login.
- **Expected:** The page remains usable, and valid credentials allow login when the demo environment accepts them.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should allow login with valid credentials after a failed attempt`
- **Note:** Assumes the demo session allows retries without lockout or CAPTCHA. Should be verified against the current demo behavior.

#### LOGIN-010 — Verify login using Enter key
- **Steps:** 1. Enter valid credentials. 2. Focus the password field. 3. Press Enter.
- **Expected:** The login form submits and authentication succeeds when credentials are valid.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should submit login via Enter key`

### 6.2 Source: Sheet `2. Dashboard test cases`

#### DASH-001 — Verify dashboard opens after login
- **Steps:** 1. Open the login page. 2. Enter valid credentials. 3. Click Login.
- **Expected:** User is redirected to `/web/index.php/dashboard/index`.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should redirect to dashboard URL after successful login`

#### DASH-002 — Verify dashboard heading
- **Steps:** 1. Log in successfully. 2. Wait for the dashboard to load. 3. Locate the dashboard heading.
- **Expected:** Dashboard heading or other agreed identifying content is visible.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should display dashboard heading after login`
- **Note:** The exact dashboard heading text should be verified against the live application.

#### DASH-003 — Verify main navigation menu
- **Steps:** 1. Log in successfully. 2. Inspect the main navigation menu.
- **Expected:** Expected navigation items are visible for the authenticated user.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should display main navigation menu items`
- **Note:** The set of expected menu items (Admin, PIM, Leave, etc.) should be verified against the live application.

#### DASH-004 — Verify dashboard widgets
- **Steps:** 1. Log in successfully. 2. Inspect the dashboard widgets or panels.
- **Expected:** Expected widgets or panels render without a visible loading or error state.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should render dashboard widgets without errors`
- **Note:** The names/count of widgets should be verified against the live application. Common widgets include Time at Work, My Actions, Quick Launch, etc.

#### DASH-005 — Verify dashboard page refresh
- **Steps:** 1. Log in successfully. 2. Refresh the dashboard.
- **Expected:** The dashboard remains accessible if the session is still valid.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should remain accessible after page refresh`

#### DASH-006 — Verify direct dashboard access without login
- **Steps:** 1. Open a fresh browser context. 2. Navigate directly to the dashboard URL without logging in.
- **Expected:** Protected dashboard content is not accessible; the user is redirected to login or access is denied.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`
- **Suggested test title:** `should redirect unauthenticated users to login page`
- **Note:** Use a new browser context or clear storage to ensure no cached session.

#### DASH-007 — Verify dashboard navigation and return
- **Steps:** 1. Log in successfully. 2. Open an available navigation item. 3. Return to the dashboard.
- **Expected:** Dashboard can be reopened and its identifying content is visible.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should allow returning to dashboard after navigating to another page`
- **Note:** The specific navigation item to click should be determined during implementation (e.g., Admin, PIM, or a low-risk module).

#### DASH-008 — Verify browser back navigation
- **Steps:** 1. Log in successfully. 2. Navigate to another page or use browser history. 3. Observe the resulting page and authentication state.
- **Expected:** Navigation respects the application's session and access-control behavior.
- **Automation status:** NEEDS_CLARIFICATION
- **Reason:** The expected result is vague — "Navigation respects the application's session and access-control behavior" — and does not specify whether the user should remain on the dashboard, be redirected to login, or see a specific page. Requires clarification on the exact expected outcome.

#### DASH-009 — Verify dashboard URL after login
- **Steps:** 1. Log in with valid credentials. 2. Read the current browser URL.
- **Expected:** URL matches the expected dashboard route.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should have correct dashboard URL after login`
- **Note:** Overlaps with DASH-001 but focuses on URL assertion rather than redirect behavior.

#### DASH-010 — Verify dashboard is usable after loading
- **Steps:** 1. Log in successfully. 2. Wait for dashboard content to load. 3. Verify an agreed dashboard element is visible and usable.
- **Expected:** Dashboard content is visible and the selected element is usable.
- **Automation status:** READY
- **Proposed POM:** `LoginPage`, `DashboardPage`
- **Suggested test title:** `should display usable dashboard content after loading`

## 7. Priority Assessment

The workbook does not include a Priority column. The following priorities are inferred from functional criticality:

| Priority | Count | Test IDs |
|---|---|---|
| **High** | 6 | LOGIN-001, LOGIN-002, LOGIN-003, LOGIN-004, DASH-001, DASH-006 |
| **Medium** | 11 | LOGIN-005, LOGIN-006, LOGIN-007, LOGIN-008, LOGIN-009, LOGIN-010, DASH-002, DASH-003, DASH-004, DASH-005, DASH-009 |
| **Low** | 2 | DASH-007, DASH-010 |
| **Needs clarification** | 1 | DASH-008 |

*Note: These are recommended priorities. Final prioritization should be confirmed by the project lead or QA lead.*

## 8. Test Data and Environment Requirements

| Item | Requirement |
|---|---|
| **Valid username** | `Admin` (demo default) — supply via `ORANGEHRM_USERNAME` env var |
| **Valid password** | `admin123` (demo default) — supply via `ORANGEHRM_PASSWORD` env var |
| **Invalid username** | A non-secret, non-empty string (e.g., `InvalidUser`) |
| **Invalid password** | A non-secret string different from the valid password (e.g., `wrongPassword123`) |
| **Application URL** | `https://opensource-demo.orangehrmlive.com` |
| **Node.js** | v24.18.0 (available) |
| **Playwright** | ^1.63.0 (installed) |
| **Environment variables** | `ORANGEHRM_USERNAME`, `ORANGEHRM_PASSWORD` must be set before execution |

## 9. Automation Feasibility Summary

| Status | Count | Explanation |
|---|---|---|
| **READY** | 19 | Test cases have clear steps and measurable expected results |
| **NEEDS_CLARIFICATION** | 1 | DASH-008 — expected outcome of browser back navigation is underspecified |
| **NOT_SUITABLE** | 0 | All cases are suitable for UI automation |

## 10. Coverage Summary

| Feature Area | Total Cases | Ready | Needs Clarification |
|---|---|---|---|
| Login page load & elements | 1 | 1 | 0 |
| Positive authentication | 1 | 1 | 0 |
| Negative authentication (invalid credentials) | 2 | 2 | 0 |
| Negative authentication (empty fields) | 3 | 3 | 0 |
| Password masking | 1 | 1 | 0 |
| Recovery after failure | 1 | 1 | 0 |
| Keyboard-driven login | 1 | 1 | 0 |
| Dashboard — redirect & URL | 2 | 2 | 0 |
| Dashboard — heading & content | 2 | 2 | 0 |
| Dashboard — navigation menu | 1 | 1 | 0 |
| Dashboard — widgets | 1 | 1 | 0 |
| Dashboard — refresh | 1 | 1 | 0 |
| Dashboard — access control | 1 | 1 | 0 |
| Dashboard — navigation & return | 1 | 1 | 0 |
| Dashboard — browser back | 1 | 0 | 1 |
| **Total** | **20** | **19** | **1** |

## 11. Ambiguities and Open Questions

1. **DASH-008 — Expected behavior of browser back navigation**
   - The expected result says: "Navigation respects the application's session and access-control behavior."
   - This is vague. Does the user expect to stay on the dashboard? Be redirected to login? See a specific page?
   - **Action:** Clarify with the test author before implementation.

2. **Validation mechanism for empty fields (LOGIN-005, LOGIN-006, LOGIN-007)**
   - The expected results say "appropriate validation is displayed" but do not specify inline client-side validation vs. server-side error messages.
   - **Pre-implementation check:** Verify against the live OrangeHRM demo to determine the actual validation behavior.

3. **LOGIN-003 / LOGIN-004 error message text**
   - The expected result says "an appropriate error message is displayed" without specifying the exact text.
   - **Pre-implementation check:** Verify the actual error message text on the demo site for invalid credentials.

4. **DASH-002 dashboard heading text**
   - The expected result says "Dashboard heading or other agreed identifying content."
   - **Pre-implementation check:** Verify the exact heading text (likely "Dashboard") on the live application.

5. **DASH-003 expected navigation menu items**
   - The workbook does not list which menu items should appear.
   - **Pre-implementation check:** Verify the actual menu items on the live dashboard (e.g., Admin, PIM, Leave, Time, Recruitment, etc.).

6. **DASH-004 expected widgets**
   - The workbook does not specify which widgets/panels are expected.
   - **Pre-implementation check:** Verify the actual widget set on the live dashboard.

7. **LOGIN-009 — Retry behavior / rate limiting**
   - The test assumes the demo permits repeated login attempts. If the demo has rate limiting, CAPTCHA, or account lockout, this test may be blocked.
   - **Pre-implementation check:** Verify the demo's retry behavior.

## 12. Proposed POM Classes

Based on the analyzed test cases, the following Page Object Model classes are needed:

| POM Class | Pages / Features | Tests Using It |
|---|---|---|
| `LoginPage` | Login form, username, password, login button, error messages, password field type | LOGIN-001 through LOGIN-010, DASH-006 |
| `DashboardPage` | Dashboard heading, URL, navigation menu, widgets | LOGIN-002, LOGIN-009, LOGIN-010, DASH-001 through DASH-010 |

*Note: Reuse any existing POM files before creating new ones. Verify the repository's `pages/` directory during implementation.*

## 13. Implementation Order (Recommended)

1. **Phase 1 — Core authentication** (High priority, 6 tests)
   - LOGIN-001, LOGIN-002, LOGIN-003, LOGIN-004, DASH-001, DASH-006

2. **Phase 2 — Validation and edge cases** (Medium priority, 11 tests)
   - LOGIN-005, LOGIN-006, LOGIN-007, LOGIN-008, LOGIN-009, LOGIN-010, DASH-002, DASH-003, DASH-004, DASH-005, DASH-009

3. **Phase 3 — Extended dashboard** (Low priority, 2 tests + 1 pending clarification)
   - DASH-007, DASH-010
   - DASH-008 (pending clarification)

## 14. Execution Strategy

1. Verify POM locators against the live OrangeHRM demo before writing tests.
2. Generate tests in JavaScript `.spec.js` files.
3. Validate syntax and test discovery with `npx playwright test --list`.
4. Run focused tests per phase.
5. Diagnose failures and heal as needed.
6. Document actual pass/fail/blocked results.

## 15. Risks and Assumptions

| Risk / Assumption | Impact | Mitigation |
|---|---|---|
| Public demo site may change or be unavailable | Tests may fail | Verify site availability before runs; capture failure evidence |
| Demo credentials may change | Auth tests fail | Use env variables for credentials |
| Rate limiting or CAPTCHA on demo | Retry tests blocked | Verify retry behavior first |
| Shared demo environment affects results | Flaky failures | Run isolated tests; document environmental issues |
| Workbook has no Priority column | Priority is inferred | Confirm priorities with project lead |
| No Precondition column | Preconditions inferred from context | Document inferred preconditions clearly |

## 16. Output Files

- **This test plan:** `artifacts/test-plan.md`
- **Future output:** `artifacts/test-specs/orangehrm-test-spec.json` (if structured JSON spec is required)
- **Future output:** Generated POM files in `pages/`
- **Future output:** Generated tests in `tests/`
- **Future output:** `artifacts/integration-report.md`
- **Future output:** `artifacts/execution-report.md`
- **Future output:** `artifacts/healing-report.md`

---

**Analysis completed:** 20 test cases extracted from 2 sheets, 19 ready for automation, 1 needs clarification.
**Source workbook:** `ManualTests/HRM Manual tests.xlsx`
**Output:** `artifacts/test-plan.md`