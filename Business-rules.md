# Business Rules

## Access

**BR-ACC-01**: If a request only reads the programme (tracks, talks, speakers), then it is allowed without authentication.
**BR-ACC-02**: If a request creates, changes, cancels, or deletes a track, speaker, or talk and the requester is not an authenticated Conference Organizer, then it is rejected.

## Time

**BR-TIM-01**: If a date-time is published or accepted for a talk, then it is expressed in UTC (RFC 3339).
**BR-TIM-02**: If a talk has a Date, then the Date is the calendar date of its start time in the venue time zone, America/Los_Angeles.

## Track

**BR-TRK-01**: If a track is created or renamed with a name already used by another track (case-insensitive), then the request is rejected.
**BR-TRK-02**: If tracks are listed, then they are ordered by display order ascending, with ties ordered by name.
**BR-TRK-03**: If a track still has talks, whether Scheduled or Cancelled, then it cannot be removed.

## Speaker

**BR-SPK-01**: If a speaker is the only speaker on any talk, then the speaker cannot be removed.
**BR-SPK-02**: If speakers are listed, then they are ordered alphabetically by name.
**BR-SPK-03**: If speakers are searched by name, then every speaker whose name contains the search text, ignoring case, is returned.
**BR-SPK-04**: If a speaker who is not the only speaker on a talk is removed, then the speaker is removed from the speaker list of each of their talks.

## Talk

**BR-TLK-01**: If a talk is saved, then it belongs to exactly one existing track.
**BR-TLK-02**: If a talk is saved, then it has at least one speaker, every speaker exists, and no speaker is listed twice.
**BR-TLK-03**: If a talk's end time is not later than its start time, then the talk is rejected.
**BR-TLK-04**: If a talk's duration is outside the limit for its format (Keynote 15–60 minutes, Talk 15–60 minutes, Panel 15–90 minutes, Workshop 30–240 minutes), then the talk is rejected.
**BR-TLK-05**: If a talk's start time and end time do not both fall on its Date in the venue time zone, then the talk is rejected.
**BR-TLK-06**: If a talk is created, then its status is Scheduled.
**BR-TLK-07**: If a talk is cancelled, then its status becomes Cancelled and it stays in the programme instead of being deleted.
**BR-TLK-08**: If a talk is Cancelled, then it cannot be reinstated, rescheduled, or have its speakers changed.
**BR-TLK-09**: If a Conference Organizer deletes a talk, whether Scheduled or Cancelled, then the talk is removed from the programme entirely.

## Scheduling

**BR-SCH-01**: If two Scheduled talks use the same room (compared ignoring case and surrounding spaces) at overlapping times, then the later change is rejected; talks that only touch (one ends when the other starts) do not overlap.
**BR-SCH-02**: If a speaker would be booked on two Scheduled talks that overlap in time, then the later change is rejected.
**BR-SCH-03**: If a talk is Cancelled, then it is ignored by BR-SCH-01 and BR-SCH-02, freeing its room and speakers.
**BR-SCH-04**: If a talk's room, time, or speakers change, then BR-SCH-01, BR-SCH-02, BR-TLK-02, BR-TLK-03, BR-TLK-04, and BR-TLK-05 are checked against the new values, ignoring the talk's own previous values.

## Programme Reading

**BR-PRG-01**: If talks are listed, then they are ordered by start time ascending, with ties ordered by title.
**BR-PRG-02**: If talks are listed, then Cancelled talks are included with their status shown.
**BR-PRG-03**: If a talk's details are shown, then they include the name of its track and the names of its speakers.
**BR-PRG-04**: If talks are listed with a track, date, or format filter, then only talks matching every given filter are returned.
**BR-PRG-05**: If a speaker's profile is shown, then it includes the speaker's talks, ordered as in BR-PRG-01.

## Synchronisation

**BR-SYN-01**: If a talk is created, rescheduled, edited, or cancelled, then its Last updated is set to the time of the change.
**BR-SYN-02**: If a track's name or a speaker's name or profile changes, then Last updated is set to the time of the change on every talk that shows that track or speaker.
**BR-SYN-03**: If a Conference Organizer saves a change, then it is visible in all reads immediately.
**BR-SYN-04**: If talks changed since a given time are requested, then every talk, including Cancelled ones, with Last updated later than that time is returned, ordered by Last updated ascending.
**BR-SYN-05**: If a talk is deleted, then it is not reported by BR-SYN-04; consumers detect it by comparing against the full programme.
