# Playwright Test Healing

## Objective

Diagnose and repair failed automated tests using execution evidence
and minimal, controlled changes.

## Inputs

- Failed test ID and name
- Failure message and stack trace
- Test source
- Related POM class
- Framework utilities and configuration
- Screenshot, video, or trace when available
- Current application state
- Previous healing attempts, if any

## Step 1: Diagnose

Classify the failure:
- Locator failure
- Timeout or synchronization issue
- Incorrect assertion
- Application defect
- Authentication or session problem
- Invalid test data
- Configuration or fixture problem
- Test isolation problem
- Environment outage
- Unclear requirements

Do not assume every failure is a test-code defect.

## Step 2: Gather Evidence

Inspect the failing locator against the current DOM.
Inspect the URL and visible page state.
Inspect available screenshots and traces.
Compare the implementation with the approved test specification.
Inspect recent code changes.

## Step 3: Repair

For a confirmed locator change:
- Use current DOM evidence.
- Prefer a stable accessible locator.
- Update the relevant POM rather than duplicating the fix in tests.

For a synchronization issue:
- Prefer built-in actionability checks and web-first assertions.
- Avoid fixed sleeps unless a justified application-specific
  requirement exists.

For an incorrect assertion:
- Compare it with the manual test case.
- Ask for clarification if the intended result is uncertain.
- Never weaken an assertion just to make the test pass.

For an environment failure:
- Report the issue rather than changing unrelated test logic.

## Step 4: Approval and Safety

Automatically apply only approved, low-risk repairs.

Require approval for:
- Changes to expected business behavior
- Removal or weakening of assertions
- Application-code changes
- Broad framework changes
- Destructive operations
- Uncertain changes to test coverage

Never disable a test simply because it fails.
Never hide errors or fabricate successful execution results.

## Step 5: Verify

1. Rerun the affected test.
2. Capture the new result.
3. Run relevant regression tests where feasible.
4. Compare the original and repaired behavior.
5. Record any remaining flakiness.

## Healing Report

Include:
- Test ID
- Original failure
- Evidence inspected
- Root-cause assessment
- Files changed
- Repair made
- Focused rerun result
- Regression result
- Remaining risks

A repair is verified only to the extent supported by actual
execution evidence.