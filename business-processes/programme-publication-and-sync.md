# Programme Publication and Sync

## Purpose
Keep the website, mobile app, venue screens, and other partner apps showing the same, current programme after every organizer change.

## Trigger
A Conference Organizer saves a change to a track, speaker, or talk.

## Roles Involved
- Conference Organizer — makes the change.
- Integration Partner — picks up the change and republishes it.
- Attendee — sees the current programme in a partner app or directly.

## Process Steps
1. Save the change to a track, speaker, or talk; it is visible immediately and affected talks get a new Last updated — [Conference Organizer] (`workflows/set-up-track.md`, `workflows/update-speaker-profile.md`, `workflows/reschedule-talk.md`) (BR-SYN-01, BR-SYN-02, BR-SYN-03)
2. Request the talks changed since the last sync — [Integration Partner] (`workflows/sync-programme-changes.md`)
3. Update the partner's copy and displays, marking cancelled talks as cancelled — [Integration Partner]
4. Read the current schedule, now consistent across channels — [Attendee] (`workflows/view-talk-details.md`)
5. Periodically download the full programme to detect deletions — [Integration Partner] (`workflows/download-full-programme.md`)

## Outputs
Partner apps that match the live programme.

## Notes / Exceptions
- No change warnings are sent to attendees in this version; apps may use status and Last updated to build them.
- Deleted talks are only detectable through a full download (BR-SYN-05).
