# Extract Assumptions

Source: INPUTS/conference-product-brief.md · Mode: --assume · Date: 2026-11-22

Every item below was not stated in the input and was assumed to complete the Product Context. Confirm, correct, or remove each one via `context-management`.

| File | Item | Assumed | Reasoning |
|---|---|---|---|
| `Business-rules.md` | BR-TLK-08 — open question "Can a cancelled talk be reinstated?" | No: a Cancelled talk cannot be reinstated, rescheduled, or have speakers changed; delete and recreate instead | No reinstate use case in the brief; keeps Status one-way and conflict checks simple |
| `Business-rules.md` | BR-TLK-04 — open question on format durations | Keynote 15–60, Talk 15–60, Panel 15–90, Workshop 30–240 minutes | Domain-typical caps; workshops longer than keynotes as the brief hints |
| `domain-entities/talk.md` | Room — open question "fixed list or free text" | Free text, 1–60 characters; compared ignoring case and spaces (BR-SCH-01) | Brief models Room as a plain String; no room entity is listed |
| `domain-entities/*.md` | Length limits — open question | Track name 60, description 500; speaker name/title/company 100, bio 2000; talk title 120, description 2000; URLs 2048 | Common API defaults |
| `Business-rules.md` | BR-SYN-05 — deleted talks and sync | Deleted talks are not reported by changed-since; partners detect them via full download | Brief gives deletion and delta sync but no tombstone mechanism; no feature added |
| `Business-rules.md` | BR-SYN-02 — Last updated on track/speaker edits | Editing a track name or speaker updates Last updated on their talks | Talk details embed track and speaker names, so partners need to re-fetch them |
| `Business-rules.md` | BR-TRK-03 — "still has talks" | Counts Scheduled and Cancelled talks | Cancelled talks stay in the programme and still reference the track |
| `Business-rules.md` | BR-SPK-01 — sole speaker | Applies to any talk, including Cancelled ones | Same reasoning; Cancelled talks still list speakers |
| `Business-rules.md` | BR-SPK-04 — removing a non-sole speaker | Speaker is removed from their other talks' speaker lists | Implied by "references" in Relations; avoids dangling references |
| `Business-rules.md` | BR-SCH-01 — overlap definition | Back-to-back talks (end = next start) do not overlap | Standard interval convention |
| `Business-rules.md` | BR-TLK-05 / BR-TIM-02 — Date vs. times | Date equals the venue-local (America/Los_Angeles) date of start; end on the same date | Brief example fits (21:30Z on 2026-02-20 is 13:30 local); no multi-day talks implied |
| `Business-rules.md` | BR-TRK-01 — unique track names | Track names unique, case-insensitive | Prevents ambiguous tracks; speaker names are not unique (people share names) |
| `Business-rules.md` | BR-SPK-03 — name search | Case-insensitive "contains" match | Typical for a "find by name" use case |
| `Business-rules.md` | BR-PRG-01 — tie order for talks | Ties ordered by title | Brief orders by start time only |
| `Business-rules.md` | BR-PRG-02 / BR-PRG-04 — filters | Lists include Cancelled talks by default; track, date, format filters combine with AND | Cancelled talks "stay visible"; use cases 1 and 3 imply filters |
| `Business-rules.md` | BR-TRK-02 — tie order for tracks | Ties ordered by name; display order ≥ 1 | Brief gives display order, not tie handling |
| `Business-rules.md` | BR-TLK-09 — delete scope | Organizer may delete a talk in either status | Brief allows deleting mistaken talks without limiting by status |
| `Business-rules.md` | Open question "warn attendees of changes" | No warnings in this version; apps use status and Last updated | Personal agendas are out of scope, so the server cannot know who planned what |
| `domain-entities/talk.md` | Last updated — mandatory | N (not user-supplied; system-set) | Brief marks it system-set, not required input |
| `domain-entities/talk.md` | Status — direct writes | Not settable on create; changed only by cancelling | Brief: "Scheduled when created" |
| `domain-entities/*.md` | URL validation | Absolute http/https URLs | Common safe default |
| `domain-entities/*.md` | Error messages | All wording | Brief gives none |
| `users/*.md` | RBAC rows and JTBDs beyond the brief's use cases | Capability rows; Organizer `Full`, others `Read`/`None`; JTBDs rephrased from use cases and Benefits | Derived from "Public read, protected write" |
| `workflows/*.md` | All steps and prerequisites | Step sequence for the 18 operations from use cases 1–15 | Use cases name operations but not steps; includes update-speaker-profile (use case 9, "maintain") and delete-talk (Product Decisions) |
| `business-processes/*.md` | Both processes | talk-lifecycle and programme-publication-and-sync | Cross-role flows implied by the Summary and Product Decisions |
