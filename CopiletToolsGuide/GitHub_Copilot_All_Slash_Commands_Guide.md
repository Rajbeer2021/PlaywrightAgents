# GitHub Copilot Chat Slash Commands --- Complete Practical Guide

**Purpose:** A practical reference for the slash commands and
customization actions that can appear when you type `/` in GitHub
Copilot Chat in VS Code, plus common coding commands and commands shown
in Copilot CLI / the GitHub Copilot app.

> **Important:** There is no single permanent list of slash commands for
> every Copilot surface. The menu changes based on whether you use VS
> Code Chat, Copilot CLI, the GitHub Copilot app, your
> extension/version, active session, selected agent, organization
> policy, and installed custom prompts or skills. The live `/` menu in
> your own Copilot UI is the source of truth for what is available to
> you. This guide covers the commands visible in your screenshots, the
> VS Code customization commands, and commonly documented commands. A
> command may not work in every interface.

## Contents

1.  [How to use slash commands](#1-how-to-use-slash-commands)
2.  [The customization commands in your
    screenshot](#2-the-customization-commands-in-your-screenshot)
3.  [Create commands: `/create-prompt`, `/create-instructions`,
    `/create-skill`, `/create-agent`,
    `/create-hook`](#3-create-commands)
4.  [Common coding commands](#4-common-coding-commands)
5.  [Session and context commands](#5-session-and-context-commands)
6.  [Agent, mode, and workflow
    commands](#6-agent-mode-and-workflow-commands)
7.  [Tools, models, extensions, and
    integrations](#7-tools-models-extensions-and-integrations)
8.  [Custom prompts and skills as slash
    commands](#8-custom-prompts-and-skills-as-slash-commands)
9.  [Recommended OrangeHRM Playwright
    workflow](#9-recommended-orangehrm-playwright-workflow)
10. [Troubleshooting and safety](#10-troubleshooting-and-safety)
11. [Official references](#11-official-references)

------------------------------------------------------------------------

## 1. How to use slash commands

1.  Open Copilot Chat in VS Code.
2.  Click the chat input and type `/`.
3.  Wait for the command picker.
4.  Select a command or continue typing to filter the list.
5.  If the command opens a configuration screen, follow the UI. If it
    accepts a prompt, provide a specific task.
6.  If you cannot find a command, check whether you are in VS Code Chat,
    Copilot CLI, or the GitHub Copilot app; the command may be specific
    to another surface.

Some slash commands are **actions** (`/clear`), some open
**configuration editors** (`/agents`), some **create reusable files**
(`/create-skill`), and some are **task prompts** (`/fix`). Some entries
may come from your own repository's prompt files or skills rather than
being built into Copilot.

### A note about "all commands"

Copilot's slash-command inventory changes. A generated custom prompt or
skill may add its own command, and some commands only appear after a
session starts or when a relevant file/PR is open. Therefore, treat the
sections below as a broad guide---not a promise that every item appears
in every installation. Type `/` in your own chat to see the commands
available in the current context.

------------------------------------------------------------------------

## 2. The customization commands in your screenshot

These open configuration or management experiences. They generally do
not perform the entire automation task themselves.

  -------------------------------------------------------------------------------
  Command                 What it does            When / why to use it
  ----------------------- ----------------------- -------------------------------
  `/agents`               Configure custom agents Create or edit a specialized
                                                  agent such as
                                                  `orangehrm-playwright-agent`.

  `/debug`                Open Chat Debug View /  Investigate unexpected chat
                          debug information,      behavior, context, or tool
                          depending on product    problems. The exact debug
                                                  output differs by product.

  `/hooks`                Configure hooks         Set up supported lifecycle
                                                  actions or policy checks.
                                                  Verify supported events before
                                                  relying on a hook.

  `/instructions`         Configure instruction   Define stable repository or
                          files                   user rules, such as
                                                  JavaScript-only, POM
                                                  conventions, and credential
                                                  handling.

  `/models` or `/model`   Open the model picker   Select an available model
                          or choose a model       suited to code generation,
                                                  analysis, or faster routine
                                                  tasks.

  `/plugins`              Manage plugins in the   Manage supported integrations
                          UI shown                in that particular Copilot
                                                  experience. Do not assume this
                                                  is the same as VS Code
                                                  extensions or MCP servers.

  `/prompts`              Configure prompt files  Create and manage reusable task
                                                  prompts.

  `/skills`               Configure/manage skills Create or manage reusable
                                                  procedures; in some CLI
                                                  environments `/skills reload`
                                                  reloads skills.

  `/tools`                Configure or inspect    Check which capabilities an
                          tools                   agent can use and adjust
                                                  available tools where
                                                  supported.

  `/clear`                Start a new chat and    Begin a clean task without
                          archive/clear the       carrying the full current
                          current one, depending  transcript forward. Check the
                          on product              UI wording: some products
                                                  clear; others archive.

  `/fork`                 Fork the current        Explore an alternative approach
                          conversation/session    while preserving the original
                                                  conversation.

  `/rename`               Rename the current      Keep project conversations
                          chat/session            organized and easy to find.
  -------------------------------------------------------------------------------

### When should you use these?

-   Use `/instructions` first to set long-lived project rules.
-   Use `/agents` to create a coordinator or specialist.
-   Use `/skills` to manage reusable task procedures.
-   Use `/prompts` for reusable task requests.
-   Use `/tools` to inspect what the agent can actually do.
-   Use `/hooks` only when you understand when the hook runs and what it
    can execute.
-   Use `/models` when you want to change the model.
-   Use `/debug` when something behaves unexpectedly.
-   Use `/fork` to compare approaches, `/rename` to organize, and
    `/clear` to start a clean session.

------------------------------------------------------------------------

## 3. Create commands

The second screenshot shows these commands:

### 3.1 `/create-prompt`

**Purpose:** Create a reusable `.prompt.md` file for a common task.

**Use it for:** Repeating a test-generation request, code review,
documentation generation, failure diagnosis, or framework integration.

**Example request:**

``` text
/create-prompt Create a reusable prompt named orangehrm-generate-tests.
It must read the approved artifacts/test-plan.md, project instructions,
and relevant context and skills; inspect existing POMs and utilities;
generate JavaScript @playwright/test tests; preserve actual test IDs;
run validation when permitted; and report real results.
```

A prompt file is a reusable **request template**. It is not the agent
role or the detailed procedural skill itself.

### 3.2 `/create-instructions`

**Purpose:** Create an instruction file, commonly `.instructions.md` in
VS Code's agent customization system, for project-wide or file-specific
guidance.

**Use it for:** Rules Copilot should follow repeatedly, such as
language, framework, architecture, security, style, and validation
requirements.

**Example request:**

``` text
/create-instructions Create repository instructions for my OrangeHRM
automation project. Use JavaScript and @playwright/test, preserve the
existing POM conventions, read relevant context and skills, avoid fixed
waits and hardcoded credentials, and never claim a test passed unless it
was actually executed successfully.
```

**Note:** Repository-wide Copilot instructions are also commonly stored
in `.github/copilot-instructions.md`. VS Code's generated
`.instructions.md` files can be scoped with metadata such as file
patterns. Follow the location and format offered by your installed UI.

### 3.3 `/create-skill`

**Purpose:** Create a reusable `SKILL.md` that packages a workflow or
specialized knowledge.

**Use it for:** Detailed steps for Excel test analysis, POM design,
JavaScript test generation, framework integration, or failure healing.

**Example request:**

``` text
/create-skill Create a skill named orangehrm-test-healing.
It must inspect real test output, trace, screenshot, and locator evidence;
identify the root cause; make minimal justified changes; never weaken
assertions or silently skip failures; rerun the focused test and relevant
regression tests; and write artifacts/healing-report.md.
```

A skill is a **reusable procedure**. In supported environments it may be
invoked by typing `/skill-name`, or selected automatically when
relevant. Discovery rules vary by product.

### 3.4 `/create-agent`

**Purpose:** Create a custom agent profile, commonly an `.agent.md`
file.

**Use it for:** Defining a task-specific role, responsibilities,
available tools, and safety rules.

**Example request:**

``` text
/create-agent Create orangehrm-playwright-agent, a coordinator for my
OrangeHRM QA workflow. It must inspect project instructions, discover
relevant context and skills, analyze the actual manual workbook, create
a reviewable test plan, generate JavaScript Playwright POMs and tests,
run tests when permitted, and heal failures using real evidence. It must
not invent workbook data or claim unexecuted tests passed.
```

An agent is the **configured role/coordinator**. It can use
instructions, context, skills, prompts, and tools, but should not be
assumed to automatically read every file.

### 3.5 `/create-hook`

**Purpose:** Create a hook configuration file, commonly JSON in the UI
shown.

**Use it for:** Supported lifecycle automation or policy checks, such as
a permitted validation step.

**Example request:**

``` text
/create-hook Propose a safe hook for my Playwright project that validates
generated JavaScript files at a supported lifecycle point. Explain the
trigger, command, permissions, risks, and how to test it safely before
enabling it. Do not create a destructive or auto-commit hook.
```

**Safety:** Hook support and lifecycle events depend on the host.
Inspect the generated configuration and verify the documented trigger
before enabling it. Never store credentials in hook files or
automatically execute destructive commands without safeguards.

### 3.6 How to choose the right create command

  Need                                                 Choose
  ---------------------------------------------------- ------------------------
  "Always follow these project rules."                 `/create-instructions`
  "Follow this repeatable step-by-step procedure."     `/create-skill`
  "Create a role that coordinates a class of tasks."   `/create-agent`
  "Save this request so I can run it again."           `/create-prompt`
  "Run a supported action at a lifecycle event."       `/create-hook`

------------------------------------------------------------------------

## 4. Common coding commands

These commands are common in VS Code's Copilot Chat, but their
availability can depend on the current editor, selection, language,
extension version, and chat mode.

  -------------------------------------------------------------------------------------------------------------------------------------------------
  Command                 Typical purpose         Example
  ----------------------- ----------------------- -------------------------------------------------------------------------------------------------
  `/explain`              Explain selected code   `/explain Explain how this LoginPage.js works and how it fits into the POM.`
                          or a file               

  `/fix`                  Suggest or implement a  `/fix Diagnose this failing login test. Inspect the evidence and preserve the assertions.`
                          fix for an issue        

  `/tests`                Generate tests for      `/tests Generate JavaScript unit tests for this utility using the existing test framework.`
                          selected code           

  `/doc`                  Generate documentation  `/doc Document this helper function and its parameters.`
                          comments for a symbol   

  `/setupTests`           Experimental test setup `/setupTests Help me configure @playwright/test in this project.`
                          assistance in some VS   
                          Code versions           

  `/new`                  Scaffold a new          `/new Create a JavaScript Playwright page object for the login page.`
                          file/project artifact   
                          in some versions        

  `/newNotebook`          Create a notebook in    Use when working on a notebook-based task, not normal Playwright test files.
                          supported contexts      

  `/simplify`             Simplify selected code  `/simplify Simplify this helper without changing behavior or public interfaces.`
                          in supported versions   

  `/plan`                 Switch to Plan mode or  `/plan Analyze the workbook and propose a test plan. Do not edit code.`
                          create an               
                          implementation plan,    
                          depending on host       

  `/review`               Review changes in       `/review Review my changes for locator reliability, assertions, security, and maintainability.`
                          supported session       
                          contexts                

  `/security-review`      Perform a               `/security-review Check the current diff for secrets and unsafe behavior.`
                          security-focused review 
                          when supported          

  `/init`                 Generate or improve     `/init Inspect the repository and propose project-wide Copilot instructions.`
                          repository instructions 
  -------------------------------------------------------------------------------------------------------------------------------------------------

**How to use them well:** Add a specific target, language, expected
outcome, and constraints. For example, instead of "`/fix` this", say
"`/fix` the locator failure in `tests/login.spec.js`; inspect the trace
and DOM first, preserve the intended assertion, and rerun the focused
test."

------------------------------------------------------------------------

## 5. Session and context commands

Some of these are more common in the GitHub Copilot app or Copilot CLI
than in the standard VS Code Chat menu.

  ------------------------------------------------------------------------
  Command                  Typical purpose         When to use it
  ------------------------ ----------------------- -----------------------
  `/clear` or `/reset`     Start a fresh session / Switch tasks or reduce
                           clear the transcript    irrelevant conversation
                                                   history.

  `/compact`               Summarize earlier       Long sessions where
                           context to reduce       earlier details still
                           context pressure        matter.

  `/context`               Show context usage      Understand what context
                           details in supported    is consuming the
                           clients                 session.

  `/restart-session`       Restart a session while Recover from a session
                           keeping its history in  issue without
                           supported CLI versions  intentionally losing
                                                   the history.

  `/rename`                Rename current session  Organize work by
                                                   project or phase.

  `/fork`                  Create a branch of the  Try an alternative
                           conversation            solution without losing
                                                   the original.

  `/debug`                 Show debug view or copy Investigate unexpected
                           session debug details,  behavior.
                           depending on host       

  `/collect-debug-logs`    Collect or upload debug Troubleshoot a
                           logs in supported       reproducible issue;
                           clients                 inspect logs for
                                                   sensitive content
                                                   before sharing.

  `/export-gist`           Export session          Share a conversation
                           transcript to a secret  for troubleshooting,
                           gist in supported       only after checking for
                           clients                 confidential content.

  `/usage`                 Show usage/rate-limit   Understand usage
                           details in supported    limits.
                           clients                 

  `/chronicle`             Session history and     Search prior work,
                           analysis features in    summarize recent work,
                           supported clients       or obtain workflow
                                                   tips.

  `/chronicle search`      Search session history  Find an earlier
                                                   decision or command.

  `/chronicle standup`     Summarize recent work   Create a brief work
                                                   summary.

  `/chronicle cost-tips`   Suggest ways to reduce  Optimize long-running
                           usage/cost              sessions.

  `/chronicle improve`     Suggest improvements to Refine project
                           instructions            guidance.

  `/chronicle reindex`     Rebuild session-history Troubleshoot missing
                           index                   search results, where
                                                   supported.

  `/chronicle tips`        Workflow tips           Learn productivity
                                                   suggestions.
  ------------------------------------------------------------------------

Use these only if your current UI offers them. Their presence in a
different Copilot product does not guarantee support in VS Code Chat.

------------------------------------------------------------------------

## 6. Agent, mode, and workflow commands

These are documented in some Copilot app and CLI versions. They are
included here because users may see them in other `/` menus.

  ------------------------------------------------------------------------
  Command                  Typical purpose         When to use it
  ------------------------ ----------------------- -----------------------
  `/agent`                 Select an available     Choose your OrangeHRM
                           custom agent            coordinator or another
                                                   specialist.

  `/interactive`           Switch to interactive   Prefer a workflow that
                           execution mode in       asks for input/approval
                           supported clients       as needed.

  `/autopilot`             Start an autonomous     Use only when the task
                           workflow in supported   and permissions are
                           clients                 clear and you are
                                                   comfortable with the
                                                   autonomy level.

  `/orchestrate`           Coordinate work across  Split a larger project
                           sessions or             into coordinated
                           repositories in         workstreams.
                           supported clients       

  `/fleet`                 Launch multiple agents  Parallelize independent
                           in parallel in          tasks; review conflicts
                           supported clients       and outputs carefully.

  `/spawn`                 Create a focused child  Delegate a bounded
                           session in supported    subtask.
                           clients                 

  `/merge-to-parent`       Merge work from a       Bring approved fork
                           forked session in       work back to its parent
                           supported clients       session.

  `/plan`                  Plan mode or planning   Understand the approach
                           workflow                before making changes.

  `/research`              Run a research workflow Research a topic before
                           and produce a cited     choosing an
                           report in supported     implementation.
                           clients                 

  `/rubber-duck`           Ask a different model   Get a second opinion on
                           to critique an approach design or debugging.
                           in supported clients    

  `/spar`                  Challenge an approach   Find weaknesses in a
                           through adversarial     plan before
                           reasoning in supported  implementation.
                           clients                 

  `/terminal`              Open a terminal or run  Run commands such as
                           a command in supported  test discovery; inspect
                           clients                 commands before
                                                   execution.

  `/sandbox on` or         Toggle local sandboxing Adjust the supported
  `/sandbox off`           where supported         local execution
                                                   isolation setting.

  `/sandbox-policy`        Inspect effective       Understand the current
                           sandbox policy in       sandbox policy.
                           supported clients       

  `/allow-all-tools` or    Enable tool             Generally avoid unless
  `/yolo`                  auto-approval in        you fully understand
                           supported clients       the risks and trust the
                                                   task.

  `/reset-allowed-tools`   Clear session-level     Return to a more
                           tool approvals in       conservative approval
                           supported clients       workflow.

  `/remote`                Manage remote control   Continue a session from
                           in supported clients    another supported
                                                   device/interface.

  `/af`                    Find installable MCP    Discover a capability
                           servers, tools, skills, you need.
                           and agents in supported 
                           clients                 

  `/inbox`                 Open an interactive     Review work items if
                           inbox in supported      your setup uses this
                           clients                 feature.
  ------------------------------------------------------------------------

### Pull-request commands (only in suitable GitHub contexts)

  ------------------------------------------------------------------------
  Command                  Typical purpose         Requirement
  ------------------------ ----------------------- -----------------------
  `/pr-open`               Open a pull request     Active session with
                           from session changes    changes.

  `/pr-fix-checks`         Address failing checks  Open PR with failing
                           on a pull request       checks.

  `/pr-resolve-comments`   Resolve review comments Open PR with unresolved
                                                   comments.

  `/pr-merge`              Merge a pull request    PR must meet the
                                                   product's merge
                                                   requirements.
  ------------------------------------------------------------------------

**Caution:** Commands such as `/yolo`, `/allow-all-tools`, auto-pilot
execution, remote control, and PR merging can increase the impact of
mistakes. Prefer explicit approval for broad edits, shell commands,
commits, pushes, merges, and destructive operations.

------------------------------------------------------------------------

## 7. Tools, models, extensions, and integrations

### `/tools`

Opens the tool configuration UI in the screenshot. Tools are
capabilities the agent can invoke, such as file operations or terminal
actions, depending on your host and permissions.

For Playwright, verify that the environment has the capabilities you
need: - Read and edit project files. - Run `npx playwright test`. -
Inspect terminal output. - Inspect screenshots, traces, and reports. -
Use browser interaction only if an appropriate browser/Playwright tool
is actually available.

Installing Playwright does not automatically mean Copilot has a
browser-control tool.

### `/models` or `/model`

Opens the model picker or selects an available model. Use a capable
coding/reasoning model for framework design and failure analysis; use a
faster model for routine summaries if it meets your needs. Model
availability depends on your account and organization.

### `/plugins`

Manage plugins in the specific Copilot interface shown. This label is
not necessarily interchangeable with VS Code extensions, MCP servers, or
GitHub Copilot CLI integrations. Check the UI and permission details
before enabling integrations.

### Extensions, MCP servers, and tools are not the same thing

-   **VS Code extension:** Adds editor functionality.
-   **MCP server/integration:** Exposes external data or actions through
    a supported protocol.
-   **Copilot tool:** A capability available to an agent at runtime.
-   **Plugin UI:** Product-specific integration management.
-   **Skill:** Procedural instructions for using capabilities.
-   **Agent:** Role, instructions, and potentially tool selection.

------------------------------------------------------------------------

## 8. Custom prompts and skills as slash commands

After you create a prompt or skill, it may appear in the slash-command
picker under its name. For example, if the repository contains a
recognized skill named `javascript-playwright-tests`, you may be able to
invoke:

``` text
/javascript-playwright-tests
```

A prompt file can similarly appear under its prompt name. Exact names
and invocation rules depend on the filename, metadata, and Copilot
surface.

### Suggested OrangeHRM customization set

``` text
.github/
├── copilot-instructions.md
├── agents/
│   └── orangehrm-playwright-agent.agent.md
├── prompts/
│   ├── orangehrm-analyze-workbook.prompt.md
│   ├── orangehrm-generate-tests.prompt.md
│   └── orangehrm-heal-failures.prompt.md
└── skills/
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

This is a suggested layout, not a guarantee that every Copilot product
uses the same discovery locations. Use `/create-*` to let your installed
VS Code experience generate the initial file, then inspect its location
and metadata.

### The responsibilities are different

  -----------------------------------------------------------------------
  Component                           Question it answers
  ----------------------------------- -----------------------------------
  Instructions                        What rules should Copilot generally
                                      follow?

  Agent                               What role should coordinate this
                                      task, and which tools may it use?

  Context file                        What project facts, constraints,
                                      and architecture matter?

  Skill                               What repeatable procedure should be
                                      followed?

  Prompt file                         What task request do I want to run
                                      again?

  Hook                                What supported action should happen
                                      at a specific lifecycle event?

  Tool                                What operation can the agent
                                      actually perform?

  Test plan                           What tests have been approved?
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 9. Recommended OrangeHRM Playwright workflow

### Stage 1 --- inspect the repository

Use your custom agent or a prompt:

``` text
Inspect the repository structure, package.json, playwright.config.js,
existing POMs, tests, fixtures, utilities, context files, and skills.
Report which files exist and which are missing. Do not edit files yet.
```

### Stage 2 --- analyze the manual workbook

``` text
Read .github/copilot-instructions.md, context/context.md,
context/01_testcase_analysis.md, and the relevant Excel analysis skill.

Locate and inspect the actual manual workbook. Create
artifacts/test-plan.md from the workbook. Preserve its real test IDs,
steps, and expected results. Do not generate test code yet.
```

### Stage 3 --- review and approve the plan

Check the test IDs, expected results, preconditions, and scope. Resolve
ambiguous workbook rows before generation.

### Stage 4 --- generate POMs and tests

``` text
Use the approved artifacts/test-plan.md. Read the POM and test-generation
context files and the relevant skills. Inspect the existing framework,
then generate JavaScript @playwright/test files consistent with its
conventions. Reuse utilities and fixtures. Do not hardcode credentials.
Validate test discovery and run relevant tests when permitted.
```

### Stage 5 --- execute and report

Typical PowerShell commands:

``` powershell
$env:ORANGEHRM_USERNAME = "Admin"
$env:ORANGEHRM_PASSWORD = "admin123"

npx playwright test --list
npx playwright test
npx playwright show-report
```

These example credentials are commonly used for the public OrangeHRM
demo; their continued validity is not guaranteed. Use credentials only
where authorized.

### Stage 6 --- heal a failure

``` text
Read context/05_test_healing.md and the relevant healing skill.
Inspect the actual error, trace, screenshot, and locator evidence.
Identify the root cause and make the smallest justified fix.
Do not weaken assertions, silently skip tests, or claim success without
execution. Rerun the focused test and relevant regression tests.
Write artifacts/healing-report.md with actual results.
```

### Stage 7 --- inspect changes

Use `/review` if supported, or ask Copilot to review the diff. Verify
the actual test output yourself. Record tests as **passed**, **failed**,
**skipped**, or **not run** accurately.

------------------------------------------------------------------------

## 10. Troubleshooting and safety

### A command does not appear

-   Confirm which Copilot surface you are using.
-   Type `/` again and search by the full command name.
-   Check the extension/CLI version and whether the command requires an
    active session, open file, repository, or pull request.
-   Some commands are added by custom prompts or skills and will not
    exist until the files are discovered.

### A custom prompt or skill does not appear

-   Check its filename and metadata.
-   Confirm it is stored in a recognized location for the current
    product.
-   Reload skills if your environment supports `/skills reload`.
-   Restart the chat/session if needed.
-   Explicitly reference the file in a request while diagnosing
    discovery.

### Copilot ignores context

-   Ask it to list the files it actually read.
-   Reference the exact file paths in the prompt.
-   Keep stable rules in instructions, project facts in context files,
    and procedural steps in skills.
-   Do not assume all context files are automatically loaded.

### Copilot says it ran tests but did not

-   Request the exact command and output.
-   Verify in the terminal or CI report.
-   A generated test file is not evidence that a test passed.
-   Treat a test that was not executed as **not run**.

### Safety checklist

-   Review generated instructions, skills, agents, and hooks before
    enabling them.
-   Do not store passwords, API keys, or tokens in Markdown, prompt
    files, skills, or hooks.
-   Avoid broad auto-approval for shell commands and file changes.
-   Review diffs before accepting large edits.
-   Require confirmation for destructive actions, commits, pushes,
    merges, and broad refactoring.
-   Do not weaken tests merely to obtain a green result.

------------------------------------------------------------------------

## 11. Official references

The official documentation is the authority for exact command
availability and current syntax. The lists differ by product and can
change.

-   [VS Code AI features cheat
    sheet](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet)
-   [VS Code prompt
    files](https://code.visualstudio.com/docs/agents/agent-customization/prompt-files)
-   [GitHub Copilot Chat slash
    commands](https://docs.github.com/en/copilot/reference/chat-cheat-sheet)
-   [GitHub Copilot app slash
    commands](https://docs.github.com/en/copilot/reference/github-copilot-app-reference/slash-commands)
-   [Copilot CLI command
    reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)
-   [Custom agents for Copilot
    CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli)
-   [Skills for Copilot
    CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills)
-   [Playwright Test Agents](https://playwright.dev/docs/test-agents)

------------------------------------------------------------------------

## Final recommendation

For your OrangeHRM project, start with:

1.  `/create-instructions` --- create project rules.
2.  `/create-agent` --- create `orangehrm-playwright-agent`.
3.  `/create-skill` --- create the workbook-analysis, POM, generation,
    integration, and healing procedures.
4.  `/create-prompt` --- save repeatable analysis, generation, and
    healing requests.
5.  `/tools` --- confirm that the agent can inspect files and run tests.
6.  `/hooks` --- add only well-understood, safe checks after the main
    workflow works.

Then use the coding, session, and workflow commands as needed. **Always
type `/` in your own Copilot Chat to confirm which commands are
currently available in that exact interface.**
