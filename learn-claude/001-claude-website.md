# Claude / Anthropic Websites & Resources

A complete map of official Claude resources. Use this as your starting compass: when you need API docs, go to platform; when you want to chat, go to claude.ai; when you need to know if something is down, go to status.

## The Five Core Domains

### `anthropic.com`
The company website. Used for:
- Research papers and the interpretability / alignment work
- Blog posts, news, announcements
- Policy, safety, and responsible-scaling publications
- Careers, enterprise sales pages

Mental model: "about the company."

### `claude.com`
The Claude **product** site (separate from the chat app and the corporate site). Used for:
- Product overview and feature pages
- Pricing for all tiers (Free / Pro / Max / Team / Enterprise)
- Model overviews

Mental model: "marketing & pricing for Claude."

### `claude.ai`
The consumer chat app. End-users sign in here to:
- Chat with Claude in Projects and conversations
- Upload files, use Claude Skills
- Run the web version of Claude Code

Mental model: "where I chat with Claude."

### `platform.claude.com`
The developer platform / console. Used for:
- API keys, organizations, workspaces
- Billing and usage
- API playground
- **All API and SDK documentation** lives under `platform.claude.com/docs`

Replaced the older `console.anthropic.com`. Mental model: "where I build with Claude's API."

### `code.claude.com`
The Claude Code product site. Used for:
- Official Claude Code documentation (`code.claude.com/docs/en/...`)
- Download / install instructions, release notes

Mental model: "where I learn Claude Code."

---

## Documentation Portals

| Resource | URL |
|---|---|
| **Claude API & SDK docs** (canonical) | https://platform.claude.com/docs |
| **Claude Code docs** | https://code.claude.com/docs |

`docs.claude.com` and `docs.anthropic.com` both 301-redirect to `platform.claude.com/docs`. API reference, Agent SDK docs, model docs all live under that root.

## Official GitHub Repos

| Repo | What's there |
|---|---|
| [anthropics/anthropic-cookbook](https://github.com/anthropics/anthropic-cookbook) | 43k-star collection of Jupyter notebooks and recipes for using Claude |
| [anthropics/claude-code](https://github.com/anthropics/claude-code) | Official Claude Code repository (125k stars) — issues, releases |
| [anthropics/anthropic-sdk-python](https://github.com/anthropics/anthropic-sdk-python) | Python SDK for the Claude API with examples |
| [anthropics/anthropic-sdk-typescript](https://github.com/anthropics/anthropic-sdk-typescript) | TypeScript / Node.js SDK |
| [anthropics/courses](https://github.com/anthropics/courses) | Five progressive courses — API fundamentals through tool use |

## Status, Trust & Operations

| Resource | URL | Use it when |
|---|---|---|
| **Service status** | https://status.claude.com | Something seems broken — check here before assuming it's your bug |
| **Trust center** | https://trust.anthropic.com | You need security / compliance / data handling info |

## Pricing & Plans

| Audience | URL |
|---|---|
| All Claude pricing tiers (consumer + business) | https://claude.com/pricing |

## Research, News, Models

| Resource | URL |
|---|---|
| Anthropic newsroom & announcements | https://anthropic.com/news |
| Anthropic research (interpretability, alignment, safety) | https://anthropic.com/research |

Model launches (Opus / Sonnet / Haiku version drops) are published on the newsroom.

## Community & Learning

- **Discord** — invite linked from official Anthropic docs (community discussion, beta announcements)
- **GitHub courses** ([anthropics/courses](https://github.com/anthropics/courses)) — closest thing to an "Anthropic Academy"; no standalone academy site exists yet
- **GitHub issues** — feedback and bug reports for [claude-code](https://github.com/anthropics/claude-code/issues) and the SDKs

---

## Quick-Lookup Table

| I want to... | Go here |
|---|---|
| Chat with Claude | claude.ai |
| Get an API key / build with the API | platform.claude.com |
| Read API or SDK docs | platform.claude.com/docs |
| Read Claude Code docs | code.claude.com/docs |
| See pricing | claude.com/pricing |
| Check if Claude is down | status.claude.com |
| Find code examples / recipes | github.com/anthropics/anthropic-cookbook |
| Learn the API from scratch | github.com/anthropics/courses |
| Report a Claude Code bug | github.com/anthropics/claude-code/issues |
| Read research papers | anthropic.com/research |
| Read announcements / blog | anthropic.com/news |
| Get security & compliance info | trust.anthropic.com |

---

## Naming Gotchas

A few things that trip people up:

- **`claude.com` vs `claude.ai`** — `claude.com` is marketing/pricing; `claude.ai` is the chat app. Easy to mix up.
- **`platform.claude.com` vs `console.anthropic.com`** — same thing; the console moved domains. Bookmarks to the old URL still redirect.
- **`docs.claude.com` vs `docs.anthropic.com`** — both redirect to `platform.claude.com/docs`. Use any of the three; you'll land at the same canonical place.
- **`status.claude.com` (not `status.anthropic.com`)** — service status lives on the Claude domain, not the Anthropic one.
- **Claude Code docs** are *separate* from the rest of the docs — they're at `code.claude.com/docs`, not under `platform.claude.com/docs`.
