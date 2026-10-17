# Playwright Page Object Model Generation

## Objective

Create maintainable POM classes for the approved OrangeHRM test
specification.

## Before Generating Code

Inspect the existing repository for:
- Login page classes
- Dashboard page classes
- BasePage or shared page abstractions
- Existing locator utilities
- Existing action wrappers
- Existing test fixtures
- Naming and folder conventions

If an equivalent POM exists, update it only when necessary.
Do not create duplicate classes.

## LoginPage Responsibilities

Represent the OrangeHRM login page.

Potential locators:
- Username textbox
- Password textbox
- Login button
- Login error message

Potential methods:
- navigate()
- enterUsername(username)
- enterPassword(password)
- clickLogin()
- login(username, password)

These names are proposed interfaces, not proof that matching
methods already exist in the repository.

## DashboardPage Responsibilities

Represent the authenticated dashboard.

Potential locators:
- Dashboard heading
- User profile menu
- Logout menu item

Potential methods:
- verifyDashboardLoaded() — only if this abstraction fits the
  existing framework
- openUserMenu()
- logout()

Keep business assertions in tests unless the current framework
deliberately centralizes them elsewhere.

## Locator Strategy

Prefer:
1. getByRole()
2. getByLabel()
3. getByTestId()
4. Other stable, evidence-backed locators

Inspect the live DOM or existing application evidence before
choosing selectors.

Do not fabricate locators or rely on dynamic CSS classes without
a reason.

## POM Quality Rules

- One page class per logical page or component, as appropriate.
- Keep locators and reusable interactions in page objects.
- Avoid duplicating framework-wide actions.
- Use built-in Playwright waiting behavior.
- Avoid fixed sleeps.
- Avoid storing scenario-specific expected results in page objects.
- Preserve existing method signatures unless a change is required.

## Required Output

For each POM, document:
- File path
- Class name
- Responsibility
- Locators and evidence
- Methods and signatures
- Shared utilities reused
- Dependencies
- Unresolved questions

Review imports, syntax, types, and compatibility with the
installed framework before integrating the class.