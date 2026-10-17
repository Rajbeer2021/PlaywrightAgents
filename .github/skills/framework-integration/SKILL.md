---
name: framework-integration
description: Integrate generated OrangeHRM JavaScript Playwright tests into an existing project while preserving configuration, fixtures, reporting, naming, and coding conventions.
---

# Skill: Playwright Framework Integration

## 1. Purpose

Integrate generated page objects and tests into the existing JavaScript Playwright Test framework safely.

The existing repository is the source of truth. Do not assume that it is empty or that it uses a particular module system.

## 2. Inspect before changing files

Inspect:
- `package.json`
- `playwright.config.js`
- Existing test directories
- Existing page objects
- Shared fixtures
- Utilities and helper functions
- Environment configuration
- Reporters and report output paths
- Git status and existing uncommitted changes

Record the framework structure in `artifacts/framework-inventory.md`.

## 3. Integration procedure

1. Detect whether the project uses CommonJS or ES modules.
2. Identify the configured test directory and filename patterns.
3. Inspect how existing tests import page objects and fixtures.
4. Reuse existing utilities and conventions.
5. Add or update the smallest necessary set of files.
6. Do not replace the Playwright configuration wholesale.
7. Do not install dependencies already available.
8. Check for syntax errors and broken imports.
9. Confirm Playwright discovers the generated test.
10. Execute the focused test and, where appropriate, the regression suite.

## 4. Example project configuration

For a new project only, a basic `playwright.config.js` might look like:

```javascript
const { defineConfig } = require('@playwright/test');

module.exports = defineConfig({
  testDir: './tests',
  timeout: 30_000,
  expect: {
    timeout: 5_000
  },
  reporter: [
    ['list'],
    ['html', { open: 'never' }]
  ],
  use: {
    baseURL: 'https://opensource-demo.orangehrmlive.com',
    browserName: 'chromium',
    headless: true,
    trace: 'retain-on-failure',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure'
  }
});
```

This is a baseline example, not a replacement to apply automatically to an existing configuration. Preserve existing browser projects, reporters, retries, authentication setup, and other settings unless a change is necessary and approved.

## 5. Environment variables

PowerShell example:

```powershell
$env:ORANGEHRM_USERNAME = "Admin"
$env:ORANGEHRM_PASSWORD = "admin123"
```

These commands set variables for the current PowerShell session. Do not print their values in agent logs.

For a longer-lived setup, use the project's approved secrets or environment-management mechanism. Ensure `.env` files containing secrets are excluded from Git.

## 6. Run and validate tests

First list discovered tests:

```powershell
npx playwright test --list
```

Run one test file:

```powershell
npx playwright test tests/login.spec.js
```

Run with a visible browser:

```powershell
npx playwright test tests/login.spec.js --headed
```

Open the HTML report after execution:

```powershell
npx playwright show-report
```

These commands assume Playwright Test is installed and the test file exists at the specified path.

## 7. Safe file operations

Before changing a file:
- Read its current contents.
- Identify the exact change needed.
- Preserve unrelated code.
- Avoid overwriting user changes.
- Explain potentially breaking changes.
- Validate the result after editing.

If a command or execution capability is unavailable, report the limitation instead of claiming it ran.

## 8. Completion report

Produce `artifacts/integration-report.md` containing:
- Files created
- Files updated
- Dependencies changed, if any
- Syntax/import validation
- Test discovery result
- Execution command
- Actual test result
- Known limitations

## 9. Completion checklist

- [ ] Existing framework inspected.
- [ ] Project conventions preserved.
- [ ] No unnecessary dependencies added.
- [ ] Generated tests are discoverable.
- [ ] Configuration changes are minimal.
- [ ] Focused test execution attempted when tools permit.
- [ ] Actual results recorded.
