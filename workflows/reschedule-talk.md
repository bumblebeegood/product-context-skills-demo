# Reschedule Talk

## Context
Organizer correcting the schedule on the day.

## Role
Conference Organizer

## Prerequisites
- The organizer is authenticated (BR-ACC-02).
- The talk exists and is Scheduled (BR-TLK-08).

## Steps
1. Organizer changes the room, date, start time, end time, track, or speakers of the talk.
2. System rechecks the new values against the validation and conflict rules, ignoring the talk's own old values (BR-SCH-04).
3. System saves the change and sets Last updated (BR-SYN-01); the new schedule is visible immediately (BR-SYN-03).
4. If a rule fails, nothing is saved and the talk keeps its previous values.

## Expected Result
The talk shows its new room, time slot, track, or speakers everywhere.
