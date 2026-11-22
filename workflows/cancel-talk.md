# Cancel Talk

## Context
Organizer marking a session as no longer happening.

## Role
Conference Organizer

## Prerequisites
- The organizer is authenticated (BR-ACC-02).
- The talk exists and is Scheduled.

## Steps
1. Organizer cancels the talk.
2. System sets status to Cancelled and keeps the talk in the programme (BR-TLK-07).
3. System frees the room and speakers for other talks (BR-SCH-03) and sets Last updated (BR-SYN-01).
4. Lists and details show the talk marked Cancelled (BR-PRG-02).

## Expected Result
A visible Cancelled talk; its room and speakers are free again.
