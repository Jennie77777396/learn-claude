# Claude Code Slash Commands

A reference of built-in slash commands and bundled skills, with deeper detail on the ones you'll reach for most often.

Commands marked **[Skill]** are bundled skills — Claude can also invoke them automatically when relevant. Availability varies by platform, plan, and environment.

---

## Must-Know First (Day 1)

If you only learn 10 commands to start, these are the ones:

| Command | Why it's essential |
|---|---|
| `/help` | See every command available in your session |
| `/init` | Generate a `CLAUDE.md` so Claude understands your project |
| `/clear` | Start a fresh conversation when switching tasks |
| `/compact` | Free up context without losing the thread you're on |
| `/context` | See *where* your context window is going |
| `/permissions` | Allow / deny tool access without leaving the chat |
| `/model` | Switch model mid-session (Opus / Sonnet / Haiku) |
| `/resume` | Jump back into an earlier conversation |
| `/memory` | Edit `CLAUDE.md` and manage auto-memory |
| `/doctor` | Diagnose setup issues; press `f` to auto-fix |

---

## Deep Dives — The Commands You'll Use Daily

### `/init`
Creates a starter `CLAUDE.md` documenting your project so Claude has context every session.

**When:** First session in a new repo.
**Gotcha:** Requires a git repo; won't generate a file outside version control. Set `CLAUDE_CODE_NEW_INIT=1` for an interactive walkthrough that also sets up skills and personal memory.

### `/clear` vs `/compact`
Both manage context, but very differently:

| | `/clear [name]` | `/compact [instructions]` |
|---|---|---|
| Effect | Brand-new conversation, empty context | Same conversation, summarized history |
| Prior session | Preserved — re-open with `/resume` | Gone (replaced by summary) |
| Skills / memory | Reset | Re-attached (most recent first, 5k tokens each, 25k total budget) |
| Use when | Switching to unrelated work | Long thread you're still continuing |

`/compact` accepts an optional instruction that biases what the summary emphasizes (e.g. `/compact focus on the migration plan`).

### `/context [all]`
Renders your context window as a colored grid with optimization suggestions and per-tool breakdowns.

**When:** You're hitting limits or want to know what's eating tokens.
**Gotcha:** In fullscreen mode, pass `all` to expand collapsed entries.

### `/recap`
One-line summary of the current session on demand. Also appears automatically when you return after time away (controlled by `awaySummaryEnabled`).

### `/rewind`
Rolls the conversation **and code** back to a previous turn or checkpoint, or summarizes a section of conversation.

**Gotcha:** Irreversible on the current branch, but the original session is still in `/resume` if you need it.

### `/permissions`
Interactive dialog for tool allow / ask / deny rules, plus working-directory management.

**When:** Granting persistent tool access without editing `settings.json` by hand.
**Gotcha:** Rules from `.claude/settings.json` take effect immediately; changes here persist across sessions. See [003-settings.md](003-settings.md) for the schema.

### `/model`, `/effort`, `/fast`
Tune the cost/quality dial:

- **`/model [model]`** — opens a picker; arrow keys browse, `d` saves as default. Switching models re-reads full history *without* cached context, so you'll be prompted for confirmation.
- **`/effort [low|medium|high|xhigh|max|auto]`** — reasoning depth, takes effect immediately for the *next* response (current response in flight isn't interrupted).
- **`/fast [on|off]`** — toggles fast mode for quicker, lower-quality responses. Available on Opus 4.6 and 4.7.

### `/agents`
Browse and configure sub-agents (built-in + custom). See [006-sub-agents.md](006-sub-agents.md) for the agent types.

### `/memory`
Edit `CLAUDE.md` files, enable/disable auto-memory, view auto-memory entries.

**When:** Refining persistent context, cleaning up stale memories. See [004-memory.md](004-memory.md).

### `/mcp`
Manage MCP server connections and OAuth authentication.

**When:** Connecting external tools (Slack, Figma, Atlassian, Salesforce, etc.). OAuth flows open in the browser. See [008-mcp.md](008-mcp.md).

### `/skills`
Lists available skills with token counts.
- `Space` — toggle visibility (cycle on / name-only / user-invocable-only / off)
- `t` — sort by token count
- `Enter` — save changes to `.claude/settings.local.json`

### `/hooks`
View configured hook entries from `settings.json`. Read-only — use the `/update-config` skill to edit. See [007-hooks.md](007-hooks.md).

### `/help` and `/doctor`
- **`/help`** — full command listing in-session
- **`/doctor`** — diagnose installation, settings, and connectivity. Press `f` to auto-fix detected issues.

### `/usage` (aliases: `/cost`, `/stats`)
Session cost, plan usage limits, and activity stats.

### `/export` and `/copy`
- **`/export [filename]`** — save the conversation as plain text (file or clipboard)
- **`/copy [N]`** — copy the last (or Nth-latest) assistant response. Opens a picker for individual code blocks; press `w` in the picker to write a file instead of using the clipboard (useful over SSH).

### `/resume` and `/branch`
- **`/resume [session]`** — open the session picker. Background sessions are marked `bg`.
- **`/branch [name]`** — fork the conversation at this point; the original remains resumable.

**Gotcha:** Each branch is a separate session that costs context — don't branch reflexively.

### `/plan [description]`
Enter plan mode directly, optionally with a starting task. Claude drafts a plan before executing.

**Gotcha:** Plan mode runs as a sub-agent; it doesn't see the full conversation history.

### `/review` and `/security-review`
- **`/review [PR]`** — review a pull request locally (fetches with `gh`). Lighter than `/ultrareview`.
- **`/security-review`** — analyzes pending git diff for security vulnerabilities (injection, auth, data leaks). Read-only.

Both need a git repo and (for `/review`) `gh` set up.

### `/add-dir <path>`
Grants Claude file access to another directory for this session.

**Gotcha:** Skills in `.claude/skills/` *are* loaded from added dirs, but other config (sub-agents, hooks) is *not*.

### `/feedback [report]` (aliases: `/bug`, `/share`)
Submit feedback, report a bug, or share your session with Anthropic.

**Gotcha:** Includes session context by default for triage. Review before sending if it contains anything sensitive.

### `/config`
Opens the Settings UI for theme, model, output style, and preferences — quick alternative to editing `settings.json`.

---

## Argument Syntax

Commands that take arguments accept them positionally after the command name:

```
/review 1234
/branch fix-auth-bug
/permissions add Bash(npm run *)
```

Custom slash commands (i.e. skills with `user-invocable: true`) can reference args in their body:

| Reference | Expands to |
|---|---|
| `$ARGUMENTS` | All arguments as a single string |
| `$1`, `$2`, `$ARGUMENTS[N]` | Specific positional arg (0-based) |
| `$name` | Named argument when `arguments:` is declared in frontmatter |

Show an expected-args hint in the `/` menu:

```yaml
---
argument-hint: [pr-number] [reviewer]
arguments: [pr, reviewer]
---

Review PR #$pr with $reviewer as the second pair of eyes.
```

See [005-skills.md](005-skills.md) for the full skill-authoring story.

## Tab Completion

- Type `/` to open the menu — shows all built-in commands + skills, filtered as you type letters
- The `argument-hint` from a skill's frontmatter appears in the menu when you hover
- No configuration needed — discovery is automatic from `~/.claude/skills/`, `.claude/skills/`, and plugins

## Custom Slash Commands

Custom commands are just **skills** with a `SKILL.md`:

```
.claude/skills/my-command/SKILL.md   ← project, shared via git
~/.claude/skills/my-command/SKILL.md  ← personal, all your projects
```

The directory name becomes the command name. Frontmatter controls:
- `disable-model-invocation: true` → manual-only (you type `/my-command`)
- `user-invocable: false` → Claude-only (auto-loads when relevant; no slash entry)
- `allowed-tools: Bash(git *)` → pre-approved tools while running

The legacy `.claude/commands/` directory still works but new work should use skills. Full details in [005-skills.md](005-skills.md).

---

## Common Workflows

**Starting fresh on a new repo**
```
/init           # generate CLAUDE.md
/permissions    # whitelist your common commands
/memory         # add anything that should always be known
```

**Long session running out of context**
```
/context        # see what's filling up
/compact        # summarize history, keep going
```
*Switching to unrelated work?* Use `/clear` instead.

**Tuning cost vs. quality**
```
/model          # pick Opus / Sonnet / Haiku
/effort high    # boost reasoning depth
/fast on        # if you want speed > quality
```

**Code review before a PR**
```
/diff           # see exactly what changed
/security-review
/review <PR#>   # if you've already pushed
```

**Recovering from a bad direction**
```
/rewind         # roll back conversation + code
# OR
/clear; /resume # jump out, then jump back into an earlier session
```

---

## Full Command Catalog

### Session Management
- `/clear [name]` — Start a new conversation with empty context, optionally labeling the previous one
- `/resume [session]` — Resume a conversation by ID, name, or open the session picker
- `/branch [name]` — Create a branch of the current conversation at this point
- `/rename [name]` — Rename the current session, or auto-generate from conversation history

### Context & Performance
- `/context [all]` — Visualize current context usage as a colored grid with optimization suggestions
- `/compact [instructions]` — Free up context by summarizing the conversation so far
- `/recap` — Generate a one-line summary of the current session on demand

### Code Review & Quality
- `/diff` — Open an interactive diff viewer showing uncommitted changes and per-turn diffs
- `/review [PR]` — Review a pull request locally in the current session
- `/security-review` — Analyze pending changes on the current branch for security vulnerabilities
- `/simplify [focus]` — **[Skill]** Review recently changed files for quality and efficiency issues, then fix them
- `/ultrareview [PR]` — Run a deep, multi-agent code review in a cloud sandbox

### Model & Configuration
- `/model [model]` — Set the AI model for the current session
- `/effort [level|auto]` — Set the model effort level (low, medium, high, xhigh, max)
- `/fast [on|off]` — Toggle fast mode on or off
- `/config` — Open the Settings interface to adjust theme, model, output style, and preferences

### Project Setup
- `/init` — Initialize project with a `CLAUDE.md` guide
- `/memory` — Edit `CLAUDE.md` memory files and manage auto-memory
- `/agents` — Manage agent/subagent configurations
- `/mcp` — Manage MCP server connections and OAuth authentication
- `/permissions` — Manage allow, ask, and deny rules for tool permissions
- `/skills` — List available skills and manage visibility

### Workflow & Planning
- `/plan [description]` — Enter plan mode directly from the prompt
- `/batch <instruction>` — **[Skill]** Orchestrate large-scale changes across a codebase in parallel
- `/goal [condition|clear]` — Set a goal that Claude keeps working toward until the condition is met
- `/ultraplan <prompt>` — Draft a plan in an ultraplan session, review in browser, then execute

### Automation & Scheduling
- `/loop [interval] [prompt]` — **[Skill]** Run a prompt repeatedly while the session stays open
- `/schedule [description]` — Create or manage routines that execute on cloud infrastructure
- `/background [prompt]` — Detach current session to run as background agent and free terminal
- `/autofix-pr [prompt]` — Spawn a web session that watches a PR and pushes fixes on CI failure

### Development Tools
- `/run` — **[Skill]** Launch and drive your project's app to see a change working
- `/run-skill-generator` — **[Skill]** Teach `/run` and `/verify` how to build and launch your project
- `/verify` — **[Skill]** Confirm a code change works by building and running the app
- `/rewind` — Rewind the conversation and/or code to a previous point or summarize from a message

### Navigation & Integration
- `/teleport` — Pull a Claude Code on the web session into your terminal
- `/desktop` — Continue the current session in the Claude Code Desktop app
- `/remote-control` — Make this session available for remote control from claude.ai
- `/add-dir <path>` — Add a working directory for file access during the current session

### Help & Debugging
- `/help` — Show help and available commands
- `/doctor` — Diagnose and verify your Claude Code installation and settings
- `/debug [description]` — **[Skill]** Enable debug logging and troubleshoot issues
- `/status` — Show version, model, account, and connectivity status
- `/feedback [report]` — Submit feedback, report a bug, or share your conversation
- `/hooks` — View hook configurations for tool events
- `/ide` — Manage IDE integrations and show status
- `/keybindings` — Open or create your keybindings configuration file

### Display & Preferences
- `/theme` — Change the color theme (includes colorblind-accessible and custom options)
- `/color [color|default]` — Set the prompt bar color for the current session
- `/tui [default|fullscreen]` — Set the terminal UI renderer (fullscreen or default)
- `/focus` — Toggle the focus view showing only the last prompt and final response
- `/scroll-speed` — Adjust mouse wheel scroll speed interactively
- `/statusline` — Configure Claude Code's status line with custom information

### Data & Output
- `/export [filename]` — Export the current conversation as plain text
- `/copy [N]` — Copy the last (or Nth-latest) assistant response to clipboard
- `/btw <question>` — Ask a quick side question without adding to the conversation

### Account & Authentication
- `/login` — Sign in to your Anthropic account
- `/logout` — Sign out from your Anthropic account
- `/upgrade` — Open the upgrade page to switch to a higher plan tier
- `/passes` — Share a free week of Claude Code with friends (if eligible)

### Usage & Analytics
- `/usage` — Show session cost, plan usage limits, and activity stats
- `/insights` — Generate a report analyzing your Claude Code sessions
- `/team-onboarding` — Generate a team onboarding guide from your usage history
- `/heapdump` — Write a JavaScript heap snapshot and memory breakdown for diagnostic purposes

### Third-Party Integrations & Setup
- `/chrome` — Configure Claude in Chrome settings
- `/install-github-app` — Set up Claude GitHub Actions app for a repository
- `/install-slack-app` — Install the Claude Slack app
- `/setup-bedrock` — Configure Amazon Bedrock authentication and settings
- `/setup-vertex` — Configure Google Vertex AI authentication and settings
- `/web-setup` — Connect GitHub account to Claude Code on the web
- `/remote-env` — Configure the default remote environment for web sessions
- `/terminal-setup` — Configure terminal keybindings for Shift+Enter and shortcuts

### Skills & Plugins
- `/claude-api [migrate|managed-agents-onboard]` — **[Skill]** Load Claude API reference material or upgrade code to newer model
- `/fewer-permission-prompts` — **[Skill]** Scan transcripts for common calls and add allowlist to reduce permission prompts
- `/plugin` — Manage Claude Code plugins
- `/reload-plugins` — Reload all active plugins to apply pending changes
- `/powerup` — Discover Claude Code features through quick interactive lessons
- `/voice [hold|tap|off]` — Toggle voice dictation or enable in a specific mode

### Utilities
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
