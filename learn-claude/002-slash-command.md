# Claude Code Slash Commands

A reference of built-in slash commands and bundled skills in Claude Code.

Commands marked **[Skill]** are bundled skills (Claude can also invoke these automatically when relevant). Availability varies by platform, plan, and environment.

## Session Management
- `/clear [name]` — Start a new conversation with empty context, optionally labeling the previous one
- `/resume [session]` — Resume a conversation by ID, name, or open the session picker
- `/branch [name]` — Create a branch of the current conversation at this point
- `/rename [name]` — Rename the current session, or auto-generate from conversation history

## Context & Performance
- `/context [all]` — Visualize current context usage as a colored grid with optimization suggestions
- `/compact [instructions]` — Free up context by summarizing the conversation so far
- `/recap` — Generate a one-line summary of the current session on demand

## Code Review & Quality
- `/diff` — Open an interactive diff viewer showing uncommitted changes and per-turn diffs
- `/review [PR]` — Review a pull request locally in the current session
- `/security-review` — Analyze pending changes on the current branch for security vulnerabilities
- `/simplify [focus]` — **[Skill]** Review recently changed files for quality and efficiency issues, then fix them
- `/ultrareview [PR]` — Run a deep, multi-agent code review in a cloud sandbox

## Model & Configuration
- `/model [model]` — Set the AI model for the current session
- `/effort [level|auto]` — Set the model effort level (low, medium, high, xhigh, max)
- `/fast [on|off]` — Toggle fast mode on or off
- `/config` — Open the Settings interface to adjust theme, model, output style, and preferences

## Project Setup
- `/init` — Initialize project with a `CLAUDE.md` guide
- `/memory` — Edit `CLAUDE.md` memory files and manage auto-memory
- `/agents` — Manage agent/subagent configurations
- `/mcp` — Manage MCP server connections and OAuth authentication
- `/permissions` — Manage allow, ask, and deny rules for tool permissions
- `/skills` — List available skills and manage visibility

## Workflow & Planning
- `/plan [description]` — Enter plan mode directly from the prompt
- `/batch <instruction>` — **[Skill]** Orchestrate large-scale changes across a codebase in parallel
- `/goal [condition|clear]` — Set a goal that Claude keeps working toward until the condition is met
- `/ultraplan <prompt>` — Draft a plan in an ultraplan session, review in browser, then execute

## Automation & Scheduling
- `/loop [interval] [prompt]` — **[Skill]** Run a prompt repeatedly while the session stays open
- `/schedule [description]` — Create or manage routines that execute on cloud infrastructure
- `/background [prompt]` — Detach current session to run as background agent and free terminal
- `/autofix-pr [prompt]` — Spawn a web session that watches a PR and pushes fixes on CI failure

## Development Tools
- `/run` — **[Skill]** Launch and drive your project's app to see a change working
- `/run-skill-generator` — **[Skill]** Teach `/run` and `/verify` how to build and launch your project
- `/verify` — **[Skill]** Confirm a code change works by building and running the app
- `/rewind` — Rewind the conversation and/or code to a previous point or summarize from a message

## Navigation & Integration
- `/teleport` — Pull a Claude Code on the web session into your terminal
- `/desktop` — Continue the current session in the Claude Code Desktop app
- `/remote-control` — Make this session available for remote control from claude.ai
- `/add-dir <path>` — Add a working directory for file access during the current session

## Help & Debugging
- `/help` — Show help and available commands
- `/doctor` — Diagnose and verify your Claude Code installation and settings
- `/debug [description]` — **[Skill]** Enable debug logging and troubleshoot issues
- `/status` — Show version, model, account, and connectivity status
- `/feedback [report]` — Submit feedback, report a bug, or share your conversation
- `/hooks` — View hook configurations for tool events
- `/ide` — Manage IDE integrations and show status
- `/keybindings` — Open or create your keybindings configuration file

## Display & Preferences
- `/theme` — Change the color theme (includes colorblind-accessible and custom options)
- `/color [color|default]` — Set the prompt bar color for the current session
- `/tui [default|fullscreen]` — Set the terminal UI renderer (fullscreen or default)
- `/focus` — Toggle the focus view showing only the last prompt and final response
- `/scroll-speed` — Adjust mouse wheel scroll speed interactively
- `/statusline` — Configure Claude Code's status line with custom information

## Data & Output
- `/export [filename]` — Export the current conversation as plain text
- `/copy [N]` — Copy the last (or Nth-latest) assistant response to clipboard
- `/btw <question>` — Ask a quick side question without adding to the conversation

## Account & Authentication
- `/login` — Sign in to your Anthropic account
- `/logout` — Sign out from your Anthropic account
- `/upgrade` — Open the upgrade page to switch to a higher plan tier
- `/passes` — Share a free week of Claude Code with friends (if eligible)

## Usage & Analytics
- `/usage` — Show session cost, plan usage limits, and activity stats
- `/insights` — Generate a report analyzing your Claude Code sessions
- `/team-onboarding` — Generate a team onboarding guide from your usage history
- `/heapdump` — Write a JavaScript heap snapshot and memory breakdown for diagnostic purposes

## Third-Party Integrations & Setup
- `/chrome` — Configure Claude in Chrome settings
- `/install-github-app` — Set up Claude GitHub Actions app for a repository
- `/install-slack-app` — Install the Claude Slack app
- `/setup-bedrock` — Configure Amazon Bedrock authentication and settings
- `/setup-vertex` — Configure Google Vertex AI authentication and settings
- `/web-setup` — Connect GitHub account to Claude Code on the web
- `/remote-env` — Configure the default remote environment for web sessions
- `/terminal-setup` — Configure terminal keybindings for Shift+Enter and shortcuts

## Skills & Plugins
- `/claude-api [migrate|managed-agents-onboard]` — **[Skill]** Load Claude API reference material or upgrade code to newer model
- `/fewer-permission-prompts` — **[Skill]** Scan transcripts for common calls and add allowlist to reduce permission prompts
- `/plugin` — Manage Claude Code plugins
- `/reload-plugins` — Reload all active plugins to apply pending changes
- `/powerup` — Discover Claude Code features through quick interactive lessons
- `/voice [hold|tap|off]` — Toggle voice dictation or enable in a specific mode

## Utilities
- `/tasks` — List and manage background tasks
- `/stop` — Stop the current background session
- `/exit` — Exit the CLI
- `/mobile` — Show QR code to download the Claude mobile app
- `/radio` — Open Claude FM lo-fi radio in your browser
- `/release-notes` — View the changelog in an interactive version picker
- `/stickers` — Order Claude Code stickers
- `/privacy-settings` — View and update your privacy settings
- `/usage-credits` — Configure usage credits to keep working when hitting a limit

---

Official docs: https://code.claude.com/docs/en/commands.md
