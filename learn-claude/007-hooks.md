# Claude Code Hook Events

Hooks are shell commands, HTTP endpoints, MCP tools, prompts, or sub-agents that Claude Code executes in response to events. They run **outside** of Claude — the harness invokes them, so they can enforce policy, inject context, or block actions that memory/preferences alone cannot.

## Session Lifecycle

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `SessionStart` | Session begins or resumes | `startup`, `resume`, `clear`, `compact` | Observe, add context |
| `Setup` | Running with `--init-only` or `--init`/`--maintenance` in prompt mode | `init`, `maintenance` | Observe, add context |
| `SessionEnd` | Session terminates | `clear`, `resume`, `logout`, `prompt_input_exit`, `bypass_permissions_disabled`, `other` | Observe only |

## User Input

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `UserPromptSubmit` | User submits a prompt, before Claude processes it | None | Block, add context, set session title |
| `UserPromptExpansion` | Slash command expands into a prompt | Command/skill name | Block, add context |

## Tool Execution

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `PreToolUse` | Before any tool executes | Tool name: `Bash`, `Edit`, `Write`, MCP tool names, etc. | Allow, deny, ask, defer, modify input |
| `PostToolUse` | After tool succeeds | Tool name | Observe, add context |
| `PostToolUseFailure` | After tool fails | Tool name | Observe, add context |
| `PostToolBatch` | After parallel batch of tool calls resolves | None | Block/continue |
| `PermissionRequest` | Permission dialog appears | Tool name | Allow, deny, modify input |
| `PermissionDenied` | Auto mode classifier denies a tool call | Tool name | Suggest retry |

**PreToolUse output schema:**
```json
{"hookSpecificOutput": {"permissionDecision": "deny|allow|ask|defer", "updatedInput": {...}}}
```

**PostToolUse output schema:**
```json
{"hookSpecificOutput": {"additionalContext": "..."}}
```

**PermissionRequest output schema:**
```json
{"hookSpecificOutput": {"decision": {"behavior": "allow|deny", "updatedInput": {...}}}}
```

## Agents & Tasks

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `SubagentStart` | Subagent is spawned | Agent type (`general-purpose`, `Explore`, `Plan`, custom names) | Observe only |
| `SubagentStop` | Subagent finishes | Agent type | Block |
| `TeammateIdle` | Agent team teammate about to go idle | None | Block |
| `TaskCreated` | Task created via `TaskCreate` | None | Block |
| `TaskCompleted` | Task marked as completed | None | Block |
| `Stop` | Claude finishes responding | None | Block |
| `StopFailure` | Turn ends due to API error | Error type: `rate_limit`, `authentication_failed`, `billing_error`, etc. | Observe only |

## Context & Configuration

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `InstructionsLoaded` | `CLAUDE.md` or `.claude/rules/*.md` loaded | Load reason: `session_start`, `nested_traversal`, `path_glob_match`, `include`, `compact` | Observe only |
| `ConfigChange` | Configuration file changes during session | Config source: `user_settings`, `project_settings`, `local_settings`, `policy_settings`, `skills` | Block |
| `CwdChanged` | Working directory changes | None | Observe only |
| `FileChanged` | Watched file changes on disk | Literal filenames: e.g., `.envrc`, `.env` | Observe only |

## Workspace

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `WorktreeCreate` | Worktree being created | None | Replace default behavior, return path |
| `WorktreeRemove` | Worktree being removed | None | Observe only |
| `PreCompact` | Before context compaction | Trigger: `manual`, `auto` | Block |
| `PostCompact` | After context compaction completes | Trigger: `manual`, `auto` | Observe only |

**WorktreeCreate output schema:**
```json
{"hookSpecificOutput": {"worktreePath": "/path"}}
```

## MCP

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `Elicitation` | MCP server requests user input during tool call | MCP server name | Accept/decline/cancel, provide form values |
| `ElicitationResult` | User responds to MCP elicitation, before response sent back | MCP server name | Override action/content |

## Notifications

| Event | When it Fires | Matchers / Input | Capabilities |
|---|---|---|---|
| `Notification` | Claude Code sends a notification | Notification type: `permission_prompt`, `idle_prompt`, `auth_success`, `elicitation_dialog`, etc. | Observe only |

---

## Configuration

Hooks are configured in `settings.json`. Locations:

- `~/.claude/settings.json` — user-level, applies to all projects
- `.claude/settings.json` — project-level, shareable via git
- `.claude/settings.local.json` — project-level local override, not shared

### Settings.json shape

```json
{
  "hooks": {
    "EVENT_NAME": [
      {
        "matcher": "FILTER_PATTERN_OR_*",
        "hooks": [
          {
            "type": "command|http|mcp_tool|prompt|agent",
            "timeout": 600,
            "statusMessage": "Optional spinner text"
          }
        ]
      }
    ]
  }
}
```

### Matcher patterns

- Exact string: `"Bash"`
- Pipe-separated list: `"Edit|Write"`
- Regex with special chars: `"^Notebook"`
- Wildcard: `"*"` (all)

### Universal output fields

These fields can appear in any hook's JSON output regardless of event type:

- `continue` — proceed with the action
- `stopReason` — reason shown to user if stopping
- `suppressOutput` — hide hook output from the transcript
- `systemMessage` — message shown to Claude
- `terminalSequence` — raw terminal control sequence to emit

---

Official docs: https://code.claude.com/docs/en/hooks-guide.md
