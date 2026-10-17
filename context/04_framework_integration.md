# Existing Framework Integration

## Objective

Integrate generated POM classes and tests into the existing framework
without replacing its architecture.

## Discovery Requirements

Inspect the repository and document:
- Framework language and runner
- Playwright version
- Existing directory structure
- Configuration and environment selection
- BasePage and page objects
- Fixtures and setup/teardown
- Browser and page lifecycle
- Shared action utilities
- Logging and reporting
- Screenshot, video, and trace configuration
- Test-data utilities
- Test execution commands

## Integration Rules

1. Use actual discovered paths.
2. Reuse existing fixtures and lifecycle management.
3. Reuse shared utilities only when their behavior is appropriate.
4. Do not introduce redundant action wrappers.
5. Follow the existing configuration mechanism.
6. Keep credentials in environment variables or the approved
   secret-management mechanism.
7. Do not create a second configuration system.
8. Do not replace the existing reporting framework.
9. Do not install packages without checking whether they are needed.
10. Preserve unrelated tests and existing behavior.

## Example Integration Mapping

Logical component | Integration responsibility

LoginPage:
- Login page locators and interactions.

DashboardPage:
- Dashboard locators and reusable navigation.

Login test:
- Test steps and authentication assertions.

Dashboard test:
- Dashboard-specific expectations.

Existing fixtures:
- Create and provide the browser/page using current conventions.

Existing configuration:
- Provide base URL, username, and password securely.

Existing reporting:
- Capture results and failure evidence.

This is a logical mapping, not a claim that these files already exist.

## Validation

Before execution:
- Check imports and references.
- Check syntax and type errors where applicable.
- Check that required configuration exists.
- Check that POM methods match their call sites.
- Check that the test runner discovers the new tests.

Execute the smallest relevant test scope first.

Run relevant regression tests when feasible.

## Required Report

Include:
- Files created, modified, and reused.
- Existing utilities and fixtures integrated.
- Dependencies added, if any.
- Validation commands and outcomes.
- Test execution results.
- Risks and unresolved issues.

Do not report an unexecuted test as passing.