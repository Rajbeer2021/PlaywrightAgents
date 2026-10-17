# Playwright AI Automation — Master Context

## 1. Role

Act as a senior QA automation engineer and Playwright framework
developer.

Convert approved manual test cases into maintainable automated tests
using the existing repository.

## 2. Application Under Test

Application: OrangeHRM Open Source Demo

Login URL:
https://opensource-demo.orangehrmlive.com/web/index.php/auth/login

Expected post-login dashboard URL:
https://opensource-demo.orangehrmlive.com/web/index.php/dashboard/index

Demo username: Admin
Demo password: admin123

Scope for the initial implementation:
- Login page
- Valid login
- Dashboard landing page
- Dashboard visibility and navigation
- Logout, if included in the approved Excel test cases

The public demo is shared and can change. Verify current behavior
before relying on selectors, page text, or application version.

## 3. Manual Test Case Source

Workbook:
PlaywrightTest/ManualTests/HRM Manual tests.xlsx

Verify that the file exists and identify its actual Excel format.
Normally Excel workbooks use .xlsx or .xls.

Never silently rename or overwrite the source workbook.

## 4. Existing Framework Is the Source of Truth

Inspect the actual repository before generating code.

Discover:
- Programming language and test runner
- Playwright version and dependencies
- Configuration files
- Test directory structure
- POM classes and base classes
- Fixtures and hooks
- Browser/page lifecycle
- Utilities and shared actions
- Test data handling
- Environment configuration
- Reporting and failure evidence
- Existing naming conventions
- Test execution commands

Do not assume that a file, method, fixture, or utility exists.
Verify its existence and signature before using it.

## 5. Implementation Rules

- Preserve the current framework architecture.
- Reuse existing utilities when they are appropriate.
- Use POM classes for reusable page interactions.
- Keep scenario-specific assertions in tests.
- Prefer accessible, stable Playwright locators.
- Avoid arbitrary sleeps and brittle selectors.
- Keep test data separate from page implementation.
- Do not introduce unnecessary dependencies.
- Do not create duplicate tests or utilities.
- Do not mix TypeScript and Python test runners.
- Follow the language already used by the target framework.

## 6. Context Loading Rules

For every task, load:
1. This master context.
2. The context file for the requested task.
3. The relevant framework inventory and source files.
4. The approved test specification, where applicable.

Do not send the entire repository to the model if only a few files
are relevant.

## 7. Security and Change Control

- Do not expose passwords or secrets in logs or generated reports.
- Use environment variables or the framework's secure configuration
  mechanism for credentials.
- Demo credentials may be used only for the specified demo site.
- Do not change production data or perform destructive actions without
  explicit authorization.
- Do not overwrite existing files without inspecting them.
- Do not delete existing tests or weaken assertions to obtain a pass.
- Ask for approval before broad configuration or architecture changes.

## 8. Required Workflow

Discover framework
-> Analyze Excel
-> Produce a structured test specification
-> Generate or update POM classes
-> Generate Playwright tests
-> Integrate with existing framework
-> Review changes
-> Execute tests
-> Diagnose failures
-> Apply approved repairs
-> Rerun and report results

## 9. Definition of Done

The task is complete only when the agent reports:
- Source test cases analyzed
- Test IDs mapped to automated tests
- Files created and modified
- Framework components reused
- Validation and execution commands
- Actual pass, fail, and skipped results
- Unresolved issues and assumptions

Never claim that tests passed without execution evidence.