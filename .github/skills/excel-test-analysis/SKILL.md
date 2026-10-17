---
name: excel-test-analysis
description: Read and analyze manual test cases from Excel for the OrangeHRM demo site, producing structured, traceable test specifications for JavaScript Playwright automation.
---

# Skill: Excel Test Case Analysis

## 1. Purpose

Convert manual QA test cases into clear, structured test specifications that can be implemented using JavaScript, Playwright Test, and Page Object Model (POM).

## 2. Application under test

- Application: OrangeHRM demo
- Login URL: https://opensource-demo.orangehrmlive.com/web/index.php/auth/login
- Username for the authorized demo environment: `Admin`
- Password for the authorized demo environment: `admin123`
- Expected dashboard URL pattern: `/web/index.php/dashboard/index`

Do not assume every test case is a login test. Follow the steps and expected results documented in the source workbook.

## 3. Input requirements

1. Locate the manual test workbook in `ManualTests/`.
2. Confirm its actual filename, extension, and available worksheets.
3. Identify the columns containing:
   - Test Case ID
   - Test Case Title
   - Preconditions
   - Test Steps
   - Test Data
   - Expected Results
   - Priority
4. If a column is absent, do not invent its contents. Record it as unavailable.

## 4. Analysis procedure

1. Read each test case and preserve its original ID.
2. Separate preconditions, actions, test data, and expected outcomes.
3. Identify the OrangeHRM page or feature involved.
4. Map each step to a possible UI action or assertion.
5. Identify dependencies, authentication requirements, and test-data requirements.
6. Flag ambiguous or contradictory cases for clarification.
7. Produce a structured specification before generating code.

## 5. Example

### Input manual test case

- Test Case ID: `TC_LOGIN_001`
- Title: Verify login with valid credentials
- Preconditions: OrangeHRM login page is accessible
- Steps:
  1. Open the login page.
  2. Enter username `Admin`.
  3. Enter password `admin123`.
  4. Click Login.
- Expected Results:
  1. Login succeeds.
  2. The dashboard page is displayed.
  3. The Dashboard heading is visible.

### Expected structured specification

```json
{
  "testCaseId": "TC_LOGIN_001",
  "title": "Verify login with valid credentials",
  "feature": "Authentication",
  "priority": "Not specified",
  "preconditions": [
    "OrangeHRM login page is accessible"
  ],
  "testData": {
    "username": "Admin",
    "password": "admin123"
  },
  "steps": [
    {
      "action": "Navigate to the login page",
      "expectedResult": "The login form is displayed"
    },
    {
      "action": "Enter valid credentials and submit",
      "expectedResult": "The user is authenticated"
    },
    {
      "action": "Verify the dashboard",
      "expectedResult": "The dashboard URL and heading are correct"
    }
  ],
  "automationStatus": "READY"
}
```

This JSON is an example of the skill's output, not a claim that the workbook has already been read.

## 6. Readiness statuses

- `READY`: The requirements and expected outcomes are sufficiently clear.
- `NEEDS_CLARIFICATION`: The case contains ambiguity or missing expected results.
- `NOT_SUITABLE`: The case cannot reasonably be automated with the available UI or tools.

## 7. Rules

- Preserve the original test case ID and intent.
- Never invent expected results or application requirements.
- Do not mark an unexecuted test as passed.
- Do not include passwords in generated reports or logs.
- If workbook parsing fails, report the actual error instead of fabricating test cases.

## 8. Output

Save analyzed specifications under `artifacts/test-specs/`, using JSON or Markdown according to the agent's implementation.

Suggested output filename: `orangehrm-test-spec.json`.

## 9. Completion checklist

- [ ] Correct workbook and worksheet identified.
- [ ] Original test IDs preserved.
- [ ] Steps and expected results separated.
- [ ] Missing information flagged.
- [ ] Structured specification saved.
