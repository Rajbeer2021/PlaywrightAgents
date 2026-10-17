# GitHub Copilot Chat Slash Commands: Comprehensive Practical Guide

> **Scope:** This guide explains the slash commands visible in the
> supplied Copilot Chat screenshot: `/clear`, `/fork`, `/rename`,
> `/agents`, `/debug`, `/hooks`, `/instructions`, `/models`, `/plugins`,
> `/prompts`, `/skills`, and `/tools`.
>
> **Important:** Copilot's commands, labels, and available features can
> vary by product (VS Code Copilot Chat, GitHub Copilot CLI, agent
> mode), version, account, and organization policy. The descriptions
> below follow the menu shown in the screenshot. If a command's behavior
> differs in your installed version, use the menu's own description and
> the official documentation linked at the end.

## Contents

1.  [How slash commands work](#1-how-slash-commands-work)
2.  [Quick reference](#2-quick-reference)
3.  [Conversation commands](#3-conversation-commands)
4.  [Agent configuration](#4-agent-configuration)
5.  [Debugging and hooks](#5-debugging-and-hooks)
6.  [Instructions, prompts, and
    skills](#6-instructions-prompts-and-skills)
7.  [Models, plugins, and tools](#7-models-plugins-and-tools)
8.  [How the pieces work together](#8-how-the-pieces-work-together)
9.  [Recommended workflow for a Playwright OrangeHRM
    project](#9-recommended-workflow-for-a-playwright-orangehrm-project)
10. [Troubleshooting and safe-use
    checklist](#10-troubleshooting-and-safe-use-checklist)
11. [Official documentation](#11-official-documentation)

------------------------------------------------------------------------

## 1. How slash commands work

In Copilot Chat, type `/` in the chat input to open the slash-command
menu. Choose a command or type its name, then follow any UI prompt or
enter a request.

Slash commands generally configure the chat experience or help manage
the current conversation. They are not all commands that generate code.
Some open configuration screens; others change the current conversation
or select the model.

**Do not assume that every item is available in every Copilot product.**
Your screenshot shows these 12 menu entries, so this guide focuses on
those entries.

## 2. Quick reference

  -----------------------------------------------------------------------
  Command                 Menu description in the When to use it
                          screenshot              
  ----------------------- ----------------------- -----------------------
  `/clear`                Start a new chat and    Start a clean
                          archive the current one conversation without
                                                  losing the previous
                                                  chat

  `/fork`                 Fork conversation into  Explore an alternative
                          a new chat session      approach while
                                                  preserving the original
                                                  conversation

  `/rename`               Rename this chat        Give a conversation a
                                                  useful, searchable name

  `/agents`               Configure custom agents Create or manage
                                                  task-specialized agents

  `/debug`                Show Chat Debug View    Investigate chat
                                                  behavior, request
                                                  context, or
                                                  troubleshooting details
                                                  exposed by the UI

  `/hooks`                Configure hooks         Configure supported
                                                  actions that run at
                                                  defined points in an
                                                  agent workflow

  `/instructions`         Configure instructions  Manage persistent
                                                  project or user
                                                  guidance for Copilot

  `/models`               Open the model picker   Choose an available AI
                                                  model

  `/plugins`              Manage plugins          Manage supported
                                                  integrations or
                                                  extensions in this
                                                  Copilot experience

  `/prompts`              Configure prompt files  Create or manage
                                                  reusable prompt
                                                  templates

  `/skills`               Configure skills        Manage reusable
                                                  procedural guidance
                                                  that Copilot can use
                                                  for tasks

  `/tools`                Configure tools         Inspect or manage the
                                                  tools available to the
                                                  agent/chat, depending
                                                  on the product
  -----------------------------------------------------------------------

## 3. Conversation commands

### 3.1 `/clear` --- start a clean chat

**What it does:** The screenshot describes this as starting a new chat
and archiving the current one.

**When it is useful** - The current chat has accumulated unrelated
discussion. - You are switching from test planning to a different
task. - You want a fresh context window while retaining the previous
conversation for later reference. - You want to test whether a concise
prompt produces a better result.

**Example** 1. Finish or save any important decisions from the current
chat. 2. Enter `/clear`. 3. Start the new task with a clear goal and
point Copilot to the required project files.

**Example prompt after clearing**

``` text
Read .github/copilot-instructions.md and context/context.md.
Inspect the current Playwright project and report its structure.
Do not modify files yet.
```

**Remember:** A clean chat does not automatically mean your project
files, instructions, skills, or agents have been deleted. It changes the
conversation session as described by your UI.

### 3.2 `/fork` --- branch the conversation

**What it does:** Creates a new chat session forked from the current
conversation.

**When it is useful** - Compare two framework designs. - Try a different
locator strategy without losing the original discussion. - Explore
whether to use a Page Object Model or a simpler test structure. - Ask
for an alternative implementation while preserving the original line of
work.

**Example** Your current conversation is designing an OrangeHRM POM
framework. Use `/fork` and ask:

``` text
Explore an alternative implementation using fewer abstractions.
Compare it with the current POM approach, and do not edit files.
```

**Best practice:** Use a fork for exploration. When you choose an
approach, bring the decision back into the main project instructions or
a tracked design document. A forked chat should not be assumed to merge
changes into another chat automatically.

### 3.3 `/rename` --- give the chat a useful name

**What it does:** Renames the current chat.

**When it is useful** - You have multiple project discussions. - You
want to find the conversation later. - You need to distinguish test
generation from failure healing.

**Useful names** - `OrangeHRM – Playwright Agent Setup` -
`Playwright – POM and Test Generation` -
`OrangeHRM – Login Failure Healing` -
`Copilot CLI – Skills and Instructions`

**Best practice:** Use a project name plus a task or phase. Rename chats
when the work changes significantly.

------------------------------------------------------------------------

## 4. Agent configuration

### 4.1 `/agents` --- configure custom agents

**What it does:** Opens the agent configuration UI shown in the
screenshot.

An agent is a task-oriented assistant profile. Depending on the Copilot
product and version, an agent can have its own description,
instructions, and a selected set of tools. Repository-level agent
profiles are commonly stored under `.github/agents/`; verify the exact
location and supported file format for your installed product.

**When it is useful** - Create an OrangeHRM Playwright coordinator. -
Separate planning, test generation, and healing responsibilities. - Give
a specialist agent instructions to use JavaScript, Playwright Test, and
your existing POM conventions. - Limit a specialist to appropriate tools
where the product supports tool restrictions.

**Suggested agent for the OrangeHRM workflow**

Name: `orangehrm-playwright-agent`

Responsibilities: 1. Read repository instructions and
`context/context.md`. 2. Discover relevant context files and skills
rather than assuming every file exists. 3. Inspect the actual workbook
and framework before making changes. 4. Prepare a test plan before
generating tests. 5. Generate JavaScript tests and POMs using existing
project conventions. 6. Run tests when permitted and report actual
results. 7. Diagnose failures and make minimal, evidence-based fixes. 8.
Never invent workbook content or claim tests passed without execution
evidence.

**Example request to the agent**

``` text
Use the orangehrm-playwright-agent.
Read the repository instructions and relevant context/skills.
Inspect the existing project and find the actual manual test workbook.
Create artifacts/test-plan.md only. Do not generate tests yet.
Report the files you read and any missing inputs.
```

**Agent versus prompt:** An agent is a configured assistant profile for
a class of work. A prompt is a reusable task request. They complement
each other; they are not the same thing.

### 4.2 Agent best practices

-   Give the agent a narrow, testable responsibility.
-   State the language and framework explicitly.
-   Tell it which project instructions and context to inspect.
-   Ask it to inspect the existing code before editing.
-   Require truthful test execution reports.
-   Review permissions and available tools before allowing broad
    changes.
-   Do not assume an agent automatically reads every context file or
    every skill. Explicitly instruct it to discover and read relevant
    files.

------------------------------------------------------------------------

## 5. Debugging and hooks

### 5.1 `/debug` --- show Chat Debug View

**What it does:** Opens the Chat Debug View according to the menu shown.

**When it is useful** - Copilot seems to ignore a project instruction. -
The wrong agent or model appears to be active. - A tool call or agent
response behaves unexpectedly. - You are troubleshooting why context or
configuration does not seem to affect a response.

**How to use it** 1. Reproduce the problem with a small, specific
request. 2. Open `/debug`. 3. Inspect the information the UI exposes
about the conversation or request. 4. Look for missing context,
unexpected configuration, errors, or tool behavior. 5. Correct the
source configuration and retry.

**Important:** Debug views can expose request or workspace details.
Avoid sharing screenshots or logs containing credentials, access tokens,
customer data, or confidential source code.

### 5.2 `/hooks` --- configure hooks

**What it does:** Opens hook configuration in the UI shown.

A hook is a supported action associated with a particular lifecycle
event. Depending on the Copilot product, hooks may run before or after
certain agent actions or at other supported points. The exact events,
configuration format, and supported operations are product-specific;
consult the documentation for your installed version.

**Potential uses** - Run a permitted validation step at a supported
point in an agent workflow. - Apply a local policy or check where
supported. - Add workflow automation that is repeatable and visible.

**For Playwright, possible checks to consider** - Check formatting or
syntax after a code change. - Run `npx playwright test --list` after
test-file generation. - Run a focused test after a change, if supported
and approved. - Verify that generated files stay within the intended
project directory.

These are examples of workflow goals, not a guarantee that every hook
event can run these commands in your installation.

**Safety** - Inspect hook configuration before enabling it. - Keep hooks
narrow and predictable. - Do not put passwords or tokens in hook
files. - Avoid automatically running destructive commands, deleting
files, committing code, or triggering expensive tests without an
appropriate approval process. - Test hooks on a small example first.

------------------------------------------------------------------------

## 6. Instructions, prompts, and skills

These three features are related but solve different problems.

### 6.1 `/instructions` --- persistent guidance

**What it does:** Opens the instructions configuration UI.

Instructions express rules Copilot should follow, such as project
conventions, language choices, coding standards, and safety
requirements. Depending on the product, instructions may be user-level,
repository-level, or path-specific.

**Use instructions for rules that should apply repeatedly**, for
example: - Use JavaScript rather than TypeScript for this project. - Use
`@playwright/test`. - Prefer accessible locators and web-first
assertions. - Reuse existing POMs, fixtures, utilities, and
configuration. - Never invent manual test cases. - Do not hardcode
credentials. - Never claim a test passed unless it was executed
successfully.

**Example repository instructions**

``` markdown
# OrangeHRM Playwright Project Instructions

- Use JavaScript and @playwright/test.
- Follow the existing Page Object Model and framework conventions.
- Read context/context.md and task-specific context files.
- Read relevant .github/skills/*/SKILL.md files before following a specialized workflow.
- Inspect existing code and configuration before editing.
- Preserve the real test IDs, steps, and expected results from the manual workbook.
- Prefer accessible locators and Playwright web-first assertions.
- Avoid fixed sleeps and arbitrary timeouts.
- Keep credentials in environment variables or approved secret storage.
- Run relevant validation and report actual outcomes.
- Do not weaken assertions just to make tests pass.
```

**Best practice:** Put stable, broadly applicable rules in instructions.
Keep long task procedures in skills and task-specific details in context
documents.

### 6.2 `/prompts` --- reusable prompt files

**What it does:** Opens the prompt-file configuration UI.

Prompt files are reusable task templates. They help you avoid rewriting
the same request every time. Their exact extension, location, metadata,
and invocation syntax depend on the Copilot product and version.

**Use prompt files for repeatable tasks**, such as: - Analyze the manual
Excel workbook. - Generate POM classes. - Generate tests from an
approved plan. - Integrate tests with existing framework utilities. -
Diagnose a failing test and prepare a healing report.

**Example reusable test-generation prompt**

``` text
Generate JavaScript Playwright tests from the approved
artifacts/test-plan.md.

Before editing:
1. Read the project instructions.
2. Read context/context.md and context/03_test_generation.md.
3. Read the relevant JavaScript Playwright and POM skills.
4. Inspect existing tests, pages, fixtures, and configuration.

Preserve the approved test IDs and expected results.
Reuse existing framework utilities.
Do not hardcode credentials.
Validate test discovery and run the relevant tests when permitted.
Report every file changed and the actual execution results.
```

**Prompt versus instruction** - Instruction: "Always use JavaScript in
this repository." - Prompt: "Generate tests for the approved login test
plan."

**Prompt versus agent** - Prompt: a reusable request for a particular
task. - Agent: a configured role that can carry out a class of tasks.

### 6.3 `/skills` --- reusable procedures

**What it does:** Opens the skills configuration UI.

A skill is reusable procedural guidance that helps Copilot perform a
particular kind of work. In supported Copilot setups, a skill commonly
consists of a `SKILL.md` file with a name, description, and
instructions. The exact discovery rules vary by product.

**Use skills for detailed, repeatable procedures**, for example:

``` text
.github/skills/
├── excel-test-analysis/
│   └── SKILL.md
├── pom-design/
│   └── SKILL.md
├── javascript-playwright-tests/
│   └── SKILL.md
├── framework-integration/
│   └── SKILL.md
├── test-healing/
│   └── SKILL.md
└── orangehrm-agent-workflow/
    └── SKILL.md
```

**Example skill instructions**

``` markdown
---
name: javascript-playwright-tests
description: Generate and validate JavaScript Playwright tests from an approved test plan using the existing POM framework.
---

# JavaScript Playwright Test Generation

1. Read the approved test plan.
2. Read the project's test-generation context.
3. Inspect existing test and POM conventions.
4. Use JavaScript and @playwright/test.
5. Reuse existing fixtures and utilities.
6. Preserve approved test IDs and expected results.
7. Use reliable locators and web-first assertions.
8. Avoid fixed sleeps and hardcoded credentials.
9. Run test discovery and the relevant tests when permitted.
10. Report changes and real execution results.
```

**Do not confuse a skill with context** - Context: facts, constraints,
architecture, application details, and project-specific knowledge. -
Skill: a procedure for performing a type of task. - Instructions:
standing rules. - Prompt: a reusable request. - Agent: a configured role
that coordinates work. - Test plan: an artifact documenting the approved
test scope.

### 6.4 Recommended division of responsibility

  -------------------------------------------------------------------------------------------
  Item                                                    Best content
  ------------------------------------------------------- -----------------------------------
  `.github/copilot-instructions.md`                       General project rules

  `context/context.md`                                    Project overview, application and
                                                          framework context

  `context/03_test_generation.md`                         Detailed project-specific
                                                          generation requirements

  `.github/skills/javascript-playwright-tests/SKILL.md`   Repeatable test-generation
                                                          procedure

  Prompt file                                             A reusable request to generate
                                                          tests

  Agent profile                                           Role, workflow, tools, and
                                                          guardrails

  `artifacts/test-plan.md`                                Approved test scope and expected
                                                          outcomes
  -------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 7. Models, plugins, and tools

### 7.1 `/models` --- select the model

**What it does:** Opens the model picker.

The model is the AI engine used for the current chat or supported agent
workflow. Available models can differ by account, plan, organization
policy, and product.

**When to use it** - Compare the quality of code generation. - Choose a
model that is suitable for a complex reasoning task. - Try a faster
model for straightforward documentation or small edits. - Diagnose
whether a model change affects the result.

**Practical selection approach** - Complex framework design or failure
diagnosis: use a strong reasoning/coding model available to you. -
Routine documentation and small refactoring: a faster model may be
sufficient. - If a model repeatedly ignores constraints, shorten the
request, explicitly name the relevant files, and ask it to report what
it read.

**Remember:** A model change does not create a new skill or agent and
does not automatically grant additional tool permissions.

### 7.2 `/plugins` --- manage integrations

**What it does:** Opens the plugin-management UI shown in the
screenshot.

Plugins or integrations connect Copilot to additional capabilities,
depending on the product and configuration. They are different from
prompts and skills.

**When it is useful** - You need a supported external service or
integration. - You want to inspect which integrations are available. -
An agent needs a capability that is not built into the current
environment.

**Before enabling a plugin** 1. Confirm that it is the right
integration. 2. Review the requested permissions. 3. Understand which
account or data it can access. 4. Test with non-sensitive data first. 5.
Disable it if it is not needed.

**Important:** The `/plugins` menu is product-specific. Do not assume
that plugins in one Copilot interface are identical to extensions, MCP
servers, or plugins in another interface.

### 7.3 `/tools` --- inspect available tools

**What it does:** Opens the tool configuration UI.

Tools are capabilities an agent can call to perform work beyond
producing text. Depending on the product, these can include file access,
terminal execution, browser interaction, search, or external
integrations.

**When it is useful** - An agent cannot access a file or perform an
action. - You want to inspect which capabilities are available. - You
need to restrict an agent to a smaller set of tools. - You are
investigating why an agent described a task but did not execute it.

**For your Playwright workflow, useful capabilities may include** -
Workspace/file reading and editing. - Terminal execution for
`npx playwright test`. - Browser or Playwright interaction, where
supported. - Test output, trace, screenshot, and report inspection.

Availability depends on your installed product, current permissions, and
agent configuration.

**Tool safety** - Grant only the permissions needed for the task. -
Review file modifications and terminal commands. - Be cautious with
external systems, secrets, destructive commands, and broad write
access. - Do not assume the agent has a browser tool merely because
Playwright is installed in the repository.

------------------------------------------------------------------------

## 8. How the pieces work together

A useful mental model:

``` text
Instructions
    ↓
Agent profile
    ↓
Relevant context + relevant skill
    ↓
Reusable prompt or current user request
    ↓
Available tools perform the work
    ↓
Validation and truthful report
```

The model reasons over the information it can access. The agent profile
defines the role and workflow. Instructions define persistent rules.
Context files provide project knowledge. Skills provide task procedures.
Prompts make common requests repeatable. Tools carry out permitted
actions. Hooks can automate supported lifecycle events.

These pieces are complementary, not interchangeable.

### Example: generating an OrangeHRM login test

1.  Choose the `orangehrm-playwright-agent`.
2.  Ask it to analyze the real manual workbook.
3.  It reads the repository instructions and the relevant analysis
    context and skill.
4.  It creates `artifacts/test-plan.md`.
5.  You review and approve the plan.
6.  You request test generation using the appropriate prompt or skill.
7.  The agent inspects the existing framework and creates or updates the
    POM and test files.
8.  It runs `npx playwright test --list` and the relevant test when
    permitted.
9.  If the test fails, it uses the failure output, screenshot, or trace
    to investigate.
10. It records the actual changes and results.

------------------------------------------------------------------------

## 9. Recommended workflow for a Playwright OrangeHRM project

### Step 1: Inspect before changing files

Ask:

``` text
Inspect the repository, package.json, playwright.config.js, existing
tests, POMs, fixtures, utilities, context files, and skills.
Report what exists and what is missing. Do not edit anything yet.
```

### Step 2: Analyze manual tests

Ask:

``` text
Read the project instructions, context/context.md,
context/01_testcase_analysis.md, and the Excel analysis skill if present.
Find and inspect the actual manual workbook.

Create artifacts/test-plan.md from the real workbook.
Preserve test IDs, steps, and expected results.
Do not generate tests yet.
```

### Step 3: Review the test plan

Confirm that the plan matches the workbook. Resolve ambiguities before
generation.

### Step 4: Generate POMs and tests

Ask:

``` text
Read context/02_pom_generation.md and context/03_test_generation.md,
plus the relevant POM and JavaScript Playwright skills.

Use the approved test plan and existing framework conventions.
Generate JavaScript Playwright tests and POMs. Do not duplicate existing
utilities. Validate the files and report what changed.
```

### Step 5: Execute the tests

In PowerShell, if the project uses the corresponding
environment-variable names:

``` powershell
$env:ORANGEHRM_USERNAME = "Admin"
$env:ORANGEHRM_PASSWORD = "admin123"

npx playwright test --list
npx playwright test
npx playwright show-report
```

Use test credentials only where authorized, and follow your
organization's secret-handling requirements. The example credentials
above are commonly used for the public OrangeHRM demo, but their
validity and availability are not guaranteed.

### Step 6: Heal failures using evidence

Ask:

``` text
Read context/05_test_healing.md and the test-healing skill if present.
Inspect the actual test output and relevant screenshot or trace.
Classify the root cause, make the smallest justified fix, rerun the
focused test and relevant regression tests, and write
artifacts/healing-report.md. Do not weaken assertions or hide failures.
```

### Step 7: Review the result

Require the agent to report: - Files read and changed. - Commands
actually executed. - Passed, failed, skipped, and not-run tests. - Root
cause and evidence for each fix. - Remaining issues and next steps.

------------------------------------------------------------------------

## 10. Troubleshooting and safe-use checklist

### Copilot ignores my instructions

-   Confirm that the correct repository/workspace is open.
-   Check the instructions configuration.
-   Mention the exact file paths in your request.
-   Ask the agent to list which instruction, context, and skill files it
    read.

### A skill is not being used

-   Confirm the file exists at the location expected by the current
    Copilot product.
-   Check the skill's metadata and description.
-   Invoke or reference the skill explicitly where supported.
-   Do not assume that all skills load into every request.

### The agent creates tests without analyzing the workbook

-   Stop generation.
-   Require the actual workbook to be located and inspected.
-   Review `artifacts/test-plan.md` before allowing code generation.

### The agent claims tests passed

-   Ask for the exact test command and real output.
-   Verify the result in the terminal or report.
-   Treat an unexecuted test as **not run**, not passed.

### The agent edits too much

-   Ask for a plan before editing.
-   Request the smallest change that addresses the issue.
-   Review the diff before accepting changes.
-   Use version control so you can inspect or revert changes.

### A hook or tool does not work

-   Check the current product's documentation and available
    configuration.
-   Verify that the tool is enabled and authorized.
-   Try a small, non-destructive test.
-   Do not copy configuration formats from another Copilot product
    without confirming compatibility.

------------------------------------------------------------------------

## 11. Official documentation

Use the documentation for the exact Copilot interface and version you
are using. Features and command names can change.

-   [GitHub Copilot CLI
    documentation](https://docs.github.com/en/copilot/how-tos/copilot-cli)
-   [Custom agents for Copilot
    CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli)
-   [Add skills to Copilot
    CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills)
-   [Customize GitHub
    Copilot](https://docs.github.com/en/copilot/how-tos)
-   [Playwright Test Agents](https://playwright.dev/docs/test-agents)
-   [Playwright CLI](https://playwright.dev/docs/getting-started-cli)

------------------------------------------------------------------------

## Final recommendation

For your OrangeHRM project, use the menu this way:

1.  `/agents` --- create and manage your main automation agent.
2.  `/instructions` --- define project-wide rules.
3.  `/skills` --- configure reusable analysis, POM, generation,
    integration, and healing procedures.
4.  `/prompts` --- save repeatable requests for each stage.
5.  `/tools` --- verify which actions the agent can actually perform.
6.  `/hooks` --- automate safe, supported validation steps where
    appropriate.
7.  `/models` --- choose a suitable model for the task.
8.  `/debug` --- investigate unexpected chat or agent behavior.
9.  `/fork` --- explore alternatives without losing the original
    discussion.
10. `/rename` --- keep conversations organized.
11. `/clear` --- start a clean conversation.
12. `/plugins` --- manage supported external integrations.

**Start with instructions, one coordinator agent, and a small number of
focused skills.** Add prompts and hooks when you find yourself repeating
tasks. This keeps the setup understandable and easier to troubleshoot.
