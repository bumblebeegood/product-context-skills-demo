# Structured Product Context skills (DEMO repo with context populated)

A workspace that turns a product brief into **structured product context**, all skills are building files from structured context.

## Requirements

- [Claude Code](https://claude.com/claude-code) or [Codex](https://openai.com/codex/) (CLI, desktop, or IDE extension) or [GitHub Copilot](https://github.com/features/copilot)
- Optional: the Atlassian connector for `backlog` (Jira) and `confluence-sync`; `pandoc` and `jq` for `confluence-sync` API mode

## Quick Start

1. Clone the repo and open it in Claude Code or other LLM provider of your choice.
2. Put your product brief in `INPUTS/` — any Markdown file describing the product, its users, use cases, and data, for example, a Product Requirements Document.
3. Build the context. Choose a mode:
   ```
   /extract-full --grill    # AI interviews you, file by file, and writes only what you confirm
   /extract-full --assume   # AI fills the gaps with plausible values and writes everything in one go
   ```

   `--assume` lists every invented value in `OUTPUT/extract-assumptions.md` so you can review it.
4. Review the result: `product-context/`, `users/`, `domain-entities/`, `Business-rules.md`, `workflows/`, `business-processes/`.

## Skills

Skills are the way you work with this workspace. Each one is a set of instructions that AI runs as a slash command (for example `/extract-full`), so every step has a defined input, a defined output and a fixed file structure. They fall into three groups: skills that **build the context** from your raw documents, skills that **turn the context into deliverables** (tickets, API stories, requirements), and skills that **connect the workspace to other tools** (Jira, Confluence). 

### Typical use:
 `extract-full`  for empty workspace. 
 `context-management` as new inputs arrive. 
 `draft-spec` when your are ready to produce a specification from your context files.

| Skill | What it does |
|---|---|
| `extract-full` | Bootstraps an empty workspace from a product brief in `INPUTS/` or a pasted description. It produces the Product Description, user groups with JTBDs, domain entities, business rules, workflows and business processes in one pass. `--grill` interviews you file by file and writes only what you confirm; `--assume` fills every gap with plausible values and logs them in `OUTPUT/extract-assumptions.md`. Use it once, when the workspace is empty. |
| `context-management` | Keeps the context current after the first build. Give it a new document or a prompt, and it classifies each piece (business rule, JTBD, workflow, data example, business process, product description), writes it to the right file, and updates the indexes and `log.md`. It also owns the file structures the other skills rely on. |
| `ears-requirements` | Rewrites loose requirements, feature descriptions or tickets into a numbered list in [EARS](https://en.wikipedia.org/wiki/Easy_Approach_to_Requirements_Syntax) form. It reads the context first so the system name, actors and entities match your domain, then writes the list to `outputs/`. |
| `draft-spec` | Drafts a ticket, user story or feature spec as a structured Markdown file in `OUTPUT/Draft/`, with user flows and acceptance criteria. It asks clarifying questions first and checks the backlog for an existing entry on the same topic. |
| `draft-api-story` | Drafts API design stories from a JTBD, a workflow or an EARS requirement. Each story names one operation (method and resource), links the entities it touches, lists errors and edge cases, and records open design questions for the product manager to review. `--bulk` plans and writes one story per operation for a whole user group. Stories go to `OUTPUT/Draft/api-stories/`. Skill comes with bundled API standards by [Emmanuel Paraskakis](https://github.com/paraskakis/skills/blob/main/api/design-api-spec/references/examples/API-standards.md). Replace with your ownn before composing a story.  |
| `backlog` | Maintains `OUTPUT/ticket-backlog.md`, the single list of ticket candidates and their status (drafted, linked to Jira). It adds entries, updates notes and dependencies, and creates Jira issues from rows through the Atlassian connector, with a manual fallback. |
| `confluence-sync` | Syncs Markdown files in the workspace with Confluence pages, in either direction, through the Atlassian connector or the Confluence API. It diffs content before it pushes or pulls and records each sync in `log.md`. Run it manually with `/confluence-sync`. |

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
