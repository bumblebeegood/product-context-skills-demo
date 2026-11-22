# Speaker

A person who presents one or more Talks. A Speaker may have many Talks; a Talk has one or more Speakers.

| attribute name | type | mandatory | description | limitations/validations | error messages | examples |
|---|---|---|---|---|---|---|
| Name | String | Y | Full name of the speaker; speakers are listed alphabetically by it | 1–100 characters after trimming | "Speaker name is required and must be 1–100 characters." | "Emmanuel Paraskakis" |
| Title | String | Y | Professional title | 1–100 characters | "Speaker title is required and must be 1–100 characters." | "API Consultant" |
| Company | String | N | Employer or organisation | Up to 100 characters | "Company must be at most 100 characters." | "Level 250" |
| Bio | String | N | Short biography | Up to 2000 characters | "Bio must be at most 2000 characters." | "API consultant and educator specializing in API strategy and design" |
| Photo URL | URL | N | Link to the speaker's photo | Absolute http or https URL, up to 2048 characters | "Photo URL must be a valid http or https URL." | "https://example.com/photos/emmanuel.jpg" |
| Website | URL | N | Personal site or social profile, for networking | Absolute http or https URL, up to 2048 characters | "Website must be a valid http or https URL." | "https://www.linkedin.com/in/example" |
