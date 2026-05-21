# Custom Skills

A reference for creating and managing custom skills in Claude Code.

## What a Skill Is

A **skill** is a directory containing a `SKILL.md` file with instructions, knowledge, or workflows that extend Claude's capabilities. Skills are loaded into context on demand — automatically when Claude detects a matching request, or explicitly when you type `/<skill-name>`.

How skills differ from other extension patterns:

| Pattern | What it is | When it loads |
|---|---|---|
| **Skill** | Reusable markdown instructions + optional bundled files | On match or `/<name>` invocation |
| **Slash command** | Built-in Claude Code feature | Always available |
| **Sub-agent** | Isolated execution context | When `Agent` tool spawns it |
| **Hook** | External shell command run by the harness | Automatically, on a specific event |
| **CLAUDE.md** | Persistent project/user instructions | Every session, always in context |

## Skill File Locations

| Scope | Path | Applies to |
|---|---|---|
| **Personal** | `~/.claude/skills/<skill-name>/` | All your projects |
| **Project** | `.claude/skills/<skill-name>/` | This project only |
| **Plugin** | `<plugin>/skills/<skill-name>/` | Where plugin is enabled |

**Precedence:** project > user > plugin (when names collide).

**Monorepo:** skills load from `.claude/skills/` in the starting directory and all parents up to the repo root.

**Live reload:** edits to skill files take effect within the current session — no restart needed.

### Folder layout

```
my-skill/
├── SKILL.md           # required
├── template.md        # optional
├── reference.md       # optional
└── scripts/
    └── helper.sh      # optional
```

## The `SKILL.md` File

Two parts: optional YAML frontmatter + markdown body (the instructions).

### YAML frontmatter fields

| Field | Type | Default | Description |
|---|---|---|---|
| `name` | string | dir name | Display name (lowercase, hyphens, max 64 chars) |
| `description` | string | first paragraph | When to use this skill — Claude matches this for auto-invocation. Combined with `when_to_use`, capped at 1,536 chars |
| `when_to_use` | string | — | Additional trigger context appended to `description` in listings |
| `argument-hint` | string | — | Autocomplete hint, e.g. `[filename] [format]` |
| `arguments` | list \| string | — | Named positional args for `$name` substitution |
| `disable-model-invocation` | boolean | `false` | `true` hides from Claude — only you can invoke with `/<name>` |
| `user-invocable` | boolean | `true` | `false` hides from `/` menu — only Claude can auto-load |
| `allowed-tools` | list \| string | — | Tools pre-approved while skill is active, e.g. `Bash(git *)` `Read` |
| `model` | string | inherit | Override model (e.g. `claude-opus-4`, `inherit`) |
| `effort` | string | inherit | Override effort: `low`, `medium`, `high`, `xhigh`, `max` |
| `context` | string | — | `fork` to run in an isolated subagent context |
| `agent` | string | `general-purpose` | Subagent type when `context: fork` (e.g. `Explore`, `Plan`) |
| `paths` | list | — | Glob patterns limiting auto-activation (e.g. `src/**/*.js`) |
| `shell` | string | `bash` | Shell for `` !`command` ``: `bash` or `powershell` |

### Markdown body

The body is the instructions Claude follows once the skill loads. Keep it concise — once invoked, it stays in context across turns, so every line is a recurring token cost.

**String substitutions in the body:**

| Variable | Expands to |
|---|---|
| `$ARGUMENTS` | All arguments passed to the skill |
| `$ARGUMENTS[N]` or `$N` | Specific arg by 0-based index |
| `$name` | Named argument from `arguments` field |
| `${CLAUDE_SESSION_ID}` | Current session ID |
| `${CLAUDE_EFFORT}` | Current effort level |
| `${CLAUDE_SKILL_DIR}` | Directory containing this `SKILL.md` |

## Triggering

| Configuration | You can invoke | Claude auto-invokes | Context loaded |
|---|:---:|:---:|---|
| default | Yes | Yes | Description always; full body on use |
| `disable-model-invocation: true` | Yes | No | Not in context; loads on invoke |
| `user-invocable: false` | No | Yes | Description always; full body on use |

- **Automatic:** Claude loads the skill when your request matches `description` (+ `when_to_use`).
- **Explicit:** type `/<skill-name>` followed by optional arguments.

## Bundled Resources

Reference other files inside the skill folder from `SKILL.md` so Claude knows what to load:

```markdown
## API Reference

For complete details, see [api-reference.md](api-reference.md).

## Examples

Check [examples.md](examples.md) for usage patterns.
```

### Dynamic context injection (preprocessing)

Use `` !`<command>` `` at the start of a line to run shell commands before Claude sees the skill. Output replaces the placeholder.

```yaml
---
name: summarize-pr
description: Summarize this pull request
---

## PR Changes
!`gh pr diff`

## PR Comments
!`gh pr view --comments`

Summarize the above PR...
```

Multi-line commands use a fenced code block:

````markdown
```!
git log --oneline -5
npm test --list
```
````

**Keep `SKILL.md` under ~500 lines.** Move reference material to separate files.

## Allowed Tools

`allowed-tools` pre-approves specific tools when the skill is active so Claude doesn't prompt:

```yaml
---
name: commit
description: Stage and commit changes
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
```

- Does not *restrict* tools — your permission settings still govern baseline approval.
- Project skills' `allowed-tools` take effect after you accept workspace trust.
- To deny a tool, use deny rules in `settings.json`.

## Visibility Settings

Override visibility in `settings.json` / `settings.local.json` without editing the skill file:

```json
{
  "skillOverrides": {
    "my-internal-skill": "name-only",
    "deploy-prod": "off"
  }
}
```

| Value | Visible to Claude | Listed in `/` menu |
|---|:---:|:---:|
| `"on"` (default) | Yes | Yes |
| `"name-only"` | Name only | Yes |
| `"user-invocable-only"` | No | Yes |
| `"off"` | No | No |

Related:
- `maxSkillDescriptionChars` — cap on combined description + `when_to_use` (default `1536`)
- `skillListingBudgetFraction` — context window % for all skill descriptions (default `0.01` / 1%)

## The `/skills` Command

- View available skills
- Highlight a skill and press `Space` to cycle states, `Enter` to save to `.claude/settings.local.json`
- Invoke any skill directly with `/<skill-name>`

## Testing & Iteration

1. Edit `SKILL.md` (auto-reloads within the session)
2. Invoke with `/<skill-name>` or send a request matching the description
3. Verify behavior; iterate

## Best Practices

**Write a skill when:**
- You keep pasting the same instructions, checklist, or procedure
- You want reusable knowledge Claude applies when relevant
- You want a command-style workflow triggered with `/<name>`

**Use CLAUDE.md instead when:**
- Instructions should apply every session ("always do X")
- Project-wide conventions, build commands, style rules

**Use a hook instead when:**
- Something must happen automatically without prompting (format-on-save, lint)
- Deterministic behavior that always executes the same way

**Use a sub-agent instead when:**
- Isolating context for a large or exploratory task
- Preventing long research from bloating the main conversation
- Parallelizing independent work

**Writing effective skills:**
- Keep the body concise — say what to do, not why
- Write descriptions that match natural-language requests
- Use `disable-model-invocation: true` for side-effect tasks (deploy, send, delete)
- Move reference material to separate files; reference rather than inline

---

## Minimal Example

`~/.claude/skills/summarize-changes/SKILL.md`:

```yaml
---
description: Summarize uncommitted git changes
---

## Current Changes

!`git diff HEAD`

## Instructions

Summarize the changes above in 2-3 bullets. Flag any risks like missing error handling or hardcoded values.
```

Invoke: ask "what did I change?" (automatic) or `/summarize-changes` (explicit).

## Richer Example — Bundled Resources

```
.claude/skills/validate-api/
├── SKILL.md
├── examples.md
├── schema.md
└── scripts/
    └── validate.js
```

**`SKILL.md`:**

````yaml
---
name: validate-api
description: Validate API responses against our schema
allowed-tools: Bash(node *)
arguments: [url, method]
---

## Task

Validate the $0 endpoint ($1) against our API schema.

## Schema Reference

See [schema.md](schema.md) for the full API specification.

## Examples

[examples.md](examples.md) contains sample valid and invalid responses.

## Validation

Run the validator:

```bash
node ${CLAUDE_SKILL_DIR}/scripts/validate.js $0 $1
```

Report any schema violations found.
````

Invoke: `/validate-api https://api.example.com/users GET`

---

Official docs: https://code.claude.com/docs/en/skills.md
