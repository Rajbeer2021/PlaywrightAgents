---
name: test-healing
description: Diagnose and repair failing OrangeHRM JavaScript Playwright tests using trace, screenshot, logs, DOM evidence, and minimal verified changes.
---

# Skill: Playwright Test Healing

## 1. Purpose

Diagnose and, where authorized, repair failing OrangeHRM automation tests.

Healing means finding and correcting the underlying problem. It does not mean forcing every test to pass.

## 2. Required evidence

Collect the available evidence:
- Failed test title and Test Case ID
- Error message and stack trace
- Screenshot
- Playwright trace
- Browser console errors, if collected
- Relevant application URL
- Actual DOM or accessible page state
- Relevant test data and environment settings, excluding secret values
- Recent changes to the test, page object, configuration, or application

Do not invent evidence that was not collected.

## 3. Classify the failure

Use one primary classification:

- `LOCATOR_FAILURE`: The locator does not identify the intended element.
- `TIMING_FAILURE`: The test interacts before the required state is ready.
- `ASSERTION_FAILURE`: Actual behavior differs from the expected result.
- `APPLICATION_FAILURE`: The application itself may be broken.
- `TEST_DATA_FAILURE`: Required test data is missing or invalid.
- `ENVIRONMENT_FAILURE`: The application, network, browser, or environment is unavailable.
- `CONFIGURATION_FAILURE`: The test runner or project configuration is incorrect.
- `ISOLATION_FAILURE`: The test depends on state left by another test.
- `UNKNOWN`: Available evidence is insufficient.

## 4. Diagnostic workflow

1. Reproduce the failure using the smallest relevant test.
2. Read the complete error message.
3. Inspect the screenshot or trace, if available.
4. Inspect the current page state and locator.
5. Compare actual behavior with the approved test specification.
6. Classify the failure.
7. Propose the smallest evidence-backed correction.
8. Apply the correction only within the agent's permitted file-editing scope.
9. Rerun the failed test.
10. Run related tests if the change affects shared code.
11. Record the evidence and outcome.

## 5. Example: login button locator failure

Suppose the test fails because this locator cannot find the button:

```javascript
page.getByRole('button', { name: 'Sign in' })
```

The OrangeHRM test specification expects the Login button.

Procedure:
1. Inspect the current login page.
2. Confirm the button's actual accessible name.
3. If the inspected button is named `Login`, correct the locator:

```javascript
page.getByRole('button', { name: 'Login' })
```

4. Rerun the focused test.
5. Record the result.

Do not change the locator based only on the error message if the current DOM has not been inspected.

## 6. Example: timing failure

Avoid adding a fixed delay by default:

```javascript
await page.waitForTimeout(5000);
```

Prefer waiting for the actual expected state:

```javascript
await expect(
  page.getByRole('heading', { name: 'Dashboard' })
).toBeVisible();
```

Playwright's web-first assertions retry while waiting for the expected condition, reducing the need for arbitrary sleeps.

## 7. Assertion failure policy

Suppose the approved test expects the dashboard, but the application remains on the login page.

Do not automatically:
- Remove the dashboard assertion.
- Change the expected URL to the login URL.
- Catch the error and mark the test as passed.
- Retry indefinitely until the test happens to pass.

Instead, inspect whether:
- Credentials were supplied correctly.
- The login action succeeded.
- The application displayed an error.
- The application or environment is unavailable.
- The approved test expectation is still correct.

If the application behavior conflicts with the approved expectation, report the mismatch rather than silently changing the requirement.

## 8. Approval boundaries

The agent may propose a fix and perform permitted low-risk changes when its configuration authorizes them.

Request approval before:
- Changing expected business behavior.
- Deleting or disabling tests.
- Modifying shared authentication or fixture behavior substantially.
- Changing dependency versions or global configuration in a potentially breaking way.
- Rewriting many files to fix a single failure.

## 9. Healing report

Save `artifacts/healing-report.md` with:
- Test Case ID and title
- Original failure
- Failure classification
- Evidence reviewed
- Root cause, or `UNCONFIRMED` if not established
- Files changed
- Reason for each change
- Rerun command
- Actual rerun result
- Remaining risks or open questions

## 10. Completion checklist

- [ ] Failure reproduced or evidence limitations recorded.
- [ ] Failure classified.
- [ ] Root cause supported by evidence.
- [ ] Minimal fix applied or proposed.
- [ ] Assertions not weakened to hide failures.
- [ ] Failed test rerun.
- [ ] Related tests checked where appropriate.
- [ ] Healing report saved.
