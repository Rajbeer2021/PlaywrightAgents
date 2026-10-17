# Playwright Test Generation

## Objective

Generate automated tests from the approved test specification and
the POM classes.

## Inputs

- context.md
- 01_testcase_analysis.md
- 02_pom_generation.md
- Approved test specification
- Existing framework inventory
- Relevant POM classes and test examples

## Rules

1. Preserve every original test case ID.
2. Map each automated test to its source test case.
3. Reuse POM methods rather than duplicating page interactions.
4. Follow the existing test runner and language.
5. Use descriptive test names.
6. Use explicit, meaningful assertions.
7. Avoid unnecessary dependencies between tests.
8. Avoid shared mutable state between test cases.
9. Do not embed passwords directly in generated test code.
10. Do not create tests for unresolved requirements without approval.

## Example: Successful Login

Test ID: TC_LOGIN_001

Title: Verify successful login with valid credentials

Preconditions:
- The login page is accessible.
- Credentials are available from approved configuration.

Steps:
1. Open the login page.
2. Enter the configured username.
3. Enter the configured password.
4. Click Login.
5. Verify the dashboard URL.
6. Verify the Dashboard heading is visible.

Expected result:
The user is authenticated and the dashboard is displayed.

## Example: Dashboard Verification

Test ID: TC_DASHBOARD_001

Title: Verify dashboard after successful login

Steps:
1. Authenticate using the existing login POM.
2. Verify the dashboard URL.
3. Verify a stable dashboard heading or landmark.
4. Verify any additional widget only if required by the
   approved manual test case.

Expected result:
The dashboard is accessible and the expected elements are visible.

## Example TypeScript Test Pattern

Use this pattern only if the existing framework uses
TypeScript and @playwright/test.

import { test, expect } from '@playwright/test';
import { LoginPage } from '../pages/LoginPage';
import { DashboardPage } from '../pages/DashboardPage';

test('TC_LOGIN_001 - successful login', async ({ page }) => {
  const loginPage = new LoginPage(page);
  const dashboardPage = new DashboardPage(page);

  await loginPage.navigate();
  await loginPage.login(
    process.env.ORANGEHRM_USERNAME!,
    process.env.ORANGEHRM_PASSWORD!
  );

  await expect(page).toHaveURL(
    /\/web\/index\.php\/dashboard\/index/
  );

  await expect(dashboardPage.dashboardHeading).toBeVisible();
});

The sample assumes these POM interfaces exist. Adapt imports,
methods, and locators to the actual framework before using it.

## Assertions

Use web-first assertions where supported by the framework.

Examples:
- URL matches the dashboard route.
- Dashboard heading is visible.
- An approved dashboard landmark is visible.

Do not assert exact widget counts, employee data, or dashboard
contents unless the manual test case specifies them.

## Output

Generate tests in the existing test directory.

Record:
- Source test case ID
- Test file and test name
- POM methods used
- Assertions implemented
- Any omitted requirements and why

Do not report success until the tests have been executed.