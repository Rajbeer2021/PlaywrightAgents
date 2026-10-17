# Playwright Agent Workflow and CLI Contract

## Objective

Execute automation tasks in separate, traceable stages using
task-specific Markdown context files.

## Root Directory

PlaywrightTest/

## Master Context

context/context.md

## Stage 1: Discover

Load:
- context.md
- Relevant repository configuration
- Existing tests, POMs, fixtures, and utilities

Output:
artifacts/analysis/framework_inventory.md

Do not modify application or framework files during discovery.

## Stage 2: Analyze Excel

Load:
- context.md
- 01_testcase_analysis.md
- Excel workbook
- Framework inventory

Output:
artifacts/analysis/test_spec.md

Verify the workbook format before reading it.

Stop and request clarification for unresolved business requirements.

## Stage 3: Generate POM

Load:
- context.md
- 02_pom_generation.md
- Approved test specification
- Relevant existing POM classes and utilities

Output:
New or updated POM classes in the existing framework.

Review the proposed changes before broad modifications.

## Stage 4: Generate Tests

Load:
- context.md
- 03_test_generation.md
- Approved test specification
- Generated or updated POMs
- Relevant existing test examples

Output:
New or updated tests in the existing test directory.

Preserve source test case IDs.

## Stage 5: Integrate

Load:
- context.md
- 04_framework_integration.md
- Framework inventory
- Changed files

Output:
Integrated, reviewed code.

Check syntax, imports, types, test discovery, configuration,
and existing framework compatibility.

## Stage 6: Execute

Use the actual test command discovered from the repository.

Run focused tests before broad regression tests.

Record the exact command, exit code, and observed result.

## Stage 7: Heal

Load:
- context.md
- 05_test_healing.md
- Failure logs
- Relevant screenshots and traces
- Failing test and POM source

Diagnose before editing.

Follow the configured approval policy.
Do not repeat unchanged failing attempts indefinitely.

## Stage 8: Final Report

Save a report under artifacts/.

Include:
- Original test case IDs
- Generated test names and paths
- Changed files
- Tests executed
- Pass/fail/skipped counts
- Healing actions
- Remaining failures and questions

Never invent a result.

## State Management

Persist stage outputs to disk.
Do not rely on the conversation history alone.
Reuse validated outputs instead of regenerating unchanged files.