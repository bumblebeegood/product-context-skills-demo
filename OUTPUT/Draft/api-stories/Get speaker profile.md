# Get speaker profile

## Scope

The system shall allow an Attendee to get a speaker's profile and the talks that speaker presents, so the attendee can understand the speaker's background and plan who to meet.

- **Method:** `GET`
- **Resource:** `/speakers/{speakerId}`
- **Source:** `users/attendee.md` JTBD 4; `workflows/view-speaker-profile.md`
- **Access:** Public read access without authentication (BR-ACC-01)
- **Response behavior:** Return the speaker profile and the speaker's talks ordered by start time ascending, with ties ordered by title (BR-PRG-01, BR-PRG-05). Include cancelled talks with `Status` shown (BR-PRG-02).

## Entity Attributes & Validations

### Parameters

| parameter | in | type | mandatory | description |
|---|---|---|---|---|
| speakerId | path | UUID | Y | The identifier of the speaker whose profile is requested. |

### Speaker

| attribute name | type | mandatory | description | limitations/validations | error messages | examples |
|---|---|---|---|---|---|---|
| Name | String | Y | Full name of the speaker; speakers are listed alphabetically by it | 1–100 characters after trimming | "Speaker name is required and must be 1–100 characters." | "Emmanuel Paraskakis" |
| Title | String | Y | Professional title | 1–100 characters | "Speaker title is required and must be 1–100 characters." | "API Consultant" |
| Company | String | N | Employer or organisation | Up to 100 characters | "Company must be at most 100 characters." | "Level 250" |
| Bio | String | N | Short biography | Up to 2000 characters | "Bio must be at most 2000 characters." | "API consultant and educator specializing in API strategy and design" |
| Photo URL | URL | N | Link to the speaker's photo | Absolute http or https URL, up to 2048 characters | "Photo URL must be a valid http or https URL." | "https://example.com/photos/emmanuel.jpg" |
| Website | URL | N | Personal site or social profile, for networking | Absolute http or https URL, up to 2048 characters | "Website must be a valid http or https URL." | "https://www.linkedin.com/in/example" |

### Talk

| attribute name | type | mandatory | description | limitations/validations | error messages | examples |
|---|---|---|---|---|---|---|
| Title | String | Y | Title of the talk | 1–120 characters after trimming | "Talk title is required and must be 1–120 characters." | "AI-Driven API Design with OpenAPI" |
| Description | String | Y | What the talk covers | 1–2000 characters | "Talk description is required and must be at most 2000 characters." | "How AI assistants change the way we design and review OpenAPI definitions." |
| Format | String (enum) | Y | Kind of session | One of `Keynote`, `Talk`, `Workshop`, `Panel`; duration limits per BR-TLK-04 | "Format must be one of Keynote, Talk, Workshop, Panel." | "Talk", "Workshop" |
| Status | String (enum) | Y | Lifecycle state | One of `Scheduled`, `Cancelled`; cannot return from `Cancelled` (BR-TLK-08) | "Status must be Scheduled or Cancelled." / "A cancelled talk cannot be reinstated." | "Scheduled" |
| Room | String | Y | Room or stage where the talk takes place (free text) | 1–60 characters after trimming | "Room is required and must be 1–60 characters." | "PRO Stage" |
| Track | Reference (Track) | Y | The one track the talk belongs to | Must reference an existing track (BR-TLK-01) | "Track is required and must refer to an existing track." | The "OpenAPI Summit" track |
| Date | Date | Y | Calendar date of the talk in the venue time zone (America/Los_Angeles), RFC 3339 full-date | Must equal the venue-local date of Start time and End time (BR-TLK-05, BR-TIM-02) | "Date must be a valid date matching the start and end times in the venue time zone." | "2026-02-20" |
| Start time | Date/time | Y | When the talk starts, RFC 3339 in UTC | Must end in `Z`; before End time (BR-TIM-01, BR-TLK-03) | "Start time must be a valid UTC date-time, for example 2026-02-20T21:30:00Z." | "2026-02-20T21:30:00Z" |
| End time | Date/time | Y | When the talk ends, RFC 3339 in UTC | Must end in `Z`; after Start time; duration within format limits (BR-TIM-01, BR-TLK-03, BR-TLK-04) | "End time must be a valid UTC date-time later than the start time." | "2026-02-20T22:15:00Z" |

## Error / Edge Cases

| Scenario | System Behavior | Message Example | Suggested Exit Flow |
|---|---|---|---|
| `speakerId` is malformed | Return `400 Bad Request` using Problem Details and identify `speakerId` as invalid. | "speakerId must be a valid UUID." | Correct the identifier format and retry. |
| The speaker does not exist | Return `404 Not Found` using Problem Details. | "Speaker was not found." | Return to the speaker list or search for another speaker. |
| The speaker has no talks | Return `200 OK` with the profile and an empty talks collection; do not return `404`. | "talks: []" | Show the profile and indicate that no talks are currently listed. |
| A listed talk is cancelled | Return `200 OK` with the talk and `Status` set to `Cancelled` (BR-PRG-02). | "Status: Cancelled" | Show the cancellation and avoid treating the talk as an available meeting opportunity. |

