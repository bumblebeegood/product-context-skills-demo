# General Description
The Conference Schedule API is the single source of truth for a developer conference's programme: its Tracks, the Talks in each Track, and the Speakers who deliver them. Anyone can read the programme without signing in; only Conference Organizers can change it. It serves a single conference held at one venue (time zone America/Los_Angeles, all times published in UTC).

# What Problems It Addresses
- Attendees cannot find talks that match their interests among hundreds of sessions in a spreadsheet or PDF.
- Rooms are hard to find and schedules change; people need to know exactly when and where a talk happens.
- Attendees cannot easily see who is speaking, their background, and what else they present.
- Last-minute changes (room swaps, cancellations) never reach the printed programme.
- There is no easy way to filter by format (for example, only workshops).
- Organizers answer the same "where is room X / is this talk still on?" questions all day and correct the schedule in several places by hand.

# Benefits
- **Attendees**: find sessions that match their skill gaps, meet the people who have the know-how they need, and get full value from their ticket and travel.
- **Speakers**: know exactly where to be and when; see who else is on their track to coordinate and cross-promote.
- **Organizers and the industry**: one place to change the schedule that every channel picks up; open access lets third parties build apps; fewer questions to staff and happier attendees who return next year.

# High-Level Functionality
- Browse the programme by track, date and format; open talk details (description, time, room, track, speakers).
- Look up speakers, search them by name, and see their talks.
- List tracks in the organizers' display order.
- Organizers maintain tracks, speakers and talks (schedule, reschedule, cancel, delete); cancelled talks stay visible, marked *Cancelled*.
- Changes are visible immediately; every talk records when it was last updated so Integration Partners can download the full programme and sync only what changed.
- Conflict protection: no double-booked rooms or speakers.

See `Business-rules.md` for the rules and `workflows/` and `business-processes/` for operations.

# User Groups / Stakeholders
- Attendee — see `users/attendee.md`
- Speaker — see `users/speaker.md`
- Conference Organizer — see `users/conference-organizer.md`
- Integration Partner — see `users/integration-partner.md`

# Future Roadmap / Expansion
Not in this version (out of scope in the brief):
- Multiple conferences or multiple venues.
- Personal agendas or favourites stored on the server.
- Ticketing, registration, and room capacity.
- Speaker self-service login.

Open for a later version: change warnings to attendees for talks they plan to attend (this version only exposes status and last-updated data for apps to do so).
