# Talk

A session delivered by one or more Speakers at a scheduled time in a room. A Talk belongs to exactly one Track and has at least one Speaker. Cancelled Talks stay in the programme with status Cancelled.

| attribute name | type | mandatory | description | limitations/validations | error messages | examples |
|---|---|---|---|---|---|---|
| Title | String | Y | Title of the talk | 1–120 characters after trimming | "Talk title is required and must be 1–120 characters." | "AI-Driven API Design with OpenAPI" |
| Description | String | Y | What the talk covers | 1–2000 characters | "Talk description is required and must be at most 2000 characters." | "How AI assistants change the way we design and review OpenAPI definitions." |
| Format | String (enum) | Y | Kind of session | One of `Keynote`, `Talk`, `Workshop`, `Panel`; duration limits per BR-TLK-04 | "Format must be one of Keynote, Talk, Workshop, Panel." | "Talk", "Workshop" |
| Status | String (enum) | Y | Lifecycle state. Set to `Scheduled` on creation (BR-TLK-06); set to `Cancelled` by cancelling (BR-TLK-07) | One of `Scheduled`, `Cancelled`; not directly settable on create; cannot return from `Cancelled` (BR-TLK-08) | "Status must be Scheduled or Cancelled." / "A cancelled talk cannot be reinstated." | "Scheduled" |
| Room | String | Y | Room or stage where the talk takes place (free text) | 1–60 characters after trimming | "Room is required and must be 1–60 characters." | "PRO Stage" |
| Track | Reference (Track) | Y | The one track the talk belongs to | Must reference an existing track (BR-TLK-01) | "Track is required and must refer to an existing track." | The "OpenAPI Summit" track |
| Speakers | Reference (Speaker) (list) | Y | The speakers delivering the talk | At least one; each must exist; no duplicates (BR-TLK-02) | "A talk needs at least one speaker, and every speaker must exist." | The speaker "Emmanuel Paraskakis" |
| Date | Date | Y | Calendar date of the talk in the venue time zone (America/Los_Angeles), RFC 3339 full-date | Must equal the venue-local date of Start time and End time (BR-TLK-05, BR-TIM-02) | "Date must be a valid date matching the start and end times in the venue time zone." | "2026-02-20" |
| Start time | Date/time | Y | When the talk starts, RFC 3339 in UTC | Must end in `Z`; before End time (BR-TIM-01, BR-TLK-03) | "Start time must be a valid UTC date-time, for example 2026-02-20T21:30:00Z." | "2026-02-20T21:30:00Z" |
| End time | Date/time | Y | When the talk ends, RFC 3339 in UTC | Must end in `Z`; after Start time; duration within format limits (BR-TIM-01, BR-TLK-03, BR-TLK-04) | "End time must be a valid UTC date-time later than the start time." | "2026-02-20T22:15:00Z" |
| Last updated | Date/time | N | Time of the most recent change to the talk. Set by the system on every change (BR-SYN-01, BR-SYN-02); not writable | RFC 3339 in UTC | — | "2026-02-18T09:12:44Z" |
