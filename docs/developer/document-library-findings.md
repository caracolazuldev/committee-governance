# Document library UX findings (M-019)

Working note for M-019. Tested in the shared Notion fixture workspace using synthetic records only. No production workspace was changed.

## Prototype contents

Created a human-facing `M-019 Library Navigation` page with links to:

- `M-019 Meetings` database
- `M-019 Research & Assets` database
- Proposed views for All Library, Meeting Documents, Research & Resources, Collaboration, Public Records, and Internal Working

Created representative records for a held meeting, published minutes, an unapproved draft, a research link, collaboration notes, a quoted source excerpt, and an upload placeholder.

## Taxonomy and metadata

The Research & Assets database uses a thin schema:

- `Name`
- `Type`: Document, Link, Upload, Quote
- `Source URL`
- `Tags`: agenda, minutes, research, collaboration
- `Date Added`
- `Added By`
- `Visibility`: Internal or Public
- `Status`: Working, Ready, Published
- `Meeting`: relation to the Meetings database

The Meetings database uses `Name`, `Date`, `Status`, `Visibility`, and `Briefing Links`. This keeps narrative documents in Notion while retaining enough structured metadata for retrieval and access filtering.

## Retrieval walkthrough

- Public filter (`Visibility = Public`) returned exactly the two public-designated records: published minutes and quoted source excerpt.
- Internal working filter (`Visibility = Internal AND Status = Working`) returned the draft, collaboration notes, and upload placeholder; none appeared in the public result.
- A relation query from the meeting returned published minutes, the unapproved draft, and collaboration notes.
- Source URLs round-tripped as metadata on every fixture, preserving traceability to the source context.
- Notion schema and record queries succeeded through the API; human-friendly database view configuration remains a Notion UI concern.

## Access boundaries

Authenticated retrieval and metadata-based exclusion of internal records are confirmed. A human published the standalone synthetic `Public Fixture` page through Notion UI after granting the integration connection access. The resulting public URL returned HTTP 200 without authentication, and the response contained none of the internal fixture names. Notion serves the page through a client-rendered shell, so raw HTML did not expose the rendered synthetic title; the API confirmed the content append and the public URL. M-014's API limitation remains: `public_url` itself cannot be set through the API.

The prototype therefore treats `Visibility = Public` as an explicit publication candidate, not as proof of public exposure. Internal working material must remain outside any published view.

## Findings and open decisions

- A single library database with typed records and filtered views is sufficient for this low-volume human-facing prototype; separate databases for every document class would add navigation overhead.
- A Meeting relation is more useful than relying on free-text meeting names for agenda, briefing, minutes, and collaboration retrieval.
- `Visibility` and `Status` must be separate: a record can be internal and ready, or public and published; conflating them makes review and publication filtering unsafe.
- The upload type can represent an asset placeholder, but actual file storage, preview behavior, retention, and citation requirements need a later focused test.
- Final schema, permissions, and publication workflow remain candidates until M-021 reconciliation.

## Test fixture identifiers

- Shared parent page: `ExpImp API Test`
- Navigation page: `M-019 Library Navigation`
- Databases: `M-019 Meetings`, `M-019 Research & Assets`
- All records are synthetic and may be archived after review.
