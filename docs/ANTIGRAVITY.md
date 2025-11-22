# Google Antigravity Documentation

## Table of Contents

- [Home](#home)
- [Get Started](#get-started)
- [Agent](#agent)
- [Models](#models)
- [Agent Modes / Settings](#agent-modes--settings)
- [Task Groups](#task-groups)
- [Browser Subagent](#browser-subagent)
- [MCP](#mcp)
- [Artifacts](#artifacts)
  - [Task List](#task-list)
  - [Implementation Plan](#implementation-plan)
  - [Walkthrough](#walkthrough)
  - [Screenshots](#screenshots)
  - [Browser Recordings](#browser-recordings)
- [Knowledge](#knowledge)
- [Editor](#editor)
  - [Tab](#tab)
  - [Command](#command)
  - [Agent Side Panel](#agent-side-panel)
  - [Review Changes (Editor)](#review-changes-editor)
- [Agent Manager](#agent-manager)
  - [Workspaces](#workspaces)
  - [Playground](#playground)
  - [Inbox](#inbox)
  - [Conversation View](#conversation-view)
  - [Browser Subagent View](#browser-subagent-view)
  - [Panes](#panes)
  - [Review Changes (Manager)](#review-changes-manager)
  - [Changes Sidebar](#changes-sidebar)
  - [Terminal](#terminal)
  - [Files](#files)
- [Browser](#browser)
  - [Chrome Extension](#chrome-extension)
  - [Allowlist / Denylist](#allowlist--denylist)
  - [Separate Chrome Profile](#separate-chrome-profile)
- [Plans](#plans)
- [Settings](#settings)
- [FAQ](#faq)

---

## Home

### Google Antigravity

Google Antigravity is an agentic development platform, evolving the IDE into the agent-first era. Antigravity enables developers to operate at a higher, task-oriented level managing agents across workspaces, while retaining a familiar AI IDE experience at its core. Antigravity extracts agents into their own surface and provides them the tools needed to autonomously operate across the editor, terminal, and browser emphasizing verification and higher-level communication via tasks and artifacts. This capability enables agents to plan and execute more complex, end-to-end software tasks, elevating all aspects of development, from building features, UI iteration, and fixing bugs to research and generating reports.

### Main Features

- **AI-powered IDE** - An AI-powered IDE with all of the AI features that developers have come to rely on such as Agent, Tab, and Command.

- **Asynchronous Agents** - Asynchronous, local agents that can work in parallel on all of your workspaces.

- **Agent Manager** - New Agent Manager view for an agent-first experience built around planning mode, the conversation UI, and artifact review.

- **Multi-window** - A multi-window product with an Editor, Manager, and Browser.

- **Browser Agent** - Agent that can actuate the browser for you and to accomplish dev tasks like dashboard reads, SCM actions, UI testing, etc. in the Browser.

### Core Surfaces

- **Editor** - A fully-functional AI-powered IDE that maps to a single workspace.

- **Browser** (In preview) - Browser-use agent capabilities to read & actuate on more surfaces beyond just the IDE.

- **Agent Manager** (In preview) - An orchestration "no code" view to start and view tasks in a minimalist product focused on the conversation and artifacts.

### Key Terms

- **Agent**: The primary AI modality within Antigravity. While the user can work tightly with an Agent within the Editor, they can also have multiple agents working across multiple codebases, orchestrated and monitored through the Agent Manager.

- **Tab & Command**: The other AI modalities within Antigravity, specifically within the text editor part of the editor surface. Tab is a more powerful "autocomplete" and Command is an inline instructive modality. From past experience, these do not get nearly as much use as the Agent.

- **Artifacts**: We define an artifact as anything that the agent creates to allow it to get its work done or communicate its accomplishments to the human user. These include rich markdown files, diff views, architecture diagrams, images, browser recordings, etc.

---

## Get Started

### Getting Started Download

Please visit **antigravity.google/download** to download Google Antigravity.

**System Requirements:**
- **macOS**: macOS versions with Apple security update support. This is typically the current and two previous versions. Min Version 12 (Monterey), X86 is not supported
- **Windows**: Windows 10 (64 bit)
- **Linux**: glibc >= 2.28, glibcxx >= 3.4.25 (e.g. Ubuntu 20. Debian 10, Fedora 36, RHEL 8)

The application will prompt when updates are available.

### Basic Navigation

The Agent Manager can be opened from the Editor via the button on the top bar or via keyboard shortcut `Cmd + E`:

Similarly, from the Agent Manager, the Editor can be opened from any workspace via the "Focus Editor" option in the workspace's drop down. When focused on a workspace, the Editor can be opened from any of the "Open Editor" buttons, or via the keyboard shortcut `Cmd + E`.

---

## Agent

### Agent

The Agent is the primary way to interact with Antigravity. It is an autonomous AI developer that can plan, execute, and verify tasks across your codebase, terminal, and browser.

### Capabilities

The Agent has access to a wide range of tools and capabilities:

- **File Editing**: Create, read, update, and delete files in your workspace.
- **Terminal Execution**: Run shell commands to build, test, and deploy your application.
- **Browser Interaction**: Control a headless or visible browser to interact with web applications, take screenshots, and verify UI changes.
- **MCP Integration**: Connect to external tools and data sources via the Model Context Protocol.
- **Artifact Generation**: Create rich artifacts like implementation plans, task lists, and walkthroughs to communicate progress and intent.

### Interaction

You interact with the Agent through a chat interface. You can give it high-level goals or specific instructions. The Agent will then break down the task, create a plan (if in Planning mode), and execute the necessary steps.

---

## Models

### Models Reasoning Model

For the core reasoning model, Antigravity offers leading frontier models from the Google Vertex Model Garden:

- Gemini 3 Pro (high)
- Gemini 3 Pro (low)
- Claude Sonnet 4.5
- Claude Sonnet 4.5 (thinking)
- GPT-OSS

Users can select which reasoning model they want to use within the model selector drop down under the conversation prompt box.

The choice of reasoning model is sticky between user messages within a conversation, so if you change the reasoning model while the Agent is running, it will continue to use the previously selected reasoning model until it has completed its steps for that user turn (or until the user cancels the current execution).

Learn more about reasoning model rate limits in our plans page.

### Additional Models

Antigravity uses a number of other models for various parts of the stack that are not customizable:

- **Nano banana**: Used by the generative image tool, when the Agent wants to produce a UI mockup or needs images to populate a web page or application.
- **Gemini 2.5 Pro UI Checkpoint**: Used by the browser subagent to actuate the browser, such as clicking, scrolling, or filling in input.
- **Gemini 2.5 Flash**: Used in the background for checkpointing and context summarization.
- **Gemini 2.5 Flash Lite**: Used by the codebase semantic search tool.

---

## Agent Modes / Settings

### Agent Modes / Settings Conversation-Level

When starting a new Agent conversation, users can choose between multiple modes:

- **Planning**: Agent can plan before executing tasks. Use for deep research, complex tasks, or collaborative work. In this mode, the Agent organizes its work in task groups, produces Artifacts, and takes other steps to thoroughly research, think through, and plan its work for optimal quality.

- **Fast**: Agent will execute tasks directly. Use for simple tasks that can be completed faster, such as renaming variables, kicking off a few bash commands, or other smaller, localized tasks. This is helpful for when speed is an important factor, and the task is simple enough that there is low worry of worse quality.

### Overall Settings

Settings across every Agent conversation can be found in the "Agent" tab of the Settings pane. Some of the major ones include:

#### Artifact Review Policy

These are the possible options for Artifact Review Policy:

- **Always Proceed**: Agent never asks for review
- **Agent Decides**: Agent will decide when to ask for review
- **Request Review**: Agent always asks for review

When Agent decides to request review from the user for implementation plans, this policy determines what the agent does. When set to "Request Review", the agent will always terminate after notifying, allowing the user to spend time reviewing the plan and adding comments to augment proposed changes.

If you do not need to manually review Agent's plan before making changes, set this to "Always Proceed", in which case every time the agent decides to request review from the user, it will then immediately continue with executing the plan.

For a hybrid setup where Agent decides when it can auto-proceed with plans, select "Agent Decides".

#### Terminal Command Auto Execution

For the terminal command generation tool:

- **Off**: Never auto-execute terminal commands (except those in a configurable Allow list)
- **Auto**: Agent decides whether to auto-execute any given terminal command
- **Turbo**: Always auto-execute terminal commands (except those in a configurable Deny list)

The allow list and deny list are configurable in the settings in the "Agent" tab. Configure these to add more advanced permissioning to your terminal auto execution policy.

Note: a change to this setting will only apply to new messages sent to Agent. In-progress responses will use the previous setting value.

For Unix shells, an allow or deny list entry matches a command if its space-separated tokens form a prefix of the command's tokens. For PowerShell, the entry tokens may match any contiguous subsequence of the command tokens.

#### Agent Non-Workspace File Access

Allow Agent to view and edit files outside of the current workspace. By default, the Agent only has access to the files in the workspace and in the application's root folder `~/.antigravity/`, which contains Artifacts, Knowledge Items, and other Antigravity-specific data.

Use with caution, as this could expose local secret or sensitive data to the Agent.

---

## Task Groups

### Task Groups

When Agent is in planning mode, large and complex tasks are handled with Task Groups, which break down these problems into smaller, more approachable units of work. Oftentimes, Agent will work on multiple parts of the greater task at the same time, and task sections are how these changes are presented to the user.

The top component of the task group specifies the overarching goal of this task as well as summarizes the changes made within this unit of work. There is also a section of edited files for quick user audit of changes: click on the file pill and you will view the current state of the changed files.

Within a task group, Agent identifies subtasks that help modularize necessary changes, and all work done by the Agent is viewable within these progress update sections. By default, the details in each subtask are not directly exposed to the user, but if you are interested, there is a toggle that will expand on the exact steps that Agent made.

Sometimes, there are pending steps, such as browser setup or terminal commands requiring approval, that are created inside these progress updates. In this case, instead of expanding all of the updates, Agent provides a special section at the end of the task group where you can review these pending steps accordingly.

---

## Browser Subagent

### Browser Subagent

When the agent wants to interact with the browser, it invokes a browser subagent to handle the task at hand. The browser subagent runs a model specialized to operate on the pages that are open within the Antigravity-managed browser, which is different from the model you selected for the main agent.

This subagent has access to a variety of tools that are necessary to control your browser, including clicking, scrolling, typing, reading console logs, and more. It can also read your open pages through DOM capture, screenshots, or markdown parsing, as well as taking videos.

While the agent is controlling a page, it will show an overlay on the page with a blue border and a small panel with short descriptions of the actions being taken. When this is shown, you will not be allowed to interact with the page to ensure it doesn't get confused by your actions.

The browser subagent can act on tabs that are not focused, so you are free to open other tabs and use them uninterrupted as it works.

---

## MCP

### Antigravity Editor: MCP Integration

Antigravity supports the Model Context Protocol (MCP), a standard that allows the editor to securely connect to your local tools, databases, and external services. This integration provides the AI with real-time context beyond just the files open in your editor.

### What is MCP?

MCP acts as a bridge between Antigravity and your broader development environment. Instead of manually pasting context (like database schemas or logs) into the editor, MCP allows Antigravity to fetch this information directly when needed.

### Core Features

#### Context Resources
The AI can read data from connected MCP servers to inform its suggestions.

- **Example**: When writing a SQL query, Antigravity can inspect your live Neon or Supabase schema to suggest correct table and column names.
- **Example**: When debugging, the editor can pull in recent build logs from Netlify or Heroku.

#### Custom Tools
MCP enables Antigravity to execute specific, safe actions defined by your connected servers.

- **Example**: "Create a Linear issue for this TODO."
- **Example**: "Search Notion or GitHub for authentication patterns."

### How to Connect

Connections are managed directly through the built-in MCP Store.

1. **Access the Store**: Open the MCP Store panel within the "..." dropdown at the top of the editor's side panel.
2. **Browse & Install**: Select any of the supported servers from the list and click Install.
3. **Authenticate**: Follow the on-screen prompts to securely link your accounts (where applicable).

Once installed, resources and tools from the server are automatically available to the editor.

### Connecting Custom MCP Servers

To connect to a custom MCP server:

1. Open the MCP store via the "..." dropdown at the top of the editor's agent panel.
2. Click on "Manage MCP Servers"
3. Click on "View raw config"
4. Modify the `mcp_config.json` with your custom MCP server configuration.

### Supported Servers

The MCP Store currently features integrations for:

- Airweave
- Atlassian
- CodeMind
- Dart
- Figma Dev Mode MCP
- GitHub
- Harness
- Heroku
- Linear
- Locofy
- MongoDB
- Neon
- Netlify
- Notion
- PayPal
- Perplexity Ask
- Pinecone
- Prisma
- Redis
- Sequential Thinking
- SonarQube
- Stripe
- Supabase

---

## Artifacts

### Artifacts

We define an Artifact as anything that the agent creates to allow it to get its work done or communicate its work and thinking to the human user. These include rich markdown files, diff views, architecture diagrams, images, browser recordings, code diffs, etc. As Agents become more autonomous and can run for longer and longer periods, Artifacts allow for the Agent to asynchronously communicate its work to the user, as opposed to requiring the user to carefully monitor every Agent step synchronously.

Artifacts are produced while the Agent is in Planning mode, and appear in both the Agent Manager and Editor views, though the former is optimized for displaying, organizing, and managing Artifacts.

Feedback is another key concept with Artifacts. Depending on the user settings, the Agent may ask for review on intermediate Artifacts to receive confirmation that it has made progress in its thinking or implementation that aligns with the user's intent and goal. The user is able to provide feedback on the Artifact to provide guidance to steer the Agent in the proper direction. The UI/UX of feedback differs from Artifact type to Artifact type.

---

## Task List

### Task List

A task list is an artifact that the agent uses to approach complex tasks and monitor progress on various action items. You can find a live snapshot of what the agent is doing in this artifact, which is constructed as a markdown list of items related to research, implementation, verification, and more. This type of artifact is generally used by the agent to keep on track with the user's overarching goal; typically, you do not need to directly interact with this artifact.

---

## Implementation Plan

### Implementation Plan

Agent utilizes the implementation plan artifact to architect changes within your codebase to accomplish a task. These plans contain technical details on what revisions are necessary and are meant to be reviewed by the user.

Unless you have you artifact review policy set to "Always Proceed", Agent will typically request your review on the implementation plan before making the changes needed to complete your task. You can click either the in-conversation or artifact header "Proceed" button to instantly continue with Agent's plan.

Oftentimes, Agent will create a plan that is slightly different from what you exactly want. Antigravity supports commenting on these artifacts so you can provide feedback to Agent for any reason, whether it be to decrease scope of changes, use a different tech stack, or correct any Agent discrepancies.

Once you have left comments on the implementation plan, you can still use the "Proceed" to continue with Agent's plan; alternatively, you can also toggle the "Review" button in the artifact header, where you can examine all comments and leave a message as feedback instead of directly proceeding, if needed.

Once you have proceeded or left a review, Agent will continue its work, either iterating on the implementation plan and re-requesting your review or beginning with its work!

---

## Walkthrough

### Walkthrough

Agent creates walkthrough artifacts when it has completed task implementation; this type of artifact includes a concise summary of the changes that have been made to remind the user of what has happened in the active conversation. This is a great way to get up to speed with the state of your codebase after Agent has made its changes in case you were not strictly following it the whole time.

For browser tasks, walkthroughs often contain screenshots and screen recordings of what Agent has built or created in the browser!

---

## Screenshots

### Screenshots

The browser subagent can take screenshots of open pages or elements on pages when it would like your review of the state of the page. This is surfaced as a tool to the agent, and you can also prompt the agent to take a screenshot of a page.

All screenshots are saved as image artifacts and can be commented on to give feedback to the agent.

---

## Browser Recordings

### Browser Recordings

Every time the browser subagent actuates on the Browser, it may choose to generate a recording of the agent's actions for your review. You can view this playback, if it is available, at the bottom of the Browser step UI.

All browser recordings are also saved as a recording artifact for your review. This view loops through the browser agent's actions.

---

## Knowledge

### Knowledge

Knowledge Items are Antigravity's persistent memory system that automatically captures and organizes important insights, patterns, and solutions from your coding sessions. They help you build upon previous work across conversations.

### What is a Knowledge Item?

A Knowledge Item is a collection of related information on a specific topic. Each Knowledge Item contains a title and summary describing what it covers, and a collection of artifacts providing information on the topic. Possible examples of artifacts include automatically generated documentation, code examples, or persistent memories of user instructions.

### How are Knowledge Items Generated?

As you interact with the agent, Antigravity automatically analyzes and extracts information from your conversation and uses that information to create new KIs or update existing KIs.

### Viewing Knowledge Items

You can view your Knowledge Items in the Antigravity Agent Manager.

### How are Knowledge Items used by the Agent?

The summaries of all your Knowledge Items are available to the agent, which uses them to inform its responses. When the agent identifies a Knowledge Item that is relevant to the conversation, it will automatically study the artifacts in that Knowledge Item and use the applicable information.

---

## Editor

### Editor

The primary entry point to Antigravity is our Editor, a surface based upon the VS Code codebase but full of rich AI-enabled features designed to improve your code-writing experience.

Much of this editor is designed to feel the same as prior experiences you may have had. You can open files up, tab through them, edit them directly, get suggestions with Tab, and work with an agent on smaller or larger tasks. When you're done, you can review your changes and interface with your preferred source control. You can also still download extensions from the Open VSX marketplace to augment your experience, through further syntax highlighting, source control integrations, or other additions.

---

## Tab

### Antigravity Editor: Tab & Navigation

This guide covers the core navigation and completion tools: Supercomplete, Tab-to-Jump, and Tab-to-Import.

### Supercomplete

Supercomplete provides code suggestions in a region near your current cursor position.

#### How it Works

- **File-Wide Suggestions**: Suggestions can modify code throughout the document, handling tasks like changing variable names or updating separate function definitions simultaneously.
- **Accepting**: Press Tab to accept the changes.

### Tab-to-Jump

Tab-to-Jump is a fluid navigation tool that suggests the next logical place in your document to move your cursor to.

#### How it Works

A "Tab to jump" icon will appear offering to move your cursor to where your next logical edit will be. Pressing Tab instantly moves your cursor to that location.

- **Accepting**: Press Tab to accept the jump.

### Tab-to-Import

Tab-to-Import handles missing dependencies without breaking your flow.

#### How it Works

- **Detection**: If you type a class or function that isn't imported, Antigravity suggests the import.
- **Action**: Press Tab to complete the word and instantly add the import statement to the top of the file.

### Settings

In your settings, you can customize the behavior of these features:

- **Enable/Disable Features**: You can individually turn off Autocomplete, Tab-to-Jump, Supercomplete, or Tab-to-Import.
- **Tab Speed**: Controls the responsiveness of suggestions.
  - **Slow**: Waits for more context before suggesting.
  - **Default**: Offers a balanced pace.
  - **Fast**: Provides rapid-fire suggestions.
- **Highlight Inserted Text**: When enabled, text inserted via Tab is highlighted to track changes easily.
- **Clipboard Context**: When enabled, Antigravity uses the contents of your clipboard to improve completion accuracy.
- **Allow Gitignored Files**: Enables Tab features (suggestions and jumping) within files listed in your .gitignore file. Tab will only ignore gitignored files if git is installed.

---

## Command

### Antigravity Editor: Command

The Command feature brings the power of natural language directly into your workflow, allowing you to request specific inline completions or terminal commands on the fly.

### How it Works

1. **Trigger**: Press Command + I (Mac) or Ctrl + I (Windows/Linux).
2. **Prompt**: A text input box will appear at your current cursor position.
3. **Instruction**: Type your request in natural language (e.g., "Create a function to sort this list" or "Add error handling to this block").
4. **Execution**: Antigravity generates the code or command directly inline for you to review and accept.

### Where to Use It

#### In the Editor

Use Command to generate boilerplate code, refactor complex functions, or write documentation without breaking your coding flow.

- **Example**: "Create a React component for a login form."

#### In the Terminal

Use Command within the integrated Antigravity terminal to generate complex shell commands without needing to memorize syntax.

- **Example**: "Find all processes listening on port 3000 and kill them."

---

## Agent Side Panel

### Agent Side Panel

The Agent Side Panel is your primary interface for interacting with the Antigravity Agent within the Editor. It's where you give instructions, see the Agent's progress, and review its work.

### Key Features:

- **Chat Interface**: A familiar chat-like interface to send messages to the Agent and receive its responses.
- **Task Progress**: View the Agent's current task, sub-tasks, and progress updates.
- **Artifacts**: Access and review artifacts generated by the Agent, such as implementation plans or walkthroughs.
- **Model Selection**: Choose the reasoning model the Agent should use.
- **Mode Selection**: Switch between Planning and Fast modes.

---

## Review Changes (Editor)

### Review Changes + Source Control

Once the agent has begun writing code within a conversation, you'll see a Review Changes section within the Agent panel's bottom toolbar. Clicking it will open up a pane within your editor where you can scroll through all of the changes you and your agent made within the conversation.

Just like with artifacts, you can comment on any of the file diffs to communicate with the agent.

---

## Agent Manager

### Agent Manager

We've built out the Agent Manager, to provide a higher level view into the work Antigravity agents are doing under your guidance. Here, you can work across multiple workspaces, oversee dozens of agents simultaneously, and interact with your codebase primarily through the agent, rather than through writing code directly. As agents and models continue to get better, we believe that this birds-eye view will be the primary entry point to all of your work. For now, as we continue to get feedback and iterate on this new surface area, the Agent Manager is under open public preview. We expect to move it to be central to the Antigravity experience soon.

At any point, you can toggle between the Agent Manager and the editor by hitting CMD+E (Mac) or CTRL+E (Windows), or through the Open Editor & Open Agent Manager buttons at the top right of the menu bar. You can also manage your editor windows through the manager, either hiding, focusing, or closing them.

---

## Workspaces

### Workspaces

In the Agent Manager, you can work across multiple workspaces simultaneously. In order to open a new workspace, just select the button in the left sidebar and select a starting folder. At any point, you can switch between conversations across workspaces through the left sidebar.

To start a new conversation within a workspace, either select the desired workspace from the Start Conversation tab or hit the Plus button next to the workspace name in the sidebar.

---

## Playground

### Playground

Playgrounds are independent workspaces that allow you to start a conversation and explore ideas instantly, without the overhead of setting up a new workspace.

### Creating a Playground

From the Start Conversation page, you can quickly send a message to a new playground by clicking the Use Playground button below the input box.

From here, you can send a message as you would normally.

### Persisting Your Work

If you want to keep the work you've done in a playground, you can move its contents into a dedicated workspace in a folder of your choosing. This action preserves your conversation history and any files created during your session, allowing you to continue exploring with multiple conversations.

Clicking on the move button in the top bar will pop up a modal for this action.

---

## Inbox

### Inbox

The inbox is your one stop shop to track all of your conversations in one place. From the inbox you can see if any of your conversations are awaiting your approval to run terminal commands, use the browser, or build out an implementation plan.

You can use the search bar and the pending switch to search for conversations by folder or by title to make sure your inbox is always focused on what is most relevant to you.

Selecting a conversation from the inbox will jump directly to the conversation, where you can continue where you left off.

---

## Conversation View

### Conversation View

The Agent Panel takes center stage in the agent manager. As the agent makes progress, you'll be able to follow along with what it's doing. To toggle off this follow-along mode, simply hit the Following button at the top right of the conversation.

---

## Browser Subagent View

### Overview

The Manager has a dedicated side panel that allows you to expand and inspect the Agent's work for a task.

In the regular manager conversation view (left half of the image), the browser subagent's work is hidden. Clicking the expand button (shown in red box) will bring up the subagent view (right half of the image). Updates to the Agent's work will be streamed into this view, so you can follow along and interact with steps as required (e.g. confirm/deny actions).

### What's in the side panel

- All subagent actions (clicking, scrolling, navigating, etc.)
- Visual feedback showing exactly where clicks happened
- Screenshots captured at each step

### Visual Inspection Feature

Tool calls that produce actions in the browser, like clicks, include a button (shown in blue box) which opens a screenshot of the browser at that exact moment and a red dot showing what interaction the agent has done in the browser.

---

## Panes

### Panes

You can open files, artifacts, knowledge items, and other content directly within the manager in panes that persist per-conversation. In order to open up a pane, simply open up the quick picker (by hitting CMD+P on Mac or CTRL+P on Windows/Linux) and select a resource. You can also hit the "+" from within a conversation's header.

These panes are resizable, splittable, and drag-and-droppable. You should configure them around as makes sense for your workflow.

If you use CMD+Click or CMD+Enter (Mac) or CTRL+Click / CTRL+Enter (non-Mac), the contents will open in a new pane, rather than replacing the currently open pane.

---

## Review Changes (Manager)

### Review Changes + Source Control

Just as in the editor, you can easily review the work you and your agent have collaborated on from within the manager.

Once you enter a conversation, you can toggle the Review Changes pane through the button at the top right to open up a pane where you can scroll through and comment on any file diffs made as a part of the conversation.

You can similarly toggle to the Source Control tab within the Review Changes pane to see changed files, stage or unstage them, and commit them upstream.

---

## Changes Sidebar

### Changes Sidebar

Similar to the toolbar at the bottom of the editor's Agent Panel, the Changes Sidebar in the manager offers a quick way to see what artifacts the agent has created and what files it has modified within a conversation.

Clicking on any of the listed resources will open its contents within a pane. The icons on each resource indicate whether there are new changes to a resource since your last review.

---

## Terminal

### Terminal

The agent manager window has terminal support as well! To toggle this, use Cmd/Ctrl + J to open the bottom pane of the agent manager, which is where terminals live. They are attached to the workspace that your current conversation is in.

**Note**: Agent manager window's terminal integration works for local workspaces only, and Agent-used terminals run inside the editor window.

---

## Files

### Files

As you open up file panes within the manager, you can also leave comments for the agent to highlight specific points.

---

## Browser

### Browser

Antigravity has the ability to open, read, and control a Chrome browser, allowing you to test development websites, read internet data sources, and automate various different browser tasks.

Using a subagent, Antigravity can operate on the browser as it sees fit, as well as recording its actions and presenting select screenshots and videos as artifacts.

To isolate the Antigravity agent from your normal browsing, it runs in a separate browser profile. This means that it will show up as a separate application in your dock and nothing will be signed in. You can read more about this in the Separate Chrome Profile section.

To disable the use of all browser tools, you may disable the browser tools setting in the "Browser" section of your settings.

Antigravity detects and uses your existing Chrome application. If you don't already have Chrome, you must download it here. If Antigravity is unable to detect your chrome installation, you may have to specify the path to it in the following setting.

---

## Chrome Extension

### Chrome Extension

Antigravity Browser Extension is required for the Antigravity Agent to access the web.

It empowers the agent to see and interact with websites to complete your development goals, whether you are building a site from scratch or automating a workflow.

It enhances the user experience by allowing the user to cancel the current conversation from the browser, switch the focus back to Antigravity from the web agent is working on, and more seamlessly work parallely with the browser agent.

The first time you use the Antigravity browser agent, you should be directed to the Chrome Web Store to install the extension.

### Troubleshooting

If for any reason you need to manually install it:

1. Click the Chrome icon in Antigravity (opens Chrome with Antigravity user profile)
   - In the editor, you can find the Chrome icon on the top right
   - In the Agent Manager, you can find the Chrome icon on the bottom left
2. Navigate to the URL and click "Add to Chrome"

---

## Allowlist / Denylist

### Allowlist / Denylist

The browser uses a two-layer security system to control which URLs can be accessed:

- **Denylist** - Deny dangerous/malicious URLs
- **Allowlist** - Explicitly allow trusted URLs

### How It Works

#### Denylist

The denylist is maintained and enforced using the Google Superroots's BadUrlsChecker service. When the browser attempts to navigate to a URL, the hostname is checked against the server-side denylist via RPC.

**NOTE**: If the server is unavailable, access is denied by default.

#### Allowlist

The allowlist is a local text file that you can edit to explicitly trust specific URLs.

Optionally, you can prepopulate the allowlist with a default set of URLs during onboarding.

When the browser attempts to navigate to a non-allowlisted URL, it will prompt you with an "always allow" button, which if clicked will add the URL to the allowlist and enable the browser to open and interact with the web page.

You can also add/remove URLS from the allowlist manually. However, the denylist always takes precedence: you cannot allowlist a URL that appears on the denylist.

---

## Separate Chrome Profile

### Separate Chrome Profile

To isolate the browser from your general browsing, it operates on a separate Chrome profile.

Since Chrome profiles are isolated, this will not share any of the cookies or sign-in information from your normal browsing profile. However, all sign-ins will be persisted such that anytime you open the browser in the future, all your accounts will still be there. This profile should also contain your installation of the Chrome Extension, so that your normal browsing profile is entirely unaffected.

If you had your normal Chrome open while launching this profile, it will show up as a separate dock icon and be considered a separate application. If Chrome was not open beforehand, this application will look the same as your default profile. To return to the default profile, you must quit the application and relaunch Chrome.

If you would like to change the location where your browser profile will be created, you can modify the following setting in the browser section.

---

## Plans

### Plans

At this moment, Google Antigravity is only available as a no-cost public preview. This plan is for use by individual accounts, with terms derived from Google's terms of service, not Google Cloud's enterprise terms of service.

Within the public preview, users receive:

- A generous rate limit of Gemini 3 Pro
- A more conservative combined rate limit of other offered Vertex Model Garden models

These rate limits are primarily determined to the degree we have capacity, and exist to prevent abuse. Quota is refreshed every five hours. Under the hood, the rate limits are correlated with the amount of work done by the agent, which can differ from prompt to prompt. Thus, you may get many more prompts if your tasks are more straightforward and the agent can complete the work quickly, and the opposite is also true. Our modeling suggests that a very small fraction of power users will ever hit the per-five-hour rate limit, so our hope is that this is something that you won't have to worry about, and you feel unrestrained in your usage of Antigravity.

There is currently no support for:

- Paid tiers with guaranteed quotas and rate limits
- Bring-your-own-key or bring-your-own-endpoint for additional rate limits
- Organizational tiers (self-serve or via contract)

---

## Settings

### Settings

You can configure your Antigravity settings across Agent, Browser, Editor, and more via:

- Keyboard shortcut in any surface: Cmd + ,
- From the Settings tab or gear icon in the Agent Manager
- From "Settings > Open Antigravity User Settings" in the Editor

---

## FAQ

### Why can I not authenticate into Google Antigravity?

Google Antigravity is currently available for non-Workspace personal Google accounts in approved geographies. Please try using an @gmail.com email address if having challenges with Workspace Google accounts (even if used for personal purposes).

### What is Google Antigravity's geographical availability?

Google Antigravity is available in the following countries and territories. If you're not in one of these countries or territories, you will be unable to use Google Antigravity at this time.

*(List of countries available in official documentation)*

### What is Google Antigravity's stance on data collection?

Please refer to the Terms of Service. You may opt out of data collection at any point from the Settings panel.

### How do I get support?

During this no-cost public preview period, there will be limited support at **antigravity-support@google.com**

### What are the model rate limits?

Please see more details in the docs on Plans.

### Does Google Antigravity currently support worktrees?

Not at the moment.

### What happens when my computer goes to sleep?

If an agent is running, Antigravity will prevent your computer from sleeping.
