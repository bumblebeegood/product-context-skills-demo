# Get speaker profile

## Scope

The system shall allow an Attendee to get a speaker's profile and the talks that speaker presents, so the attendee can understand the speaker's background and plan who to meet.

- **Method:** `GET`
- **Resource:** `/speakers/{speakerId}`
- **Source:** `users/attendee.md` JTBD 4; `workflows/view-speaker-profile.md`
- **Access:** Public read access without authentication (BR-ACC-01)
- **Response behavior:** Return the speaker profile and the speaker's talks ordered by start time ascending, with ties ordered by title (BR-PRG-01, BR-PRG-05). Include cancelled talks with `Status` shown (BR-PRG-02).

## Entity Attributes & Validations

The response uses the canonical definitions and validations in these entity files; their attribute names, types, examples, and error messages are not repeated here:

- [Speaker entity](../../../domain-entities/speaker.md) — return the speaker profile attributes.
- [Talk entity](../../../domain-entities/talk.md) — return the talk attributes needed to identify, understand, and schedule the speaker's presentations, including `Last updated`.
- [Track entity](../../../domain-entities/track.md) — return the talk's track reference according to the entity definition.

### Parameters

| parameter | in | type | mandatory | description |
|---|---|---|---|---|
| speakerId | path | UUID | Y | The identifier of the speaker whose profile is requested. |

## Error / Edge Cases

| Scenario | System Behavior | Message Example | Suggested Exit Flow |
|---|---|---|---|
| `speakerId` is malformed | Return `400 Bad Request` using Problem Details and identify `speakerId` as invalid. | "speakerId must be a valid UUID." | Correct the identifier format and retry. |
| The speaker does not exist | Return `404 Not Found` using Problem Details. | "Speaker was not found." | Return to the speaker list or search for another speaker. |
| The speaker has no talks | Return `200 OK` with the profile and an empty talks collection; do not return `404`. | "talks: []" | Show the profile and indicate that no talks are currently listed. |
| A listed talk is cancelled | Return `200 OK` with the talk and `Status` set to `Cancelled` (BR-PRG-02). | "Status: Cancelled" | Show the cancellation and avoid treating the talk as an available meeting opportunity. |

## Consumer Considerations

- Omit optional speaker attributes when they have no value; do not emit them as `null`.
- Consumers must tolerate unknown enum values and additive response fields.
- Consumers should use each talk's `Last updated` value to recognize potentially stale schedule data.
- Photo and website URLs are optional external links and may be unavailable to the consumer.
- Consumers should handle standard `406`, `429`, and `5xx` responses according to the API standards, including `Retry-After` where supplied.

## Open Design Questions

- Should all of a speaker's talks be embedded in this response, or should the talks be exposed through a separately paginated `GET /speakers/{speakerId}/talks` collection?
- Should the embedded talks contain the full [Talk entity](../../../domain-entities/talk.md) representation or a smaller summary?
- Should this read endpoint support `ETag` and conditional GET for consumers that cache speaker profiles?
