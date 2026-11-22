# conference-organizer

## JTBDs
1. Have a set of tracks with a name, description, and the display order they want
2. Have every presenting speaker registered with an up-to-date profile
3. Have each talk scheduled in a track, room, and time slot with its speakers, without conflicts
4. Have the schedule reflect last-minute changes to room, time slot, track, or speakers
5. Have cancelled talks clearly marked as cancelled while staying visible in the programme
6. Have tracks, speakers, and mistakenly created talks that are no longer part of the programme removed
7. Know that every channel shows the same, current schedule

## RBAC

| Capability | Conference Organizer |
|---|---|
| Read tracks | Read |
| Read talks (including cancelled) | Read |
| Read speakers and their talks | Read |
| Read talks changed since a given time | Read |
| Create, update, remove tracks | Full |
| Register, update, remove speakers | Full |
| Schedule, reschedule, cancel, delete talks | Full |
