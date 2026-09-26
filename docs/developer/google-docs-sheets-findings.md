# Google Docs/Sheets API access findings (M-015)

Working note for M-015 (validate Google Docs and Sheets access), tested using the `gcloud auth application-default login` token described in [google-adc-setup.md](google-adc-setup.md), scoped to `documents`, `spreadsheets`, `cloud-platform` (required by `gcloud`), `userinfo.email`, and `openid`.

## Create and read (confirmed working)

- `POST https://docs.googleapis.com/v1/documents` and `POST https://sheets.googleapis.com/v4/spreadsheets` both created files successfully under the human account — no storage-quota restriction, since this uses the human's own OAuth grant (not a service account).
- Content round-tripped correctly: `documents:batchUpdate` (insertText) → `documents.get` read back the exact text; `values` PUT → `values` GET read back the exact grid; `values:append` (recruitment-form-shaped write) correctly appended a new row and reported `updatedRange`.

## Scope boundary: Drive-level operations require a separate scope

Everything below returned **HTTP 403 `insufficientPermissions` / `ACCESS_TOKEN_SCOPE_INSUFFICIENT`**, because none of the granted scopes cover the Drive API:

- Setting custom Drive `properties` on the Doc (the tag-and-file back-reference mechanism, e.g. embedding a Notion page ID) — blocked.
- Restricting sharing via `drive/v3/files/{id}/permissions` (the "respondents cannot see others' entries" requirement) — blocked.
- Reading current permissions on a file — blocked.
- Exporting the Doc via `drive/v3/files/{id}/export` (tested `text/plain`, `text/markdown`, `application/pdf` — all 403, same scope error, not a per-mime-type rejection) — blocked.
- **File lifecycle (trash/delete) also requires Drive scope.** The Docs/Sheets APIs only manage document *content*; there is no way to delete or trash a file without Drive API access. The two test files created during this spike could not be cleaned up via API and remain in the human account as intentionally-kept fixtures.

**Conclusion:** `documents` + `spreadsheets` scopes are sufficient for the read/write content operations M-015 asked about, but the tag-and-file convention, submission-privacy restriction, export, and file cleanup all require adding a Drive scope (likely `https://www.googleapis.com/auth/drive.file`, which is scoped to files the app creates/opens rather than full Drive access) before M-017 (Docs-to-Notion bridge) or M-020 (recruitment form) can be implemented. This is a deliberate least-privilege choice for this spike, not a platform limitation — re-authorization with the added scope is a small follow-up, not a blocker.

## Artifacts

- Test Doc: `12zURSQuVCQcdfFbnf9BU-6YOSbh6i8cOKNL5CKvBPR8` ("M-015 ExpImp API Test Doc")
- Test Sheet: `14Ulg-9-P0nLmBuw2QsW5FeScZfCVqBttLDZWVwIsR9Q` ("M-015 ExpImp API Test Sheet")
- Both remain in the authorizing human's Drive (could not be trashed via API — see scope boundary above).
