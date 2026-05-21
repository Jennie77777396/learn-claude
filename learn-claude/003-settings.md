# Claude Code `settings.json` Reference

A reference of configuration keys for Claude Code's `settings.json` files.

## File Locations & Precedence

### Standard Paths

| Scope | macOS / Linux | Windows |
|---|---|---|
| Managed | `/etc/claude/managed-settings.json` (or plist) | `%ProgramData%\Claude\managed-settings.json` (or registry) |
| User | `~/.claude/settings.json` | `%APPDATA%\.claude\settings.json` |
| Project (shared) | `.claude/settings.json` (git-committed) | `.claude/settings.json` |
| Project (local) | `.claude/settings.local.json` (gitignored) | `.claude/settings.local.json` |

### Resolution Order (highest priority first)

1. **Managed** — server/policy/registry, cannot be overridden
2. Command-line arguments
3. **Local** — `.claude/settings.local.json`
4. **Project** — `.claude/settings.json`
5. **User** — `~/.claude/settings.json`

---

## Model & Performance

| Key | Type | Default | Description |
|---|---|---|---|
| `model` | string | — | Override default Claude model (use `/model` to switch mid-session) |
| `modelOverrides` | object | — | Map Anthropic model IDs to provider-specific IDs (Bedrock ARNs, Vertex IDs) |
| `availableModels` | array | — | Restrict models available via `/model` and `--model` |
| `effortLevel` | `"low"` \| `"medium"` \| `"high"` \| `"xhigh"` | — | Reasoning depth; persists across sessions |
| `alwaysThinkingEnabled` | boolean | `false` | Enable extended thinking mode by default |
| `showThinkingSummaries` | boolean | `false` | Show extended thinking summaries in interactive mode |

## User Interface

| Key | Type | Default | Description |
|---|---|---|---|
| `editorMode` | `"normal"` \| `"vim"` | `"normal"` | Keybinding mode |
| `tui` | `"default"` \| `"fullscreen"` | `"default"` | Terminal rendering mode |
| `spinnerTipsEnabled` | boolean | `true` | Show tips/facts while Claude works |
| `spinnerTipsOverride` | object | — | `{"tips": [...], "excludeDefault": boolean}` |
| `spinnerVerbs` | object | — | `{"mode": "append"\|"replace", "verbs": [...]}` |
| `prefersReducedMotion` | boolean | `false` | Reduce or disable animations |
| `outputStyle` | string | — | Adjust system prompt (requires restart or `/clear`) |
| `viewMode` | `"default"` \| `"verbose"` \| `"focus"` | `"default"` | Output verbosity |
| `autoScrollEnabled` | boolean | `true` | Auto-scroll output in fullscreen mode |
| `showTurnDuration` | boolean | `true` | Show "Cooked for Xm Ys" timing |
| `terminalProgressBarEnabled` | boolean | `true` | Progress bar in compatible terminals |
| `language` | string | — | UI language, e.g. `"japanese"`, `"spanish"` |
| `syntaxHighlightingDisabled` | boolean | `false` | Disable syntax highlighting in diffs/code |

## Permissions & Security

| Key | Type | Description |
|---|---|---|
| `permissions.allow` | array | Auto-approve rules (e.g. `"Bash(npm run lint)"`) |
| `permissions.ask` | array | Rules requiring user confirmation |
| `permissions.deny` | array | Block rules (e.g. `"Read(./.env)"`) |
| `permissions.additionalDirectories` | array | Extra working directories for file access |
| `permissions.defaultMode` | `"default"` \| `"acceptEdits"` \| `"plan"` \| `"auto"` \| `"dontAsk"` \| `"bypassPermissions"` | Default permission handling mode |
| `permissions.disableBypassPermissionsMode` | `"disable"` | Prevent bypass mode activation |
| `permissions.skipDangerousModePermissionPrompt` | boolean | Skip confirmation before bypass mode |
| `allowedHttpHookUrls` | array | Allowlist of HTTP hook URLs (supports `*`) |
| `httpHookAllowedEnvVars` | array | Env vars HTTP hooks may access |
| `allowManagedHooksOnly` | boolean | **(Managed)** Block user/project hooks |
| `allowManagedPermissionRulesOnly` | boolean | **(Managed)** Only managed permission rules apply |

## File & Memory

| Key | Type | Default | Description |
|---|---|---|---|
| `autoMemoryEnabled` | boolean | `true` | Enable auto memory across sessions |
| `autoMemoryDirectory` | string | `~/.claude/memory` | Storage location for auto memory |
| `claudeMd` | string | — | **(Managed)** Organization-wide CLAUDE.md instructions |
| `claudeMdExcludes` | array | — | Glob patterns of CLAUDE.md files to skip |
| `respectGitignore` | boolean | `true` | Respect `.gitignore` in `@` file picker |
| `plansDirectory` | string | `~/.claude/plans` | Where plan files are stored |
| `cleanupPeriodDays` | number | `30` | Delete session files older than threshold |

## Tools & Integrations

| Key | Type | Description |
|---|---|---|
| `allowedMcpServers` | array | **(Managed)** Allowlist of permitted MCP servers |
| `deniedMcpServers` | array | Denylist of blocked MCP servers |
| `allowManagedMcpServersOnly` | boolean | **(Managed)** Only admin-defined MCP servers apply |
| `enableAllProjectMcpServers` | boolean | Auto-approve all project `.mcp.json` servers |
| `enabledMcpjsonServers` | array | Approve specific servers from `.mcp.json` |
| `disabledMcpjsonServers` | array | Reject specific servers from `.mcp.json` |
| `skipWebFetchPreflight` | boolean | Skip domain safety check (Bedrock/Vertex/Foundry) |

## Plugins & Marketplaces

| Key | Type | Description |
|---|---|---|
| `allowedChannelPlugins` | array | **(Managed)** Allowlist of channel plugins |
| `strictKnownMarketplaces` | array | **(Managed)** Allowlist of plugin marketplaces |
| `blockedMarketplaces` | array | **(Managed)** Blocklist of marketplaces |
| `pluginTrustMessage` | string | Custom message appended to plugin trust warning |

## Authentication & APIs

| Key | Type | Description |
|---|---|---|
| `apiKeyHelper` | string | Script generating auth headers (`X-Api-Key`, `Authorization: Bearer`) |
| `forceLoginMethod` | `"claudeai"` \| `"console"` | Restrict login type |
| `forceLoginOrgUUID` | string \| array | Require login to specific org UUID(s) |
| `forceRemoteSettingsRefresh` | boolean | **(Managed)** Block startup until remote settings fetch succeeds |

## Cloud & Credentials

| Key | Type | Description |
|---|---|---|
| `awsAuthRefresh` | string | Script to refresh AWS credentials when expired |
| `awsCredentialExport` | string | Script outputting JSON with AWS credentials |
| `gcpAuthRefresh` | string | Script to refresh GCP Application Default Credentials |
| `otelHeadersHelper` | string | Script generating dynamic OpenTelemetry headers |

## Git & Attribution

| Key | Type | Default | Description |
|---|---|---|---|
| `attribution` | object | — | `{"commit": "text", "pr": "text"}` |
| `includeGitInstructions` | boolean | `true` | Include built-in git workflow instructions |
| `prUrlTemplate` | string | — | PR badge URL template (`{host}`, `{owner}`, `{repo}`, `{number}`) |

## Environment & Updates

| Key | Type | Description |
|---|---|---|
| `env` | object | Environment variables applied to all sessions |
| `autoUpdatesChannel` | `"stable"` \| `"latest"` | Update channel — stable (weekly) or latest |
| `minimumVersion` | string | Floor version for auto-updates (e.g. `"2.1.100"`) |
| `disableDeepLinkRegistration` | `"disable"` | Prevent `claude-cli://` protocol handler registration |
| `disableRemoteControl` | boolean | Disable Remote Control feature (v2.1.128+) |

## Organization & Management

| Key | Type | Description |
|---|---|---|
| `companyAnnouncements` | array | Announcements displayed at startup (random selection) |
| `channelsEnabled` | boolean | **(Managed)** Allow channels for organization |
| `parentSettingsBehavior` | `"first-wins"` \| `"merge"` | **(Managed, v2.1.133+)** Parent-supplied settings interaction |
| `policyHelper` | object | **(Managed, v2.1.136+)** `{"path": "/usr/local/bin/claude-policy"}` |

## Specialized Features

| Key | Type | Default | Description |
|---|---|---|---|
| `agent` | string | — | Run main thread as named subagent |
| `awaySummaryEnabled` | boolean | `true` | Show session recap when returning after minutes away |
| `autoMode` | object | — | `{"environment": [...], "allow": [...], "soft_deny": [...], "hard_deny": [...], "include": ["$defaults"]}` |
| `disableAutoMode` | `"disable"` | — | Prevent auto mode activation |
| `autoConnectIde` | boolean | `false` | Auto-connect to IDE from external terminal |
| `autoInstallIdeExtension` | boolean | `true` | Auto-install Claude Code IDE extension |
| `externalEditorContext` | boolean | `false` | Prepend last response as comment in external editor |
| `fastModePerSessionOptIn` | boolean | `false` | Require per-session opt-in for fast mode |
| `feedbackSurveyRate` | number | — | Probability (0–1) of showing session quality survey |
| `fileSuggestion` | object | — | Custom `@` file autocomplete script |
| `statusLine` | object | — | Custom status line command |
| `defaultShell` | `"bash"` \| `"powershell"` | `"bash"` | Default shell for `!` commands |
| `preferredNotifChannel` | string | `"auto"` | `auto`, `terminal_bell`, `iterm2`, `iterm2_with_bell`, `kitty`, `ghostty`, `notifications_disabled` |
| `showClearContextOnPlanAccept` | boolean | `false` | Show "clear context" option on plan accept |
| `useAutoModeDuringPlan` | boolean | `true` | Use auto mode semantics in plan mode |
| `disableSkillShellExecution` | string | — | Disable inline shell execution in skills |
| `disableAllHooks` | boolean | `false` | Disable all hooks and custom status line |
| `disableAgentView` | boolean | `false` | Turn off background agents and agent view |
| `maxSkillDescriptionChars` | number | `1536` | Per-skill character cap on listing (v2.1.105+) |
| `skillListingBudgetFraction` | number | `0.01` | Context fraction for skill listing (v2.1.105+) |
| `skillOverrides` | object | — | Per-skill visibility: `"on"`, `"name-only"`, `"user-invocable-only"`, `"off"` (v2.1.129+) |
| `teammate` | `"in-process"` \| `"tmux"` \| `"auto"` | — | Agent team display mode |
| `voice` | object | — | `{"enabled": true, "mode": "hold"\|"tap", "autoSubmit": true}` |
| `voiceEnabled` | boolean | `false` | **(Legacy)** Use `voice.enabled` instead |

## Worktree

```json
{
  "worktree": {
    "baseRef": "fresh",
    "symlinkDirectories": ["node_modules"],
    "sparsePaths": ["packages/my-app"],
    "bgIsolation": "worktree"
  }
}
```

| Key | Values |
|---|---|
| `baseRef` | `"fresh"` (origin/default) or `"head"` (local HEAD) |
| `symlinkDirectories` | Array of directory names to symlink |
| `sparsePaths` | Array of sparse-checkout paths |
| `bgIsolation` | `"worktree"` or `"none"` |

## Sandbox

```json
{
  "sandbox": {
    "enabled": false,
    "failIfUnavailable": false,
    "autoAllowBashIfSandboxed": true,
    "excludedCommands": ["docker *"],
    "allowUnsandboxedCommands": true,
    "filesystem": {
      "allowWrite": ["/tmp/build"],
      "denyWrite": ["/etc"],
      "denyRead": ["~/.aws/credentials"],
      "allowRead": ["."],
      "allowManagedReadPathsOnly": false
    },
    "network": {
      "allowUnixSockets": [],
      "allowAllUnixSockets": false,
      "allowLocalBinding": false,
      "allowMachLookup": [],
      "allowedDomains": ["github.com"],
      "deniedDomains": [],
      "allowManagedDomainsOnly": false,
      "httpProxyPort": 8080,
      "socksProxyPort": 8081
    },
    "bwrapPath": "/usr/bin/bwrap",
    "socatPath": "/usr/bin/socat"
  }
}
```

## Hooks

Hooks reload without restart when modified.

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "Bash|Edit",
        "hooks": [
          {
            "type": "command",
            "timeout": 600,
            "statusMessage": "Optional spinner text"
          }
        ]
      }
    ]
  }
}
```

See [hooks.md](./hooks.md) for the full list of hook events.

---

## Reload Behavior

**Reloads without restart:**
- `permissions`, `hooks`, `apiKeyHelper`, credential helpers
- Triggers `ConfigChange` hook on update

**Requires restart or `/clear`:**
- `model` (use `/model` to switch mid-session instead)
- `outputStyle` (rebuilds system prompt)

---

## Minimal Example

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "claude-sonnet-4-6",
  "effortLevel": "high",
  "editorMode": "vim",
  "permissions": {
    "allow": ["Bash(npm run *)"],
    "deny": ["Bash(curl *)", "Read(./.env*)"]
  },
  "env": {
    "NODE_ENV": "development"
  }
}
```

---

## Notes

- **Managed settings** (marked **(Managed)**) apply only in `managed-settings.json` and cannot be overridden by user/project settings.
- Use `/update-config` skill to modify `settings.json` or add hooks.
- Use `/keybindings-help` skill to customize shortcuts in `~/.claude/keybindings.json`.
- `env` variables apply to all Claude Code sessions.
- Permission rules support glob wildcards (e.g. `Bash(npm run *)`, `Read(src/**/test.ts)`).
- Official docs: https://code.claude.com/docs/en/settings.md
