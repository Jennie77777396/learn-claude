# Claude Code Built-In Sub-Agents

Sub-agents (also called "agent types") are specialized assistants that Claude Code can spawn via the Agent tool. Each has a focused purpose and a restricted toolset. Launch them with the `Agent` tool, setting `subagent_type` to one of the names below.

Multiple agents launched in a single message run **concurrently**.

## `claude`
**Purpose:** Catch-all for any task that doesn't fit a more specific agent. FleetView's default when no agent name is typed.
**Tools:** All tools (`*`)
**When to use:** General delegation when no specialized agent fits.

## `claude-code-guide`
**Purpose:** Answers questions about:
1. **Claude Code** (the CLI tool) — features, hooks, slash commands, MCP servers, settings, IDE integrations, keyboard shortcuts
2. **Claude Agent SDK** — building custom agents
3. **Claude API** (formerly Anthropic API) — API usage, tool use, Anthropic SDK usage

**Tools:** `Glob`, `Grep`, `Read`, `WebFetch`, `WebSearch`
**When to use:** "Can Claude...", "Does Claude...", "How do I..." questions about Claude Code, the Agent SDK, or the Claude API.
**Note:** Before spawning a new one, check whether a running or recent `claude-code-guide` agent can be continued via `SendMessage`.

## `Explore`
**Purpose:** Fast read-only search agent for locating code — find files by pattern, grep for symbols/keywords, answer "where is X defined / which files reference Y."
**Tools:** All tools except `Agent`, `ExitPlanMode`, `Edit`, `Write`, `NotebookEdit`
**When to use:** Targeted lookups across the codebase.
**When NOT to use:** Code review, design-doc auditing, cross-file consistency checks, open-ended analysis — it reads excerpts, not whole files, and will miss content past its read window.
**Search breadth parameter:**
- `quick` — single targeted lookup
- `medium` — moderate exploration
- `very thorough` — search multiple locations and naming conventions

## `general-purpose`
**Purpose:** Researching complex questions, searching for code, and executing multi-step tasks.
**Tools:** All tools (`*`)
**When to use:** Searching for a keyword or file when you are not confident you will find the right match in the first few tries, or open-ended research and multi-step work.

## `Plan`
**Purpose:** Software architect agent for designing implementation plans. Returns step-by-step plans, identifies critical files, and considers architectural trade-offs.
**Tools:** All tools except `Agent`, `ExitPlanMode`, `Edit`, `Write`, `NotebookEdit`
**When to use:** Planning the implementation strategy for a task before writing code.

## `statusline-setup`
**Purpose:** Configures Claude Code's status line setting.
**Tools:** `Read`, `Edit`
**When to use:** When the user wants to set up or modify the status line.

---

## How to launch a sub-agent

Use the `Agent` tool with these parameters:

| Parameter | Purpose |
|---|---|
| `subagent_type` | One of the agent names above (defaults to `general-purpose` if omitted) |
| `description` | Short 3–5 word task summary |
| `prompt` | The self-contained task for the agent (it has no prior conversation context) |
| `run_in_background` | Optional — `true` to run asynchronously with a notification on completion |
| `model` | Optional override (`opus`, `sonnet`, `haiku`) |
| `isolation` | Optional — `worktree` runs the agent in a temporary git worktree |

## Prompt-writing tips

- Brief the agent like a colleague who just walked in — it hasn't seen your conversation.
- Explain the goal *and* what you've ruled out.
- For lookups, hand over the exact command. For investigations, hand over the question.
- Cap response length when you need it short ("under 200 words").
- **Never delegate understanding** — don't write "based on your findings, fix the bug." Synthesize the agent's report yourself, then act.

---

Official docs: https://code.claude.com/docs/en/sub-agents.md
