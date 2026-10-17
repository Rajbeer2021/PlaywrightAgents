---
name: pom-design
description: Design and maintain reusable JavaScript Page Object Model classes for the OrangeHRM demo site using verified locators and existing framework conventions.
---

# Skill: Page Object Model Design

## 1. Purpose

Create reusable JavaScript page objects that separate:
- UI locators
- Page-level actions
- Page-specific state checks

Test cases should describe the scenario and assertions. Page objects should provide reusable interactions with the application.

## 2. OrangeHRM page inventory

Start with the pages required by the approved test cases.

| Page | Suggested class | Responsibility |
|---|---|---|
| Login | `LoginPage` | Navigate, enter credentials, submit login |
| Dashboard | `DashboardPage` | Verify dashboard is displayed |
| Admin | `AdminPage` | Admin navigation and approved Admin scenarios |
| PIM | `PimPage` | Personnel-information scenarios |
| Leave | `LeavePage` | Leave-management scenarios |

The table is a suggested design, not confirmation that every page has been inspected or that all listed features are in scope.

## 3. Design procedure

1. Inspect the existing `pages/` directory.
2. Reuse an existing page object if it already covers the required page.
3. Inspect the current DOM or use a verified locator source.
4. Choose stable locators.
5. Add methods for meaningful page actions.
6. Keep scenario-specific assertions in tests unless the framework already standardizes page-level assertion methods.
7. Avoid duplicate classes, redundant wrappers, and unnecessary abstractions.
8. Export the class using the repository's current module system.
9. Validate imports and run affected tests.

## 4. Example: LoginPage

`pages/LoginPage.js`

```javascript
class LoginPage {
  constructor(page) {
    this.page = page;
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

Before accepting the page object, verify that each locator matches the current OrangeHRM login form. If the accessible name or DOM structure differs, inspect and correct the locator using evidence.

## 5. Example: DashboardPage

`pages/DashboardPage.js`

```javascript
class DashboardPage {
  constructor(page) {
    this.page = page;
    this.heading = page.getByRole('heading', {
      name: 'Dashboard'
    });
  }
}

module.exports = { DashboardPage };
```

Keep this class small until actual scenarios require more dashboard behavior.

## 6. Example: adding a page method

If an approved test case requires opening the Admin section, inspect the navigation element first. Then add a method such as:

```javascript
async openAdmin() {
  await this.page.getByRole('link', { name: 'Admin' }).click();
}
```

Use this method only after verifying that the actual application exposes the expected link and accessible name.

## 7. Locator selection policy

Prefer locators based on accessible roles and names when they correctly identify the element.

Use CSS selectors when:
- A stable semantic locator is unavailable.
- The selector is grounded in inspected DOM evidence.
- The selector is specific enough to identify the intended element.

Avoid storing locators in tests if they are shared across many tests and belong naturally in a page object.

## 8. POM responsibilities

A page object may:
- Navigate to its page.
- Fill fields.
- Click buttons and links.
- Select options.
- Expose locators for assertions.
- Provide reusable page-specific actions.

A page object should not:
- Read the manual workbook.
- Decide which test cases should exist.
- Silently change test requirements.
- Catch and ignore test failures.
- Store production passwords in source code.

## 9. Completion checklist

- [ ] Existing page objects inspected.
- [ ] No unnecessary duplicate classes created.
- [ ] Locators verified against application evidence.
- [ ] Class and file naming follow project conventions.
- [ ] Page methods represent reusable actions.
- [ ] Imports and affected tests validated.
