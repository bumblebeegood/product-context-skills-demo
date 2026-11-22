# Remove Speaker

## Context
Organizer dropping a presenter from the programme.

## Role
Conference Organizer

## Prerequisites
- The organizer is authenticated (BR-ACC-02).
- The speaker exists.

## Steps
1. Organizer asks to remove the speaker.
2. System checks the speaker is not the only speaker on any talk (BR-SPK-01); if they are, the request is rejected. Organizer first changes the speakers (see `reschedule-talk.md`) or deletes the talk (see `delete-talk.md`).
3. System removes the speaker from the speaker lists of their other talks (BR-SPK-04) and sets Last updated on them (BR-SYN-01).
4. System removes the speaker; they disappear from lists and name search immediately (BR-SYN-03).

## Expected Result
The speaker no longer exists and no talk lists them.
