# GitHub Copilot `#` References and Tools --- Comprehensive VS Code Guide

**What this guide covers:** The `#` picker shown in the screenshot,
including `#agent`, `#askQuestions`, `#browser`, `#browser_annotate`,
`#browser_check`, `#browser_click`, `#browser_close`,
`#browser_console_clear`, and `#browser_console_messages`, plus how to
discover and use the other `#` tools and context references available in
your own VS Code Copilot setup.

> **Important:** The `#` picker is dynamic. Entries vary with VS Code
> and Copilot versions, enabled features, installed extensions/MCP
> servers, active agent, workspace, and organization policy. The
> screenshot shows only part of the list, and duplicate names can appear
> when tools come from different providers. This guide explains the
> visible entries and the major categories, but it cannot truthfully
> guarantee a fixed, exhaustive inventory for every installation. Type
> `#` in your own Copilot Chat to see the tools and context items
> actually available in your environment.

## Contents

1.  [What does `#` mean?](#1-what-does--mean)
2.  [How to use a `#` item](#2-how-to-use-a--item)
3.  [Items visible in your
    screenshot](#3-items-visible-in-your-screenshot)
4.  [Browser tool reference](#4-browser-tool-reference)
5.  [Agent delegation and questions](#5-agent-delegation-and-questions)
6.  [Other common tool and tool-set
    references](#6-other-common-tool-and-tool-set-references)
7.  [Context references: files, symbols, selection, terminal, and
    more](#7-context-references-files-symbols-selection-terminal-and-more)
8.  [How to identify duplicate tool
    names](#8-how-to-identify-duplicate-tool-names)
9.  [Examples for OrangeHRM Playwright
    automation](#9-examples-for-orangehrm-playwright-automation)
10. [Safety and troubleshooting](#10-safety-and-troubleshooting)
11. [Official documentation](#11-official-documentation)

------------------------------------------------------------------------

## 1. What does `#` mean?

In VS Code Copilot Chat, typing `#` opens a picker of **tool references,
tool sets, and context references**.

These are related but different:

  -----------------------------------------------------------------------
  Type                    What it means           Example
  ----------------------- ----------------------- -----------------------
  Tool                    Performs an action      A browser-click tool
                                                  can click an element.

  Tool set                Groups related tools    `#browser` can expose
                                                  several browser
                                                  actions.

  Context item            Adds information to the `#selection` can
                          prompt                  include the selected
                                                  code.

  File/folder/symbol      Supplies project        `#LoginPage.js` or a
  reference               context                 code symbol.

  Delegation tool         Asks another agent to   `#agent` or a specific
                          work on a task          subagent tool, if
                                                  available.
  -----------------------------------------------------------------------

A `#` item is not necessarily a command that runs immediately when
selected. Often it inserts a reference into your message; the agent then
decides how to use it, subject to permissions and approvals. Follow the
description displayed in the picker.

### `#` is different from `/`

-   `/` usually opens **slash commands**: actions, reusable prompts, or
    configuration workflows.
-   `#` usually opens **tools and context**: capabilities the agent can
    use and information to include in the request.
-   `@` can select a chat participant or specialized agent in some
    Copilot interfaces.

For example:

``` text
/agents
```

opens agent configuration in the UI shown earlier, while:

``` text
#browser
```

references the browser tool set in the VS Code chat environment, if
available.

------------------------------------------------------------------------

## 2. How to use a `#` item

1.  Open VS Code and your project workspace.
2.  Open GitHub Copilot Chat and select Agent mode if the task requires
    tool use.
3.  Type `#` in the chat input.
4.  Search for a tool or context item by name.
5.  Select the desired item from the picker.
6.  Complete your prompt with the task, target, expected result, and
    restrictions.
7.  Review any tool calls and permission prompts. Verify the actual
    result.

### Example

``` text
#browser Open https://opensource-demo.orangehrmlive.com/web/index.php/auth/login.
Inspect the login form and report the accessible names and visible validation
messages. Do not change project files.
```

Selecting `#browser` does not by itself specify the task. The
natural-language prompt tells the agent what to do.

### Tool sets versus individual tools

If both `#browser` and `#browser_click` are available: - `#browser` may
be a **tool set** that gives the agent access to multiple browser
actions. - `#browser_click` may be an individual action, potentially
from a different provider.

The icon and description in the picker are useful clues. Open the tool
description if available, and inspect the provider or source when
duplicate entries appear.

------------------------------------------------------------------------

## 3. Items visible in your screenshot

The screenshot shows the following entries. It also shows duplicate
instances of some browser names, so see [Section
8](#8-how-to-identify-duplicate-tool-names).

### 3.1 `#agent` --- delegate tasks to other agents

**Description in screenshot:** "Delegate tasks to other agents."

**Purpose:** Delegate a bounded piece of work to a specialist or
subagent when the host provides agent delegation.

**When useful** - Ask one agent to inspect the project while the main
agent plans. - Delegate an independent code review. - Ask a subagent to
analyze a workbook or summarize failures. - Split a large task into
independent workstreams.

**Example**

``` text
#agent Ask a subagent to inspect the existing Playwright test structure and
report which POMs, fixtures, and utilities can be reused. Do not edit files.
I will use the report to plan test generation.
```

**Best practices** - Give the delegated agent a clear deliverable. -
Keep the subtask narrow. - Tell it whether it may edit files or only
analyze. - Review the subagent's results before merging or applying
changes. - Do not assume all subagents share the same conversation
history, permissions, or context. Include the key file paths and
constraints.

### 3.2 `#askQuestions` --- ask clarifying questions

**Purpose:** Ask the user structured questions to resolve ambiguity
before proceeding. The exact interaction depends on the provider.

**When useful** - A workbook has unclear expected results. - The agent
cannot tell whether to create a new POM or extend an existing one. -
Environment, browser, or test scope is missing. - A potentially
destructive change needs explicit approval.

**Example**

``` text
#askQuestions Before generating tests, ask me about any ambiguous manual
test cases, missing expected results, or unclear environment requirements.
Do not invent requirements.
```

**Best practices** - Use questions to resolve genuine blockers, not to
ask for information already in the repository. - Let the agent inspect
the project first when it can safely do so. - Ask only the smallest set
of necessary questions.

### 3.3 `#browser` --- browser tool set

**Purpose:** Gives the agent access to browser interaction capabilities
exposed by the VS Code environment.

**Typical capabilities may include** - Open or navigate to a web page. -
Read page content and accessible elements. - Click or hover elements. -
Enter text and handle dialogs. - Capture screenshots. - Inspect console
errors. - Run a scripted Playwright flow, if supported.

**When useful** - Explore a live website before writing locators. -
Verify a page flow interactively. - Inspect the rendered result after a
code change. - Reproduce a UI bug. - Check a web application at
different viewport sizes.

**Example**

``` text
#browser Open the OrangeHRM login page. Inspect the username and password
fields, the Login button, and the visible page heading. Report the
accessible roles and names. Do not log in or modify files.
```

**Important distinction:** Browser tools support interactive exploration
and verification. They do not replace committed Playwright tests for
repeatable regression testing in CI. Keep durable tests in your
repository.

### 3.4 `#browser_annotate` --- annotate or attach browser feedback

**Purpose:** The exact behavior is provider-dependent. In a browser-tool
integration, an annotation tool may add notes or feedback tied to
browser content, elements, or screenshots.

**When useful** - Provide visual feedback about a particular element. -
Explain which part of a page should change. - Attach comments to a page
region for the agent to consider.

**Example**

``` text
#browser_annotate Add feedback to the login form area: the error message
should be visible near the form and must not overlap the Login button.
Do not modify the application until you inspect the current page.
```

**Caution:** Confirm the picker description for your specific provider.
This name is not guaranteed to have identical behavior across all
browser integrations.

### 3.5 `#browser_check` --- check browser state or page conditions

**Purpose:** A browser integration may expose a check/verification
action for the current page or an element. The exact inputs and return
value are determined by the tool provider.

**When useful** - Verify that a heading or control is present. - Check
whether a page reached an expected state. - Validate a visual or
functional condition after interaction.

**Example**

``` text
#browser_check Check whether the OrangeHRM login page is loaded and whether
the Username field and Login button are visible. Report the evidence.
```

**Important:** Do not assume that a generic browser check has the same
assertion semantics as Playwright's `expect()`. For durable automated
assertions, use `@playwright/test`.

### 3.6 `#browser_click` --- click a browser element

**Purpose:** Click an element using the provider's supported browser
identification mechanism.

**When useful** - Click a navigation link or button. - Submit a form
during exploratory testing. - Reproduce a sequence of user interactions.

**Example**

``` text
#browser_click Click the Login button only after checking that the required
fields have been filled. Report what happens next.
```

**Best practices** - Identify the element by accessible role/name or
other reliable evidence. - Verify the page state after the click. -
Avoid actions that submit real transactions or change real data unless
explicitly authorized. - If two `#browser_click` entries appear, inspect
their provider and description before selecting one.

### 3.7 `#browser_close` --- close a browser page or session

**Purpose:** Close a page, tab, or browser session managed by that tool
provider. The exact scope can differ.

**When useful** - Finish an exploratory browser session. - Close a
temporary tab. - Clean up a session after a test.

**Example**

``` text
#browser_close Close only the temporary browser page opened for this
OrangeHRM inspection. Do not close my other browser sessions.
```

**Caution:** Check whether the tool closes one page or an entire
session. Avoid closing a user-owned page containing unsaved work.

### 3.8 `#browser_console_clear` --- clear browser console messages

**Purpose:** Clear console messages in the browser session managed by
the provider.

**When useful** - Start a clean console inspection before reproducing a
bug. - Separate new errors from old messages. - Reduce noise before a
focused test.

**Example**

``` text
#browser_console_clear Clear the current test page's console messages,
then reproduce the login-page issue and capture only the new messages.
```

**Caution:** Clearing the console removes diagnostic history from the
visible console. Capture or preserve existing evidence first if it may
be needed.

### 3.9 `#browser_console_messages` --- retrieve console messages

**Purpose:** Read console messages from the browser session, where
supported.

**When useful** - Diagnose JavaScript exceptions. - Inspect failed
resource loads. - Investigate client-side errors after a click or
navigation. - Compare console output before and after a fix.

**Example**

``` text
#browser_console_messages Read the console messages after loading the
OrangeHRM login page. Separate errors, warnings, and informational output.
Do not assume a console error is the root cause without supporting evidence.
```

**Best practice:** Pair console output with the page state, network
evidence where available, screenshot, and Playwright trace. A console
message alone does not prove the cause of a failed test.

------------------------------------------------------------------------

## 4. Browser tool reference

Your screenshot shows the beginning of a longer browser tool list.
Depending on the integration, you may see additional names such as the
following. The exact tool names and availability vary, so use this as a
category guide and confirm each item's picker description.

  ------------------------------------------------------------------------------
  Tool or family                 Typical purpose         Useful example
  ------------------------------ ----------------------- -----------------------
  `#browser`                     Browser tool set        Explore and verify a
                                                         user journey.

  `#browser_open` / open-page    Open a browser page     Open the application
  variants                                               URL.

  `#browser_navigate` /          Navigate to a URL       Move to a route or
  navigation variants                                    reload a page.

  `#browser_click`               Click an element        Submit a form or open a
                                                         menu.

  `#browser_type` / type         Enter text              Fill a test form.
  variants                                               

  `#browser_hover`               Hover over an element   Inspect hover menus.

  `#browser_check`               Check a condition       Verify a visible
                                                         control or page state.

  `#browser_read` / page-content Read page content       Inspect headings,
  variants                                               labels, and visible
                                                         text.

  `#browser_screenshot` /        Capture a screenshot    Save visual evidence of
  screenshot variants                                    a layout issue.

  `#browser_console_messages`    Read console messages   Investigate runtime
                                                         errors.

  `#browser_console_clear`       Clear console messages  Isolate new errors
                                                         during reproduction.

  `#browser_annotate`            Attach annotations or   Point out a visual
                                 feedback                issue.

  `#browser_close`               Close a page/session    Clean up after
                                                         exploration.

  `#browser_dialog` / dialog     Handle browser dialogs  Test an alert or
  variants                                               confirmation dialog.

  `#browser_runPlaywrightCode` / Run a scripted browser  Automate a longer
  scripted variants              flow                    sequence when
                                                         supported.
  ------------------------------------------------------------------------------

These rows describe common browser-tool capabilities, not a guarantee
that each exact name exists in your picker. The current VS Code browser
tools may use names such as `openBrowserPage`, `navigatePage`,
`readPage`, `screenshotPage`, `clickElement`, `hoverElement`,
`typeInPage`, `handleDialog`, and `runPlaywrightCode` rather than the
underscore names shown by some providers.

### A good browser investigation prompt

``` text
Use the available browser tools to open:
https://opensource-demo.orangehrmlive.com/web/index.php/auth/login

Inspect the rendered login page and report:
1. Page title and main heading.
2. Accessible roles and names of the username, password, and Login controls.
3. Any visible validation or console errors.
4. Evidence supporting each observation.

Do not enter credentials, submit forms, or modify files.
If a tool is unavailable, say so instead of pretending it ran.
```

------------------------------------------------------------------------

## 5. Agent delegation and questions

### `#agent` versus `#askQuestions`

  -----------------------------------------------------------------------
                          `#agent`                `#askQuestions`
  ----------------------- ----------------------- -----------------------
  Goal                    Delegate work           Clarify requirements

  Output                  A subagent's findings   User answers to
                          or work                 targeted questions

  Use when                A task can be split     A requirement is
                                                  missing or ambiguous

  Example                 Analyze the existing    Ask which environment
                          POMs independently      the tests should target
  -----------------------------------------------------------------------

### Other agent-related references you may encounter

VS Code's current tool reference documents references such as
`#agent/runSubagent`, `#todos`, and `#vscode/askQuestions`. Availability
depends on the installed version.

  -----------------------------------------------------------------------
  Reference                           Purpose
  ----------------------------------- -----------------------------------
  `#agent`                            Tool set for delegating to
                                      subagents, when available.

  `#agent/runSubagent`                Run a bounded task in an isolated
                                      subagent context.

  `#todos`                            Track task progress using a todo
                                      list, if provided.

  `#vscode/askQuestions`              Ask clarifying questions using an
                                      interactive UI, if provided.
  -----------------------------------------------------------------------

### Delegation example for your project

``` text
Use #agent to delegate a read-only inspection of the existing project.
Ask the subagent to identify:
- Existing Playwright config and package scripts.
- Existing POM classes and reusable utilities.
- Test files and naming conventions.
- Missing files needed for the OrangeHRM workflow.

Do not ask the subagent to edit files. Summarize its findings before planning.
```

Do not delegate two agents to edit the same files simultaneously unless
your workflow has explicit conflict management.

------------------------------------------------------------------------

## 6. Other common tool and tool-set references

The exact picker inventory changes. The following names are documented
by VS Code or may be contributed by enabled integrations. Search for
them in the picker rather than assuming they are enabled.

### 6.1 Search and web tools

  -----------------------------------------------------------------------
  Reference                           Typical purpose
  ----------------------------------- -----------------------------------
  `#search`                           Tool set for searching the
                                      workspace, if present.

  `#search/textSearch`                Search text across files.

  `#web`                              Tool set for accessing web content,
                                      if available.

  `#web/fetch`                        Fetch and read a webpage.

  `#githubRepo`                       Search a GitHub repository
                                      semantically; specify `owner/repo`.

  `#githubTextSearch`                 Search text/code patterns in a
                                      GitHub repository or organization.
  -----------------------------------------------------------------------

**Example**

``` text
#search/textSearch Find every use of `page.waitForTimeout` in this project.
Report file paths and line numbers; do not edit files.
```

**Example web research**

``` text
#web/fetch Find the current official Playwright documentation for
getByRole() and summarize its supported options.
```

Web tools may not have access to private sites or authenticated
resources. Respect your organization's data and access policies.

### 6.2 Workspace and editor tools

  -----------------------------------------------------------------------
  Reference                           Typical purpose
  ----------------------------------- -----------------------------------
  `#newWorkspace`                     Create a workspace in supported
                                      versions.

  `#vscode/extensions`                Search for extensions and
                                      information about them.

  `#vscode/installExtension`          Install an extension if permitted.

  `#vscode/runCommand`                Run a VS Code command.

  `#vscode/VSCodeAPI`                 Look up or use VS Code API
                                      capabilities where supported.

  `#vscode/getProjectSetupInfo`       Obtain project scaffolding/setup
                                      information.
  -----------------------------------------------------------------------

**Example**

``` text
#vscode/getProjectSetupInfo Explain the recommended setup for a
JavaScript Playwright Test project. Do not install anything yet.
```

Installation and command execution can change the environment. Review
the requested action and permissions before proceeding.

### 6.3 Terminal, test, and source-control tools

Some environments expose terminal, test, Git, or source-control tools
through the `#` picker, but their exact names are not universal.

Use the picker to look for capabilities related to: - Running terminal
commands. - Listing or executing tests. - Reading terminal output. -
Inspecting source-control changes. - Searching test results or
diagnostics.

For a Playwright project, you may ask the agent to run:

``` powershell
npx playwright test --list
npx playwright test
```

But only do so if terminal execution is available and authorized. A
tool's existence does not mean every command should be run without
review.

### 6.4 Tool sets

A tool set groups related actions. For example, `#browser` may provide
navigation, interaction, screenshots, and console inspection. Selecting
a tool set can be simpler than naming every action in a prompt.

Use an individual tool when you know the exact action you need; use a
tool set when you want the agent to choose among related capabilities.

------------------------------------------------------------------------

## 7. Context references: files, symbols, selection, terminal, and more

Not every `#` item is a tool. Some simply add relevant information to
your prompt.

### 7.1 `#<file>` --- attach a file

Type `#` and choose a file such as `playwright.config.js`,
`tests/login.spec.js`, or `context/context.md`.

**Use it when:** The answer must be grounded in a specific file.

Example:

``` text
Review #playwright.config.js and #tests/login.spec.js.
Explain how the test timeout and browser projects are configured.
Do not modify either file.
```

### 7.2 `#<folder>` --- add folder context

Use a folder reference when the agent needs to understand a group of
files.

Example:

``` text
Inspect #pages and #tests. Explain the current POM structure and identify
reusable classes before proposing a new login test.
```

Do not attach a huge folder when a few targeted files will provide
enough context.

### 7.3 `#<symbol>` --- reference a code symbol

A symbol can be a function, class, method, or other code element. You
may need to open the file containing the symbol first.

Example:

``` text
Explain the `login()` method in LoginPage and identify how its locators
could be made more robust without changing behavior.
```

### 7.4 `#selection` --- add selected code

Select a block in the editor and reference `#selection` if it appears.

**Use it when:** You want a focused explanation, review, refactor, or
fix for a specific block.

Example:

``` text
Review #selection for asynchronous control-flow issues. Explain the
problem and propose a minimal fix before changing the code.
```

### 7.5 Other context sources

Depending on the version and enabled features, the picker may offer
terminal output, source-control changes, diagnostics, images, browser
page context, or other references.

Use these when they directly support the task: - Terminal output for an
actual failure. - Source-control changes for code review. - A screenshot
for visual debugging. - Browser page context for DOM/accessibility
inspection. - A GitHub issue or pull request for repository-specific
work.

**Best practice:** Include the smallest relevant context set that gives
the agent enough evidence to act correctly.

------------------------------------------------------------------------

## 8. How to identify duplicate tool names

Your screenshot shows repeated entries such as `#browser_click`,
`#browser_close`, and `#browser_console_messages`.

This can happen when multiple providers expose tools with similar names.
Do not assume the entries are identical.

1.  Move the selection over each entry and read its description.
2.  If the UI shows a provider or source, compare it.
3.  Prefer the tool from the integration you intended to use.
4.  Test a low-risk operation, such as reading the current page.
5.  If the purpose remains unclear, ask Copilot to explain the available
    tool providers or consult the extension/MCP configuration.
6.  Avoid calling both versions for the same action unless you know why.

For example, one browser tool could operate on VS Code's integrated
browser while another may belong to a third-party browser/MCP extension.
Their names can be similar but their sessions and permissions may
differ.

------------------------------------------------------------------------

## 9. Examples for OrangeHRM Playwright automation

### Scenario A --- inspect the login page

``` text
#browser Open the OrangeHRM demo login page and inspect its accessible
controls. Report the username field, password field, Login button, page
heading, and any visible validation message. Do not submit the form.
```

### Scenario B --- inspect existing framework

``` text
#agent Delegate a read-only inspection of the repository. Ask the subagent
to report the current config, POMs, fixtures, tests, and utilities.
Do not edit files. Summarize the result before proposing changes.
```

### Scenario C --- ask about ambiguity

``` text
#askQuestions Ask only the essential questions needed to resolve ambiguous
manual test cases before creating the approved test plan. Do not invent
expected results.
```

### Scenario D --- diagnose console errors

``` text
#browser_console_clear Clear the console for the test browser session.
Reload the login page, then use #browser_console_messages to inspect newly
produced messages. Report errors and evidence. Do not assume that a
warning is the cause of a test failure.
```

If the tool picker does not support combining references in one request,
select the relevant tools separately or ask the agent to use the
available browser tool set.

### Scenario E --- generate repeatable tests

``` text
Read the approved artifacts/test-plan.md, context/context.md, and
context/03_test_generation.md. Read the relevant JavaScript Playwright
skill. Inspect existing POMs and utilities before editing. Generate
JavaScript @playwright/test cases consistent with the existing framework.
Run test discovery and relevant tests when permitted. Report actual
results and files changed.
```

Browser interaction is useful for exploration; repository tests are
needed for repeatable regression testing.

### Scenario F --- inspect a failing test

``` text
Use the available browser and terminal tools if permitted. Inspect the
actual failing test output, screenshot, trace, and console evidence.
Identify the root cause before editing. Make the smallest justified fix,
preserve intended assertions, rerun the focused test, and report actual
results. Do not claim success if the test was not run.
```

------------------------------------------------------------------------

## 10. Safety and troubleshooting

### The tool is not listed

-   Confirm that you are using VS Code Copilot Chat, not a different
    Copilot interface.
-   Check that the relevant feature, extension, MCP server, or tool
    provider is enabled.
-   Check organization policy and version requirements.
-   Search the current picker for related terms.
-   Do not assume a tool name from another user's screenshot will exist
    in your setup.

### The tool appears twice

-   Compare the descriptions and providers.
-   Determine which browser/session or external service each tool
    targets.
-   Test with a read-only action before making changes.

### The agent does not use the selected tool

-   State the required action explicitly in natural language.
-   Ensure the selected item is a tool rather than just a context
    reference.
-   Confirm the tool is enabled and permitted.
-   Ask the agent to report which actions it actually performed.

### The agent cannot access the page

-   Check network access and whether the site is reachable.
-   Check browser tool permissions and domain restrictions.
-   If authentication is required, use an approved browser session;
    never paste secrets into public prompts.
-   Distinguish a browser-tool failure from an application failure.

### Safe-use checklist

-   Prefer read-only inspection before write actions.
-   Review commands and file diffs.
-   Do not submit real transactions or change real user data without
    authorization.
-   Never put passwords, access tokens, or private customer data into
    prompts unnecessarily.
-   Do not claim that a browser action, command, or test succeeded
    unless the tool returned evidence.

------------------------------------------------------------------------

## 11. Official documentation

Use the current VS Code documentation for the exact list and behavior of
tools in your installed version:

-   [VS Code tools and context
    reference](https://code.visualstudio.com/docs/agents/reference/tools-reference)
-   [Add context to Copilot
    Chat](https://code.visualstudio.com/docs/chat/copilot-chat-context)
-   [Use browser tools with
    agents](https://code.visualstudio.com/docs/agents/run/browser-tools)
-   [VS Code AI features cheat
    sheet](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet)
-   [GitHub Copilot in the
    IDE](https://docs.github.com/en/copilot/how-tos/copilot-in-your-ide/chat-with-copilot/chat-in-ide)

------------------------------------------------------------------------

## Final recommendation

For your OrangeHRM workflow, the most useful `#` references are likely
to be:

1.  `#browser` --- inspect the live UI and gather locator evidence.
2.  `#agent` --- delegate independent, bounded analysis.
3.  `#askQuestions` --- clarify missing requirements.
4.  `#search` / `#search/textSearch` --- find existing tests and
    utilities.
5.  `#terminal` or the available terminal tool --- run Playwright tests,
    if exposed.
6.  `#<file>` and `#<folder>` --- provide explicit context for the
    relevant code and project rules.
7.  `#browser_console_messages` --- gather runtime evidence when
    debugging.

Use the `#` picker in your own VS Code session to discover the exact
additional tools available. The menu's descriptions and the official
tool reference are more reliable than any static list because
integrations and Copilot releases can add or rename entries.
