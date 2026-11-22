# Conference Schedule API — Product Brief

## Summary

The Conference Schedule API is the single source of truth for a developer conference's programme: its Tracks, the Talks in each Track, and the Speakers who deliver them. Attendees, speakers, and third-party apps read the programme without signing in. The organizing team maintains it through the same API, so the website, the mobile app, and the venue screens always show the same, current schedule.

## Problem

Based on interviews with 40+ conference attendees:

- Attendees can't find talks that match their interests without scrolling through hundreds of sessions in a spreadsheet or PDF.
- Conferences are big, rooms are hard to find, and schedules change. People need to know exactly when and where a talk is happening.
- Attendees want to see who is speaking, their background, and what else they present, so they can plan who to meet.
- Last-minute changes (room swaps, cancellations) never reach the printed programme.
- There is no easy way to filter by format: "I only want workshops, not keynotes."
- Organizers field the same "where is room X / is this talk still on?" questions all day, and correct the schedule in several places by hand.

## Users

| User | Who they are | What they need from the programme |
|---|---|---|
| **Attendee** | Developers and tech leads attending the conference. | Browse and plan their days: by track, date, and format; read talk details; look up speakers. |
| **Speaker** | People presenting one or more sessions. | Find their room and time slot; see who else is on their track. |
| **Conference Organizer** | The programme team. | Define tracks, register speakers, schedule talks, and keep the schedule correct when things change on the day. |
| **Integration Partner** | Third-party apps: schedule builders, digital signage, networking tools, the official mobile app. No human in the loop. | A complete, current, machine-readable copy of the programme, and a way to pick up changes quickly. |

## Benefits

**Attendees**
- Leave better educated: they find the sessions that match their skill gaps, not just whatever is next door.
- Meet the people who have the know-how they need, which turns the conference from passive listening into active networking.
- Get full value from their ticket, travel, and days out of the office.

**Speakers**
- Know exactly where to be and when. No more wandering the venue looking for their room five minutes before their talk.
- See who else is on their track, which makes it easier to coordinate and cross-promote sessions.

**Organizers and the industry**
- One place to change the schedule; every channel picks it up.
- Open access lets third parties build apps that improve the overall experience.
- Fewer questions to conference staff and less frustration. Happy attendees come back next year.

## Use Cases

Attendee and speaker use cases, in order of popularity from the interviews:

1. Browse talks by track or date.
2. Get the details of a specific talk: description, time, room, track, and speakers.
3. Filter talks by format (for example, only workshops).
4. View a speaker's profile and their talks.
5. As a speaker, find my talk's room and time slot.
6. List all tracks of the conference.
7. Find a speaker by name.

Organizer use cases:

8. Set up the conference's tracks, with a name, description, and display order.
9. Register a speaker and maintain their profile.
10. Schedule a talk in a track, room, and time slot, with its speakers.
11. Reschedule a talk: move it to another room, time slot, or track, or change its speakers.
12. Cancel a talk.
13. Remove a track or speaker that is no longer part of the programme.

Integration Partner use cases:

14. Download the full programme to display it in another app.
15. Pick up only what has changed since the last sync.

## Product Decisions

These came out of the first design review of the read API.

- **Public read, protected write.** Everyone can read the programme without authentication. Only Conference Organizers can change it.
- **Cancelled talks stay visible.** A cancelled talk is not deleted. It stays in the programme marked *Cancelled*, so apps can tell attendees "this talk was cancelled" instead of the talk silently disappearing. Organizers may still delete a talk that was created by mistake.
- **Schedules read in time order.** Lists of talks are ordered by start time. Tracks follow the display order the organizers set. Speakers are listed alphabetically by name.
- **Talk details show names, not just references.** When an attendee looks at a talk, they see the track's name and the speakers' names without a second lookup.
- **Speakers find their own talks by name.** There is no speaker login in this version. A speaker searches for their own name and opens their talks.
- **Speaker profiles support networking.** In addition to name, title, company, and bio, a profile can carry a photo and a website or social link.
- **Changes propagate immediately.** A change is visible in the API as soon as the organizer saves it. Every talk records when it was last updated, so partners can sync only what changed.
- **One conference, one venue time zone.** This version serves a single conference held in one venue. All times are published in UTC; the venue time zone is America/Los_Angeles.

## Domain

* A Conference has Tracks (thematic groupings like "API Design" or "AI/ML").
* A Track contains Talks. A Talk is a session delivered by one or more speakers at a scheduled time in a room.

### Track
- ID (string, UUID) - required
- Name (string) - example: "OpenAPI Summit" - required
- Description (string) - required
- Display order (integer) - example: 1 - required

### Speaker
- ID (string, UUID) - required
- Name (string) - example: "Emmanuel Paraskakis" - required
- Title (string) - example: "API Consultant" - required
- Company (string) - example: "Level 250"
- Bio (string) - example: "API consultant and educator specializing in API strategy and design"
- Photo URL (string, URL)
- Website (string, URL) - a personal site or social profile

### Talk
- ID (string, UUID) - required
- Title (string) - example: "AI-Driven API Design with OpenAPI" - required
- Description (string) - required
- Format (enum: Keynote, Talk, Workshop, Panel) - example: "Talk" - required
- Status (enum: Scheduled, Cancelled) - example: "Scheduled" - required, Scheduled when created
- Room (string) - example: "PRO Stage" - required
- TrackID (string, UUID, reference to Track) - required
- SpeakerIDs (array of string, UUIDs, references to Speaker) - required
- Date (date, RFC3339/ISO8601) - example: "2026-02-20" - required
- StartTime (datetime, RFC3339/ISO8601 UTC) - example: "2026-02-20T21:30:00Z" - required
- EndTime (datetime, RFC3339/ISO8601 UTC) - example: "2026-02-20T22:15:00Z" - required
- LastUpdated (datetime, RFC3339/ISO8601 UTC) - set by the system on every change

### Relations
1. A Track has many Talks.
2. A Talk belongs to exactly one Track.
3. A Talk has one or more Speakers.
4. A Speaker may have multiple Talks.

## Rules We Already Know

- A talk always belongs to exactly one track and has at least one speaker.
- A talk ends after it starts.
- Two scheduled talks cannot use the same room at overlapping times. A cancelled talk frees its room.
- A speaker cannot be booked into two scheduled talks that overlap.
- A track that still has talks cannot be removed.
- A speaker who is the only speaker on a talk cannot be removed.

## Open Questions

- How long can each format run (for example, are keynotes capped and workshops longer)?
- Is the list of rooms fixed for the venue, or free text entered per talk?
- What are the length limits for titles, descriptions, and bios?
- Can a cancelled talk be reinstated?
- Should attendees be warned when a talk they plan to attend changes?

## Out of Scope (this version)

- Multiple conferences or multiple venues.
- Personal agendas or favourites stored on the server.
- Ticketing, registration, and room capacity.
- Speaker self-service login.
