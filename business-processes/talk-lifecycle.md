# Talk Lifecycle

## Purpose
Take a talk from creation by the organizers to attendees, speakers, and partner apps seeing its current state, including reschedules and cancellation.

## Trigger
A Conference Organizer decides to add a talk to the programme.

## Roles Involved
- Conference Organizer — creates and maintains the talk.
- Attendee — reads the talk to plan their days.
- Speaker — reads the talk to find their room and time slot.
- Integration Partner — copies and syncs the talk into other apps.

## Process Steps
1. Register the speakers and set up the track the talk needs — [Conference Organizer] (`workflows/register-speaker.md`, `workflows/set-up-track.md`)
2. Schedule the talk in a track, room, and time slot; status becomes Scheduled — [Conference Organizer] (`workflows/schedule-talk.md`)
3. Browse and read the talk — [Attendee] (`workflows/browse-talks-by-track-or-date.md`, `workflows/view-talk-details.md`)
4. Find the room and time slot — [Speaker] (`workflows/find-my-talk-room-and-time-slot.md`)
5. Download the programme and later sync changes — [Integration Partner] (`workflows/download-full-programme.md`, `workflows/sync-programme-changes.md`)
6. When things change, move the talk or change its speakers — [Conference Organizer] (`workflows/reschedule-talk.md`)
7. Re-read the changed talk; partners pick it up by Last updated — [Attendee], [Speaker], [Integration Partner]
8. If the talk will not happen, cancel it; it stays visible as Cancelled — [Conference Organizer] (`workflows/cancel-talk.md`)
9. If the talk was created by mistake, delete it — [Conference Organizer] (`workflows/delete-talk.md`)

## Outputs
A talk that every channel shows with the same status, room, and time slot; Last updated on each change; no double-booked rooms or speakers (BR-SCH-01, BR-SCH-02).

## Notes / Exceptions
- A Cancelled talk cannot be reinstated or rescheduled (BR-TLK-08); delete and recreate it instead.
- A deleted talk is not reported as a change; partners detect it by comparing against a full download (BR-SYN-05).
- A track or speaker with talks attached cannot be removed (BR-TRK-03, BR-SPK-01).
