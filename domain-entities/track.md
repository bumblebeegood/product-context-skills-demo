# Track

A thematic grouping of the conference (for example "API Design" or "AI/ML"). A Track contains many Talks; every Talk belongs to exactly one Track.

| attribute name | type | mandatory | description | limitations/validations | error messages | examples |
|---|---|---|---|---|---|---|
| Name | String | Y | Name of the track, shown to attendees | 1–60 characters after trimming; unique across tracks, case-insensitive (BR-TRK-01) | "Track name is required and must be 1–60 characters." / "A track with this name already exists." | "OpenAPI Summit", "AI/ML" |
| Description | String | Y | What the track covers | 1–500 characters | "Track description is required and must be at most 500 characters." | "Sessions on designing, documenting and governing APIs with OpenAPI." |
| Display order | Integer | Y | Position of the track in track lists; lower comes first | Whole number ≥ 1; ties are ordered by Name (BR-TRK-02) | "Display order must be a whole number of 1 or more." | 1, 2 |
