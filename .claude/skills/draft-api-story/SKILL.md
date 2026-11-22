---
name: draft-api-story
description: 'Draft one API design story from the existing Product Context: a JTBD from users/, a workflow, or an EARS requirement, grounded in domain-entities/, Business-rules.md and the API standards. Produces a structured story .md in OUTPUT/Draft/api-stories/. With --bulk, produces one file per story. Use when the user says "/draft-api-story" or asks to write API story / stories from a JTBD or EARS requirement. Does not write the OpenAPI spec (that is `design-api-spec`).'
argument-hint: 'A JTBD, workflow or EARS requirement to turn into an API story (e.g. "Rules Manager creates device type"), or --bulk <users/rules-manager.md | outputs/<ears-file>.md>'
license: MIT
metadata:
  version: "1.1.0"
  author: "Illia Shapoval"
  adapted-from: "https://github.com/paraskakis/skills/tree/main/api/design-api-stories by Emmanuel Paraskakis"
---

# Draft API Story

Produces an API design story `.md` file in `OUTPUT/Draft/api-stories/`, using the Product Context as input. A story is the bridge between a product requirement and the OpenAPI spec (Emmanuel Paraskakis's method for designing APIs with LLMs). It is written for a product manager to review: "yes, that is the right API" or "no, we are missing X". 

This skill is the workspace-aware version of `design-api-story`. Entities, users, workflows, rules and product context come from the workspace, not from files the user hands over. The clarify step and the template are shared with the `draft-spec` skill.

## Scope and Routing

- In scope: turning one requirement (or, with `--bulk`, a list of requirements) plus the Product Context plus API standards into reviewable API stories.
- Out of scope: ticket drafts (use `draft-spec`), EARS rewriting (use `ears-requirements`), generating the OpenAPI spec (use `design-api-spec`), and changing Product Context files (use `context-management`).

Use **after** the Product Context has the relevant entries. If context is missing, run `context-management` first or ask the user to provide it.

## Input: what a story is written from

One story is written from **one requirement**. The requirement is one of:

1. **A JTBD** from `users/<group>.md` (named by group and number, or quoted).
2. **An EARS requirement**, or a requirement file produced by `ears-requirements` (`outputs/<slug>.md`, or pasted lines).
3. **A plain request** from the user (e.g. "story for creating a device type"), grounded in the matching Workflow in `workflows/`.

Without `--bulk`: one story. If the source holds several requirements (a whole `users/<group>.md`, an EARS list), ask which one to use, or suggest `--bulk`.

## `--bulk`

`/draft-api-story --bulk <source>` creates multiple stories, **one file per story**. `<source>` is one of:

- A `users/<group>.md` file: every JTBD in it.
- A list of JTBDs, by number or quote.
- An `ears-requirements` output file: every line of the `EARS-compliant list`.

Bulk procedure:

1. Run **Step 0 — Clarify** once for the whole batch, not once per story.
2. Derive the stories. One story is one operation with one action verb. A JTBD or requirement that needs several operations (e.g. "have a current catalog of device types" → create, update, deactivate, list) becomes several stories. A requirement with no API operation (pure UI behaviour, a business rule that no consumer calls) is skipped with a reason.
3. **Present the plan before writing anything** and wait for one confirmation:

   | # | Story title | Source requirement | Method + Resource |
   |---|---|---|---|

   Also list the skipped requirements with the reason, and any decision that applies to the whole batch (pagination style, error format, deactivate as `PUT` or `DELETE`). Decide it once and apply it identically to every story.
4. After confirmation, run Steps 1–3 for every story in the plan. Each file is self-contained.
5. Write **one log line** for the batch (Step 3).

---

## Procedure

### Step 0 — Clarify

Only proceed once the task is clarified.

1. Read `_master-index.md`.
2. Read `OUTPUT/ticket-backlog.md`. Match the request against existing rows by title similarity or subject.
  - If matched: tell the user and use the row's Notes and Dependency as additional context.
  - If the matched row's `Dependency` is non-empty: state the open decision and ask whether it is resolved or should be treated as a known open item.
  - If the matched row's `Draft Created` is non-empty: tell the user a ticket draft exists, read it as extra context, and carry on. The backlog is **read-only** for this skill.
3. Look up the relevant Workflows, JTBDs, Domain Entities, Business Rules, and Business Processes by reading the matching files:
  - `users/<group>.md` — the JTBD and the RBAC table (which role may perform the operation).
  - `workflows/` — the steps the operation serves.
  - `domain-entities/` — attributes, types, validations and error messages of every entity the operation touches.
  - `Business-rules.md` — rules that constrain the operation (cite by code).
  - `product-context/` — scope (v1 limits such as US only, USD) and non-functional targets that affect the API.
4. **Locate the API standards.** Use, in this order:
  1. A file the user names or pastes.
  2. A file in the workspace with a `# API Standards` or `# Style Guide` heading.
  3. [references/API-standards.md](references/API-standards.md) — the default, bundled with this skill. Read it in full before composing any story.

  Tell the user which file you will use and confirm it once per session. If none exists, ask the user. Never write a story without standards: method, resource, nesting, pagination and error codes all come from them.
5. Break down the request:
  - **Actor** — the role from `users/` that performs the operation.
  - **Goal** — scope and purpose; the related JTBD / Workflow.
  - **Operation** — the single action verb and the entity it acts on; the resulting `Method` and `Resource` per the standards.
  - **Parameters** — path and query parameters, and the request-body attributes taken from the entity.
  - **Edge cases** — suggested from context: business rules, entity validations, RBAC.
  - **Out of scope** — only if the user explicitly mentions; do not infer.
6. **Surface terminology conflicts and gaps.** If the user introduces terms that differ from the Product Context, or an entity or attribute the operation needs is not in `domain-entities/`, ask before drafting. Never invent an entity.
7. User story for API must focus on the user (other system or actual user) outcomes, not simply CRUD operations. If outcomes are not clear from gathered context - clarify with the user.

### Step 1 — Compose the story

Build the file using the **Story Template** below. All sections are produced in order. If a section has no input data (besides Out of Scope, which is dropped when nothing applies), write `TBD` rather than skipping the heading.

### Step 2 — Place the story

1. Write to `OUTPUT/Draft/api-stories/<title>.md`. File name = the story title.
2. Do not wait for confirmation before placing a single story — the user reviews after it is written. (Bulk waits once, for the plan.)
3. If a file with that name exists, ask whether to overwrite, keep both with a numeric suffix, or skip. Never overwrite silently.
4. Do not write to `OUTPUT/ticket-backlog.md`.

### Step 3 — Log

Append one line to `log.md`:

```
YYYY-MM-DD | <source file or "prompt"> | Drafted API story "<title>" in OUTPUT/Draft/api-stories/<title>.md.
```

For `--bulk`:

```
YYYY-MM-DD | <source file or "prompt"> | Drafted <N> API stories in OUTPUT/Draft/api-stories/ from <source>.
```

### Step 4 — Suggest next steps

After placing the story or stories, suggest, naming the target file and the skill that handles it:

- Missing or `TBD` entity attributes, workflows, JTBDs or business rules the story implies (`context-management`).
- Backlog candidates implied by Out of Scope (`backlog`).
- Once the set of stories for an API is complete, generating the OpenAPI spec (`design-api-spec`).

Do not act on suggestions without explicit user instruction.

---

## Story Template

The file has no frontmatter. The first line of the body is the H1 = the title.

### 1. Title

```
# [action] [entity] [by user] [context]
```

Only `[action]` and `[entity]` are required. `[user]` and `[context]` are optional.

Rules:

- **One action verb only.** "Create and update" is two stories — split and surface to the user.
- `[action]` is a single imperative verb (e.g. *Create*, *Get*, *List*, *Search*, *Update*, *Deactivate*).
- `[entity]` matches a `domain-entities/` file (singular for one item, plural for a collection).
- `[by user]` is a role from `users/`. Add it when the role disambiguates the story.
- `[context]` is used only for disambiguation.

Examples: `Create device type`, `List orders by Operator`, `Update quote amount by Operator after assessment`.

Title is also the file name (`OUTPUT/Draft/api-stories/<title>.md`).

---

### 2. Scope

```markdown
## Scope

The system shall allow [user] to [action] [entity] [context or conditions].

- **Method:** `POST`
- **Resource:** `/device-types`
- **Source:** `users/rules-manager.md` JTBD 1
```

Or, when one action needs several conditions:

```markdown
## Scope

The system shall allow [user] to [action] [entity] when:
- <condition 1>
- <condition 2>

- **Method:** …
- **Resource:** …
- **Source:** …
```

Every Scope ends with the bullet list. Besides `Method`, `Resource` and `Source`, add these two bullets when they apply:

```markdown
- **Access:** Public read access without authentication (BR-ACC-01)
- **Response behavior:** Return the speaker profile and the speaker's talks ordered by start time ascending (BR-PRG-01, BR-PRG-05). Include cancelled talks with `Status` shown (BR-PRG-02).
```

- `Access` — who may call the operation and whether authentication is required, from the RBAC table in `users/` and the Business Rules. Cite the rule code.
- `Response behavior` — for a `GET` or a list: what is returned and how it is ordered, filtered or included, from the Business Rules. Cite the rule codes. Omit for operations with no such rule.

Rules:

- Use "The system shall …" phrasing. State the goal, not steps or validations.
- `Method` and `Resource` MUST follow the API standards (naming, nesting, pluralization, which method for which action). Use `domain-entities/` names for the resource, in the casing the standards require.
- `Source` names the requirement the story came from: `users/<group>.md` + JTBD number, the `outputs/<slug>.md` EARS file + line number, or `prompt`. It is the traceability link.
- Nest resources per the entity relations, within the nesting limit of the standards.

---

### 3. Entity Attributes & Validations

The entity files in `domain-entities/` are the single source of truth for attributes. **Link to them; do not copy their attribute tables.** The section has an intro sentence, one bullet per entity touched, and a `Parameters` sub-section when the operation has path or query parameters that are not entity attributes.

```markdown
## Entity Attributes & Validations

The response uses the canonical definitions and validations in these entity files; their attribute names, types, examples, and error messages are not repeated here:

- [Speaker entity](../../../domain-entities/speaker.md) — return the speaker profile attributes.
- [Talk entity](../../../domain-entities/talk.md) — return the talk attributes needed to identify and schedule the presentations, including `Last updated`.

### Parameters

| parameter | in | type | mandatory | description |
|---|---|---|---|---|
| <name> | path / query | <type> | <Y/N> | <what it represents> |
```

Rules:

- One bullet per entity the operation touches: a link to `domain-entities/<entity>.md` (relative path `../../../domain-entities/<entity>.md` from the story file), then a short note saying **which part of the entity this operation uses**: the request-body attributes for `POST` / `PUT`, the filter attributes for a list or search, the attributes returned for a `GET`.
- Do not reproduce attribute names, types, validations, error messages or examples in the story. The conversion to wire names happens in `design-api-spec`, which reads the entity file.
- Write in the note only what is specific to this operation and not in the entity file: which attributes are in or out, which are system-generated or not accepted in the request (`Set by the system; not accepted in the request`), immutability, and decimal precision for currency / numeric fields when the entity file does not state it.
- If the operation needs an attribute or validation the entity file lacks, do not add it to the story. Flag it in Inferred Suggestions and suggest `context-management`.
- `Parameters` are written out in full, because they are not in any entity file. They follow the standards exactly (e.g. pagination parameters for every collection). Mark each as `path` or `query`. Do not include a parameter for an ID the entity already defines, other than the path ID.
- Omit the `Parameters` sub-section when there are none.

---

### 4. Error / Edge Cases

```markdown
## Error / Edge Cases

| Scenario | System Behavior | Message Example | Suggested Exit Flow |
|---|---|---|---|
| <what goes wrong> | <how the system handles it, with the HTTP status per the standards> | "<exact error text>" | <how the API consumer recovers> |
```

Cover the categories that apply:

- Validation errors (missing mandatory attributes, invalid formats, values outside an enum).
- Duplicate data and state conflicts.
- Resource not found.
- Permissions (the role is not allowed, per the RBAC table in `users/`).
- Business-rule violations (cite the rule code).
- Boundary conditions (zero, maximum, empty collection).
- Concurrent access by multiple users.
- Mid-flow data changes (referenced data becomes stale or unavailable).

Skip categories that genuinely don't apply. Do not list generic 429 and 500 responses — `design-api-spec` adds them to every operation. Take the message text from the entity's error messages when one exists.

---

### 5. Out of Scope

```markdown
## Out of Scope

- <Item explicitly excluded by the user>
- <Item explicitly excluded by the user>
```

Rules:

- **Only items the user mentioned explicitly.** Do not infer exclusions.
- **Omit the entire section if there is nothing to list** — this is the only section that may be silently dropped.
- Each item is a future-story candidate; surface it to the user as input to the `backlog` skill after the story is placed.

---

### 6. Consumer Considerations

```markdown
## Consumer Considerations

- Omit optional attributes when they have no value; do not emit them as `null`.
- Consumers must tolerate unknown enum values and additive response fields.
- Consumers should use each talk's `Last updated` value to recognize potentially stale schedule data.
- Consumers should handle standard `406`, `429`, and `5xx` responses according to the API standards, including `Retry-After` where supplied.
```

What an API consumer needs to know to integrate safely, beyond the happy path. It is addressed to the consumer, so each bullet is a behaviour the consumer must expect or handle.

Rules:

- Bullets only, each one stating an exact behaviour. No inferred behaviour: every bullet traces to the API standards, the entity files or a Business Rule.
- Cover the categories that apply: optional and omitted attributes, forward compatibility (unknown enum values, additive fields), staleness and caching signals (`Last updated`, `ETag`), external links that may be unavailable, retry and rate-limit handling.
- The generic `406`, `429` and `5xx` handling is mentioned here once, as a consumer duty. Do not add those statuses to Error / Edge Cases.
- Write `TBD` if the standards and context give nothing to say. Do not drop the heading.

---

### 7. Open Design Questions

```markdown
## Open Design Questions

- Should all of a speaker's talks be embedded in this response, or exposed through a separately paginated `GET /speakers/{speakerId}/talks` collection?
- Should this read endpoint support `ETag` and conditional GET for consumers that cache speaker profiles?
```

The design decisions the PM or API designer must make before the story is accepted. The context and the standards leave them open; the story does not assume an answer.

Rules:

- Every item is phrased as a question, ends with `?`, and names the concrete options. If the reasoning is not obvious from the question, add it after the question in one clause.
- Never put an unconfirmed decision in Scope or in any other section.
- Do not drop the heading. If there are no open questions, write `None.`

Categories to consider:

- A `Method` or `Resource` the standards do not settle (e.g. deactivate as `PUT` of a status or as `DELETE`).
- Embedded vs separate sub-resource, full vs summary representation.
- Filters, sort order, default values, retention windows.
- Idempotency, optimistic concurrency, `ETag` and conditional requests.

---

### 8. Inferred Suggestions

```markdown
## Inferred Suggestions

> The following items are inferred or proposed — they are **not** stated in the source context. Confirm or revise before acceptance.

| Item | Reasoning |
|---|---|
| <inferred item or mismatch> | <why it was inferred or why it does not fit> |
```

Everything the story needs that the source does **not** state, and everything in the user's input that does not fit the skill or the context. This is the only place where the model's own inference or invention may appear.

Rules:

- The disclaimer line is mandatory.
- Every row has reasoning.
- Phrase as proposals or questions, not statements.
- Never present an inference as a fact in any other section. If you catch yourself writing something the source does not support, move it here.
- Omit the entire section if nothing was inferred and nothing in the input failed to fit.

Categories to consider:

- Operations the requirement implies but does not state (the matching `GET` after a `POST`).
- Missing entity attributes or `TBD` values in `domain-entities/`.
- Unstated business rules.
- Format conventions (date, currency, locale).
- Permission gaps in the RBAC table.
- Notifications triggered by the operation.
- User input that conflicts with the Product Context, uses a different term, asks for several operations in one story, or asks for something the skill does not cover (e.g. response schemas, implementation details).

---

## Cross-Section Rules

To avoid duplication:

| Concern                               | Section                         |
| ------------------------------------- | ------------------------------- |
| What the operation is and why         | Scope                           |
| What data exists and its rules        | Entity Attributes & Validations (a link to the entity file) |
| What if something goes wrong          | Error / Edge Cases              |
| What is explicitly excluded           | Out of Scope                    |
| What the consumer must expect or handle | Consumer Considerations       |
| What design decision is open          | Open Design Questions           |
| What is inferred, or does not fit     | Inferred Suggestions            |

If two sections would say the same thing, the higher-up section wins; the lower one references or omits.

Avoid uncertain language in Scope and in validations: "or", "optional", "maybe", "at least / at a minimum", "between X and Y", "fast / quick", "flexible", "e.g.", "i.e.", "shouldn't / won't". State the exact value, the exact condition, and what the system shall do. If a condition is dangerous to infer, ask the user.

---

## Key Principles

1. **Stories are for PM review, not engineering specs.** Keep them scannable. No response schemas, headers or implementation details; `design-api-spec` adds them.
2. **One story, one operation.** One action verb, one method, one resource.
3. **Follow the standards religiously.** Method, resource, nesting, pluralization and pagination come from the API standards. The stories are the first enforcement of the standards.
4. **Use domain language exactly.** Entity and attribute names come from `domain-entities/`; roles come from `users/`; rules are cited by code from `Business-rules.md`. Do not invent a synonym.
5. **Link, don't copy.** Entity attributes live in `domain-entities/`. The story links to the entity file and states only what this operation uses, so a change to an entity never leaves a stale copy in a story.
6. **Nest resources per the domain relations**, within the standards' nesting limit.
7. **Don't invent requirements.** If the requirement does not mention delete or update, do not add them. If something looks missing, flag it in Inferred Suggestions; do not add it silently.

## References

- [references/API-standards.md](references/API-standards.md) — Emmanuel Paraskakis's API Standards (default standards file, Step 0.5). Where each part is applied:

| Standards section | Applied in |
|---|---|
| Paths, Resources, Methods, Casing (path segments, path template parameters) | Scope → `Method` and `Resource` |
| Query Parameters, Collections & Pagination | Entity Attributes & Validations → `Parameters` (filters, `offset` / `limit`, comma-separated values) |
| IDs, References | `Parameters` (UUID path IDs); reference attributes |
| Status Codes, Errors | Error / Edge Cases → the HTTP status in System Behavior (`400` vs `409`, `404` only for a missing single resource, empty collection is `200`) |
| Security | Error / Edge Cases → `401` / `403` rows only when the API requires authentication |
| Idempotency, Responses, Properties | Error / Edge Cases (duplicates, concurrency, omitted optional fields) |
| SHOULD / MAY items (`fields`, `sort` / `order`, `expand`, conditional GET, `ETag`) | Open Design Questions, unless the requirement asks for them; `ETag` and staleness signals also in Consumer Considerations once decided |

## Example

File `OUTPUT/Draft/api-stories/Create device type.md`:

```markdown
# Create device type

## Scope

The system shall allow a Rules Manager to create a device type in the catalog.

- **Method:** `POST`
- **Resource:** `/device-types`
- **Source:** `users/rules-manager.md` JTBD 1

## Entity Attributes & Validations

The request uses the canonical definitions and validations in this entity file; its attribute names, types, examples, and error messages are not repeated here:

- [Device Type entity](../../../domain-entities/device-type.md) — accept `model`, `category`, `make` and `minimum manufacture year`. `status` is set by the system to Active and is not accepted in the request.

## Error / Edge Cases

| Scenario | System Behavior | Message Example | Suggested Exit Flow |
|---|---|---|---|
| Mandatory attribute missing or invalid | `400`; no device type is created | "Enter a 4-digit minimum manufacture year." | Consumer corrects the field and resubmits |
| Caller does not hold the Rules Manager role | `403`; no device type is created | TBD | Employee requests the role from an Admin |

## Consumer Considerations

- The response omits optional attributes that have no value; it does not emit them as `null`.
- Consumers must tolerate unknown enum values and additive response fields.

## Open Design Questions

- Should a device type with the same model, make and category return `409`? The entity file defines no uniqueness rule; the standards require a defined duplicate behaviour for `POST`.
```
