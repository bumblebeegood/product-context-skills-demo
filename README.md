# Structured Product Context skills (DEMO repo with context populated)

A Claude Code workspace that turns a product brief into **structured product context**, all skills are building files from structured context.


## Requirements

- [Claude Code](https://claude.com/claude-code) or [Codex](https://openai.com/codex/) (CLI, desktop, or IDE extension) or [GitHub Copilot](https://github.com/features/copilot)
- Optional: the Atlassian connector for `backlog` (Jira) and `confluence-sync`; `pandoc` and `jq` for `confluence-sync` API mode

## Quick Start

1. Clone the repo and open it in Claude Code.
2. Put your product brief in `INPUTS/` — any Markdown file describing the product, its users, use cases, and data.
3. Build the context. Choose a mode:

   ```
   /extract-full --grill    # Claude interviews you, file by file, and writes only what you confirm
   /extract-full --assume   # Claude fills the gaps with plausible values and writes everything in one go
   ```

   `--assume` lists every invented value in `OUTPUT/extract-assumptions.md` so you can review it.
4. Review the result: `product-context/`, `users/`, `domain-entities/`, `Business-rules.md`, `workflows/`, `business-processes/`.

## Workspace Layout

| Path | Holds |
|---|---|
| `product-context/` | What the product is, problems, benefits, roadmap |
| `users/` | One file per user group: JTBDs and RBAC |
| `domain-entities/` | One file per entity: attributes, types, validations, error messages, examples |
| `Business-rules.md` | `If …, then …` rules, grouped by topic, with stable codes (`BR-ORDER-01`) |
| `workflows/` | Single-role, step-by-step operations |
| `business-processes/` | End-to-end flows across roles |
| `INPUTS/` | Raw documents waiting to be processed |
| `Archive/` | Processed inputs (read-only) |
| `OUTPUT/` | Drafts, API stories, specs, ticket backlog |
| `_master-index.md`, `*/_index.md` | Indexes the agents read first |
| `log.md` | One line per change |

The rules every agent follows are in [AGENTS.md](AGENTS.md).

## Skills

| Skill | What it does |
|---|---|
| `extract-full` | Bootstraps an empty workspace from a brief (`--grill` or `--assume`) |
| `context-management` | Adds new inputs to existing context, and owns the file structures |
| `ears-requirements` | Rewrites requirements in EARS form |
| `draft-spec` | Drafts tickets and user stories |
| `draft-api-story` | Drafts API design stories from the workspace context (`--bulk` for many) |
| `backlog` | Maintains `OUTPUT/ticket-backlog.md` and creates Jira issues |
| `confluence-sync` | Syncs Markdown files with Confluence pages |
