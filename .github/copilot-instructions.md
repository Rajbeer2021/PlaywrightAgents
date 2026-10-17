# OrangeHRM Playwright Automation — Project Instructions

## 1. Purpose and Technology

This repository automates the OrangeHRM Open Source Demo application.

- **Login URL:** https://opensource-demo.orangehrmlive.com/web/index.php/auth/login
- **Dashboard URL:** https://opensource-demo.orangehrmlive.com/web/index.php/dashboard/index
- **Language:** JavaScript
- **Test framework:** `@playwright/test`
- **Design pattern:** Page Object Model (POM)

Use the existing framework and conventions. Do not introduce TypeScript, Python, another test runner, or a replacement framework unless explicitly requested.

## 2. Required Project Context

Before significant work, read:

- `context/context.md`
- `context/06_agent_workflow.md`

Also read the relevant task-specific context:

- `context/01_testcase_analysis.md` — Excel test case analysis
- `context/02_pom_generation.md` — Page Object Model design
- `context/03_test_generation.md` — JavaScript test generation
- `context/04_framework_integration.md` — Framework integration
- `context/05_test_healing.md` — Failure diagnosis and healing

Read the corresponding skill under `.github/skills/` when available. Verify that referenced files exist and are accessible. Never claim to have read a file or loaded a skill unless that actually happened.

## 3. Repository Discovery

Before making changes:

1. Inspect the repository structure and relevant existing files.
2. Read `package.json` and identify the installed Playwright version, scripts, dependencies, and module format.
3. Inspect the actual Playwright configuration, existing tests, page objects, fixtures, utilities, test data, and reporting configuration.
4. Locate the real manual test workbook if the task requires it.
5. Follow the existing directory structure and naming conventions.

Do not assume files, directories, scripts, dependencies, or configuration settings exist before verifying them. Reuse existing implementations where practical.

## 4. Manual Test Analysis and Traceability

When working from manual test cases:

- Inspect the actual Excel workbook and its worksheets, headers, IDs, preconditions, test data, steps, and expected results.
- Preserve original test case IDs, step order, and expected behavior.
- Maintain traceability between each source test case, the test plan, generated tests, and execution results.
- Never invent requirements, test data, expected results, or application behavior.
- Identify missing, contradictory, or ambiguous information explicitly.
- Use `artifacts/test-plan.md` as the canonical generated test plan unless the user specifies another location.
- Treat a plan as approved only when the user or an explicitly authorized workflow approves it.

If the user requests analysis only, do not generate code. If approval is required, do not proceed to implementation until approval is received. Do not implement cases whose unresolved requirements could change the test's meaning.

## 5. Page Objects and Test Generation

- Use JavaScript and `@playwright/test`.
- Follow the existing POM design and reuse existing page objects, fixtures, and utilities.
- Keep page-specific locators and interactions in page objects when consistent with the framework.
- Keep test scenarios and assertions in test files according to existing conventions.
- Prefer reliable, accessible locators such as `getByRole()` and `getByLabel()` when they identify the intended element correctly.
- Verify selectors using available source code, browser evidence, or actual execution. Do not invent selectors or assume an element exists.
- Use web-first assertions such as `await expect(locator).toBeVisible()` where appropriate.
- Prefer Playwright's automatic waiting and event-driven synchronization over arbitrary fixed waits.
- Use meaningful test titles and preserve source test case IDs.
- Keep tests independent where practical.
- Do not create duplicate utilities, unnecessary dependencies, or parallel implementations of existing functionality.

Do not weaken assertions, remove important validations, or change business expectations merely to make tests pass.

## 6. Test Data and Configuration

- Use environment variables or the project's approved secret-management mechanism for credentials.
- Use `ORANGEHRM_USERNAME` and `ORANGEHRM_PASSWORD` only if compatible with the existing configuration, or implement them consistently when needed.
- Do not assume environment variables are configured; verify their presence without printing their values.
- Use demo credentials only against the intended demo application and only when authorized.
- Never hardcode or log passwords, access tokens, cookies, API keys, private keys, connection strings, or other secrets.
- Never commit credentials, `.env` files, authentication state, or sensitive artifacts.
- Use synthetic or anonymized test data wherever practical.
- Never include secrets or unnecessary personal information in test plans, source files, screenshots, traces, console logs, or reports.

## 7. Confidentiality and External Services

Treat repository files, workbooks, source code, test data, logs, screenshots, traces, and reports as potentially confidential unless explicitly identified as public.

- Use project data only for the requested task.
- Do not upload or transmit project files, credentials, test data, logs, screenshots, or source code to external services or third-party tools unless the user has authorized that specific transfer.
- Before sending project content to an external service, identify what information will be transmitted and obtain authorization if that transfer is not already clearly authorized.
- Do not disclose confidential content in chat responses, generated documentation, test reports, or commits.
- Inspect diagnostic artifacts before sharing them and redact secrets or sensitive data.
- Ask for confirmation before publishing confidential information, exposing sensitive data, or performing actions with significant security or privacy impact.

If a secret is accidentally exposed, do not repeat it. Notify the user without reproducing the value and recommend appropriate revocation or rotation.

## 8. Framework Integration and Validation

Before declaring implementation complete:

1. Check syntax, imports, module compatibility, and test data references.
2. Preserve existing configuration, fixtures, and reporting unless changes are required.
3. Run `npx playwright test --list` or the equivalent project command when applicable.
4. Execute the requested tests when tools, permissions, environment, and application availability allow.
5. Inspect actual execution output and available failure evidence.
6. Record commands executed, results, failures, and validation that could not be completed.
7. Redact sensitive data from output and reports.

A generated test is not a passed test. Test discovery is not test execution. Never report success without supporting execution evidence.

## 9. Failure Diagnosis and Healing

When tests fail:

- Read `context/05_test_healing.md` and the applicable healing skill.
- Inspect the failure message, stack trace, relevant test and page object, and available screenshots, videos, or traces.
- Determine the likely cause from evidence before editing.
- Make the smallest justified change.
- Preserve test intent and meaningful assertions.
- Rerun the focused test and relevant regression tests when possible.
- Document the root cause, changes, results, and remaining issues.

Never blindly change selectors, add arbitrary waits, suppress errors, or skip failing tests to obtain a green run. Ask before changing business expectations, removing assertions, or making broad framework changes.

## 10. Change Safety

- Inspect files before modifying them.
- Do not overwrite unrelated user changes.
- Do not delete files, replace major configuration, or perform destructive operations without confirmation.
- Avoid unrelated refactoring.
- Keep changes focused on the requested task.
- If a required file, tool, skill, or dependency is missing, report the limitation rather than pretending it exists.

## 11. Task Completion Report

At the end of the task, summarize:

- Files and framework components inspected.
- Files created or modified.
- Manual test case IDs covered, where applicable.
- Validation and test commands actually executed.
- Actual passed, failed, blocked, and not-executed counts when known.
- Reports and evidence created.
- Unresolved questions, assumptions, and limitations.
- Relevant security issues or redactions, without revealing secret values.

Follow the user's requested scope. Do not automatically execute every workflow stage when the user asks for planning, analysis, code review, or another limited task.