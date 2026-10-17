Task: Generate JavaScript Playwright tests for OrangeHRM.

Load these files:
- `context/context.md`
- `context/03_test_generation.md`
- `skills/javascript-playwright-tests/SKILL.md`
- `skills/pom-design/SKILL.md`

Read:
- `artifacts/test-plan.md`
- `artifacts/test-specs/orangehrm-test-spec.json`
- Existing files in `pages/`, `tests/`, and `playwright.config.js`

Target application:
https://opensource-demo.orangehrmlive.com/web/index.php/auth/login

Requirements:
1. Use JavaScript and `@playwright/test`.
2. Follow the project's existing CommonJS or ES module format.
3. Reuse existing page objects.
4. Create or update `LoginPage.js` and `DashboardPage.js` only if needed.
5. Preserve the approved test case IDs.
6. Use `ORANGEHRM_USERNAME` and `ORANGEHRM_PASSWORD` environment variables.
7. Assert that successful login reaches the dashboard and that the Dashboard heading is visible.
8. Verify locators against available application evidence.
9. Do not invent requirements or silently overwrite unrelated code.
10. Validate imports and test discovery.
11. Run the generated test if terminal execution is available.

Return a list of created or modified files and report the actual validation and execution results. If a required tool is unavailable, report that limitation explicitly.




--------------
prompt for fixing test

Task: Diagnose and fix the failed OrangeHRM login test.

Load:
- `context/context.md`
- `context/05_test_healing.md`
- `skills/test-healing/SKILL.md`
- `skills/javascript-playwright-tests/SKILL.md`

Failed test:
`TC_LOGIN_001 - valid login opens dashboard`

Failure:
The Login button locator did not resolve to an actionable element.

Required actions:
1. Read the failed test and the relevant page object.
2. Inspect the complete error and stack trace.
3. Inspect the screenshot, trace, or current DOM if available.
4. Determine whether the issue is an incorrect locator, timing, application behavior, configuration, or environment.
5. Verify the current button role and accessible name before changing the locator.
6. Apply the smallest justified fix, within the authorized file-editing scope.
7. Do not remove or weaken the dashboard assertions.
8. Rerun the focused test.
9. Run related tests if shared code was changed.
10. Save the findings to `artifacts/healing-report.md`.

Report the root cause only when supported by evidence. Otherwise mark it `UNCONFIRMED`. Include the actual rerun result and any remaining blockers.




----

prompt for healing test

--

Use the official Playwright Healer agent to investigate the failed OrangeHRM tests.

Read:
- @.github/copilot-instructions.md
- @context/context.md
- @context/05_test_healing.md
- @context/03_test_generation.md

Use the /test-healing skill.

Inspect the actual failure output, stack trace, screenshot, trace, and current DOM where available.

For each failed test:
1. Identify the root cause if the evidence supports one.
2. Distinguish locator, timing, test-data, application, assertion, configuration, and environment failures.
3. Make the smallest justified change.
4. Do not weaken assertions or silently skip a failing test to make the suite green.
5. Rerun the affected test.
6. Run relevant regression tests after shared-code changes.
7. Save the diagnosis and actual rerun result to `artifacts/healing-report.md`.

If the root cause is uncertain, report it as unconfirmed. If a change needs approval, propose the change instead of applying it.