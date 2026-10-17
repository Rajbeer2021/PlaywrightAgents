---
name: javascript-playwright-tests
description: Generate reliable JavaScript test cases for OrangeHRM using @playwright/test, existing Page Objects, and approved test specifications.
---

# Skill: JavaScript Playwright Test Generation

## 1. Purpose

Generate maintainable JavaScript automated tests for the OrangeHRM demo application.

Use:
- Node.js
- JavaScript
- `@playwright/test`
- Page Object Model
- Playwright fixtures
- Web-first assertions

Do not generate Python or TypeScript test files unless the user explicitly changes the target language.

## 2. Required inputs

Before generating a test, obtain:

1. Approved test specification.
2. Test Case ID and title.
3. Preconditions and test data.
4. Expected results.
5. Existing page objects and framework conventions.
6. Existing `playwright.config.js`.

If any important expected result is unclear, request clarification instead of inventing behavior.

## 3. Test generation procedure

1. Confirm the test specification is marked `READY`.
2. Inspect existing tests and determine whether they use CommonJS (`require`) or ES modules (`import`).
3. Inspect existing POMs and fixtures.
4. Reuse existing page objects and helpers wherever possible.
5. Create one test or a logical test group per approved scenario.
6. Preserve the original Test Case ID in the test title or project-standard metadata.
7. Add assertions for the documented expected results.
8. Validate imports, syntax, and test discovery.
9. Execute the test if execution tools are available.
10. Report actual results without assuming success.

## 4. Example: valid OrangeHRM login

### Example page object

`pages/LoginPage.js`

```javascript
class LoginPage {
  constructor(page) {
    this.page = page;

    // Confirm these locators against the current application DOM.
    this.usernameInput = page.getByRole('textbox', {
      name: 'Username'
    });

    this.passwordInput = page.locator('input[name="password"]');

    this.loginButton = page.getByRole('button', {
      name: 'Login'
    });
  }

  async goto() {
    await this.page.goto(
      'https://opensource-demo.orangehrmlive.com/web/index.php/auth/login'
    );
  }

  async login(username, password) {
    await this.usernameInput.fill(username);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}

module.exports = { LoginPage };
```

The password locator is an example based on a common OrangeHRM form structure. Inspect the live DOM before accepting it.

### Example dashboard page object

`pages/DashboardPage.js`

```javascript
class DashboardPage {
  constructor(page) {
    this.page = page;

    this.heading = page.getByRole('heading', {
      name: 'Dashboard'
    });
  }

  async expectLoaded() {
    const { expect } = require('@playwright/test');

    await expect(this.page).toHaveURL(
      /\/web\/index\.php\/dashboard\/index/
    );

    await expect(this.heading).toBeVisible();
  }
}

module.exports = { DashboardPage };
```

### Example test

`tests/login.spec.js`

```javascript
const { test, expect } = require('@playwright/test');
const { LoginPage } = require('../pages/LoginPage');
const { DashboardPage } = require('../pages/DashboardPage');

test('TC_LOGIN_001 - valid login opens dashboard', async ({ page }) => {
  const loginPage = new LoginPage(page);
  const dashboardPage = new DashboardPage(page);

  const username = process.env.ORANGEHRM_USERNAME;
  const password = process.env.ORANGEHRM_PASSWORD;

  if (!username || !password) {
    throw new Error(
      'Set ORANGEHRM_USERNAME and ORANGEHRM_PASSWORD before running tests.'
    );
  }

  await loginPage.goto();
  await loginPage.login(username, password);

  await expect(page).toHaveURL(
    /\/web\/index\.php\/dashboard\/index/
  );

  await dashboardPage.expectLoaded();
});
```

The example intentionally uses environment variables rather than embedding credentials in test code.

## 5. Example: invalid login

Only generate this test if the approved manual test case requires invalid-login validation and defines the expected error behavior.

```javascript
test('TC_LOGIN_002 - invalid login shows an error', async ({ page }) => {
  const loginPage = new LoginPage(page);

  await loginPage.goto();
  await loginPage.login(
    process.env.ORANGEHRM_USERNAME || 'Admin',
    'invalid-password-for-test'
  );

  await expect(page).toHaveURL(/\/auth\/login/);

  // Replace with the exact verified error locator and message
  // after inspecting the application's current behavior.
  await expect(loginPage.errorMessage).toBeVisible();
});
```

Do not use `loginPage.errorMessage` until that locator has been implemented and verified in `LoginPage.js`. Do not invent an error message.

## 6. Locator rules

Prioritize:
1. `getByRole()`
2. `getByLabel()`
3. `getByPlaceholder()`
4. `getByText()` when the text is a meaningful, stable identifier
5. `getByTestId()` when a test ID exists
6. CSS or XPath only when stronger options are unsuitable and the selector is verified

Avoid:
- Guessing selectors.
- Fragile absolute XPath expressions.
- Using `nth()` without a clear reason.
- Adding arbitrary `waitForTimeout()` calls to mask timing problems.

## 7. Assertion rules

Use assertions that match the expected result:

```javascript
await expect(page).toHaveURL(/dashboard\/index/);
await expect(page.getByRole('heading', {
  name: 'Dashboard'
})).toBeVisible();
```

Use `await` with Playwright's asynchronous web assertions.

Do not:
- Assert only that a click completed.
- Weaken an assertion just to make a failing test pass.
- Replace an expected result with a different one without approval.

## 8. File and naming conventions

Follow the existing repository conventions.

If the framework is new, a reasonable default is:
- Page objects: `pages/LoginPage.js`
- Tests: `tests/login.spec.js`
- Shared configuration: `playwright.config.js`

Do not overwrite existing files without inspecting them first.

## 9. Completion checklist

- [ ] Correct Test Case ID included.
- [ ] JavaScript syntax validated.
- [ ] Existing POMs and fixtures reused.
- [ ] Locators verified or explicitly marked unverified.
- [ ] Assertions reflect the approved expected results.
- [ ] Credentials are not committed to source control.
- [ ] Test execution status reported truthfully.
