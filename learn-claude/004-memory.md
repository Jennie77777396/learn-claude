# Auto Memory

Auto memory is a persistent, file-based memory system Claude Code uses across sessions. Claude reads it at the start of every conversation and writes to it during work, so future sessions inherit a picture of who you are, how you collaborate, and the context behind ongoing work.

## Storage Location

| Platform | Default Path |
|---|---|
| macOS / Linux | `~/.claude/memory/` |
| Windows | `%USERPROFILE%\.claude\projects\<project-id>\memory\` |

Override with the `autoMemoryDirectory` setting in `settings.json`. Disable entirely with `autoMemoryEnabled: false`.

## How It Differs From `CLAUDE.md`

| | Auto memory | `CLAUDE.md` |
|---|---|---|
| Loaded | Selectively, by Claude | Always, every session |
| Written by | Claude (during conversations) | You (manually) |
| Scope | User-level, cross-session | Project-level (or user) |
| Best for | Profile, feedback, project state | Conventions, build commands, rules |

Auto memory is what Claude *learns* about you; `CLAUDE.md` is what you *tell* Claude up front.

## The Four Memory Types

### `user`
Information about your role, goals, responsibilities, and knowledge.
**Saved when:** Claude learns who you are or what you focus on.
**Used to:** Tailor responses to your perspective and expertise level.

### `feedback`
Guidance you've given Claude about how to approach work — corrections *and* validated approaches.
**Saved when:** You correct Claude's approach ("don't do X") or confirm a non-obvious choice was right ("yes, that was the right call").
**Body structure:** rule → **Why:** (the reason or past incident) → **How to apply:** (when/where it kicks in).

### `project`
Facts about ongoing work, initiatives, bugs, or decisions that aren't visible in the code or git history.
**Saved when:** You share who is doing what, why, or by when.
**Body structure:** fact → **Why:** → **How to apply:**.
**Convention:** relative dates are converted to absolute ("Thursday" → "2026-03-05").

### `reference`
Pointers to where information lives in external systems (Linear, Slack, Grafana, etc.).
**Saved when:** You mention where to find context outside this repo.
**Used to:** Tell Claude where to look when you reference an external resource.

## File Format

Each memory is one file with YAML frontmatter:

```markdown
---
name: short-kebab-case-slug
description: one-line summary used to decide relevance in future conversations
metadata:
  type: user | feedback | project | reference
---

Memory content. For feedback/project, structure as:
- the rule or fact
- **Why:** the reason given
- **How to apply:** when this guidance kicks in

Link related memories with [[their-slug]].
```

## The `MEMORY.md` Index

`MEMORY.md` sits at the top of the memory directory and is loaded into every conversation. It is an *index*, not a memory — one line per entry, under ~150 chars:

```markdown
- [User role](user_role.md) — senior backend engineer, learning React
- [Test policy](feedback_testing.md) — never mock the database in integration tests
- [Auth rewrite](project_auth.md) — driven by compliance, not tech debt
```

Lines after #200 are truncated, so keep it concise. Never write memory content directly into `MEMORY.md`.

## What NOT to Save

Even when explicitly asked to save, Claude declines (or rephrases) these:

- Code patterns, conventions, file paths, architecture — derivable from the project
- Git history or who-changed-what — `git log` / `git blame` are authoritative
- Debugging solutions or fix recipes — the fix is in the code
- Anything already documented in `CLAUDE.md`
- Ephemeral task details, in-progress work, current conversation context

For activity summaries or PR lists, Claude asks what was *surprising* or *non-obvious* — that's the part worth keeping.

## When Memories Are Accessed

Claude consults memory:
- When stored facts seem relevant to the task
- When you reference prior-conversation work
- Always when you explicitly ask Claude to recall, check, or remember
- Never when you say to *ignore* or *not use* memory

## Verification Before Acting on Memory

Memories can become stale. Before recommending something based on a memory:

- **File path mentioned:** verify the file still exists
- **Function or flag mentioned:** grep for it
- **About to act on the recommendation:** verify first

"The memory says X exists" is not the same as "X exists now." For *recent* or *current* state (recent activity, current architecture), prefer `git log` and reading the code over recalling a frozen snapshot.

## Memory vs. Other Persistence

| Need | Use |
|---|---|
| Persist across sessions | **Auto memory** |
| Persist across all sessions, manually authored | `CLAUDE.md` |
| Persist within current conversation | Plans, tasks |
| Project conventions visible to teammates | `CLAUDE.md` (committed) |

If you're aligning on an implementation approach mid-conversation, that's a **plan**, not a memory. If you're tracking discrete steps in the current task, that's **tasks**. Memory is reserved for what should outlive this conversation.

## Settings

| Setting | Default | Description |
|---|---|---|
| `autoMemoryEnabled` | `true` | Enable/disable auto memory entirely |
| `autoMemoryDirectory` | `~/.claude/memory` | Custom storage location |

Manage interactively with `/memory`.

---

Official docs: https://code.claude.com/docs/en/memory.md
