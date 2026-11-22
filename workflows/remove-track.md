# Remove Track

## Context
Organizer dropping a theme from the programme.

## Role
Conference Organizer

## Prerequisites
- The organizer is authenticated (BR-ACC-02).
- The track exists.

## Steps
1. Organizer asks to remove the track.
2. System checks the track has no talks, Scheduled or Cancelled (BR-TRK-03); if it has, the request is rejected and the track stays. Organizer first moves (see `reschedule-talk.md`) or deletes (see `delete-talk.md`) its talks.
3. System removes the track; it disappears from track lists immediately (BR-SYN-03).

## Expected Result
The track no longer exists.
