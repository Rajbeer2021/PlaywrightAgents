---
name: orangehrm-agent-workflow
description: Orchestrate the complete OrangeHRM automation lifecycle from manual Excel test cases to JavaScript Playwright tests, framework integration, execution, and evidence-based healing.
---

# Skill: OrangeHRM Automation Agent Workflow

## 1. Purpose

Coordinate the other skills to automate approved OrangeHRM test cases.

The AI agent may be implemented in Python and use a local Ollama model, while the generated browser tests remain JavaScript files using `@playwright/test`.

## 2. Target application

- Login URL: https://opensource-demo.orangehrmlive.com/web/index.php/auth/login
- Username: `Admin`
- Password: `admin123`
- Expected successful-login destination: `/web/index.php/dashboard/index`

Use the supplied demo credentials only for the intended demo environment. Load credentials from environment variables when executing tests.

## 3. Workflow stages

### Stage 1: Discover the project

Load:
- `context/context.md`
- `context/06_agent_workflow.md`
- `framework-integration/SKILL.md`

Actions:
1. Inspect the repository.
2. Identify the JavaScript Playwright Test configuration.
3. Identify existing tests, page objects, utilities, fixtures, and reports.
4. Record the project inventory.

Output: `artifacts/framework-inventory.md`

### Stage 2: Analyze manual tests

Load:
- `context/01_testcase_analysis.md`
- `excel-test-analysis/SKILL.md`

Actions:
1. Locate the actual Excel workbook.
2. Read its worksheets and test case columns.
3. Extract IDs, steps, data, and expected results.
4. Identify ambiguous cases.
5. Save the approved or reviewable specification.

Output: `artifacts/test-specs/orangehrm-test-spec.json`

Do not continue to code generation for a test marked `NEEDS_CLARIFICATION` until its requirements are resolved.

### Stage 3: Design page objects

Load:
- `context/02_pom_generation.md`
- `pom-design/SKILL.md`

Actions:
1. Inspect existing page objects.
2. Identify required pages.
3. Verify selectors using application evidence.
4. Reuse or create the smallest necessary page objects.
5. Validate the page-object imports and syntax.

Output: the appropriate files under `pages/`.

### Stage 4: Generate tests

Load:
- `context/03_test_generation.md`
- `javascript-playwright-tests/SKILL.md`

Actions:
1. Read the approved test specification.
2. Generate JavaScript `.spec.js` files.
3. Reuse page objects and fixtures.
4. Add assertions for the expected results.
5. Preserve the Test Case IDs.
6. Check syntax and test discovery.

Output: the appropriate files under `tests/`.

### Stage 5: Integrate the tests

Load:
- `context/04_framework_integration.md`
- `framework-integration/SKILL.md`

Actions:
1. Check module format and imports.
2. Preserve existing configuration and reporting.
3. Verify test discovery.
4. Avoid duplicate utilities or unnecessary dependencies.

Output: updated project and `artifacts/integration-report.md`.

### Stage 6: Execute tests

Use the available terminal or test-execution tool.

Example:

```powershell
npx playwright test --list
npx playwright test tests/login.spec.js
```

Capture the actual exit code, console output, and available reports.

Possible execution statuses:
- `PASSED`
- `FAILED`
- `BLOCKED`
- `NOT_EXECUTED`

A generated test is not a passed test. Only actual execution can establish its result.

### Stage 7: Heal failures

Load:
- `context/05_test_healing.md`
- `test-healing/SKILL.md`

Actions:
1. Collect failure evidence.
2. Classify the failure.
3. Propose or apply an authorized, evidence-backed fix.
4. Rerun the failed test.
5. Record the outcome and remaining issues.

Output: `artifacts/healing-report.md`

### Stage 8: Summarize the run

Produce a final summary containing:
- Source workbook
- Number of test cases analyzed
- Number ready for automation
- Number of test files generated
- Number executed
- Passed, failed, blocked, and not-executed counts
- Files created or modified
- Report locations
- Outstanding questions

Never fabricate counts or claim execution that did not happen.

## 4. Skill selection rules

| User request | Skill to load |
|---|---|
| Read Excel test cases | `excel-test-analysis` |
| Create or update page objects | `pom-design` |
| Generate JavaScript tests | `javascript-playwright-tests` |
| Add code to the existing framework | `framework-integration` |
| Diagnose a failed test | `test-healing` |
| Automate the entire process | `orangehrm-agent-workflow` |

Load the corresponding context file as well as the selected skill.

## 5. Agent safety rules

- Inspect files before editing.
- Preserve existing framework conventions.
- Never invent requirements or test evidence.
- Do not log passwords.
- Do not disable assertions to make tests pass.
- Do not overwrite unrelated user changes.
- Ask for clarification when requirements are ambiguous.
- Report unavailable tools or failed commands honestly.

## 6. Completion criteria

The workflow is complete only when each requested stage has either:
- Finished with its output recorded, or
- Been explicitly marked as blocked, not executed, or awaiting clarification.
