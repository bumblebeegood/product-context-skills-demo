# Schedule Talk

## Context
Organizer putting a session into the programme.

## Role
Conference Organizer

## Prerequisites
- The organizer is authenticated (BR-ACC-02).
- The track and the speakers exist.

## Steps
1. Organizer provides title, description, format, room, track, speakers, date, start time, and end time.
2. System validates the track, speakers, times, duration, and date (BR-TLK-01 to BR-TLK-05, BR-TIM-01, BR-TIM-02).
3. System checks that no Scheduled talk uses the room or any speaker at an overlapping time (BR-SCH-01, BR-SCH-02, BR-SCH-03).
4. System saves the talk with status Scheduled and sets Last updated (BR-TLK-06, BR-SYN-01); it is visible immediately (BR-SYN-03).

## Expected Result
A Scheduled talk in the programme.
