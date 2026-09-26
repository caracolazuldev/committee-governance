# Meeting facilitation UX findings (M-018)

Working note for M-018. A dedicated synthetic facilitation workspace was created under the shared Notion test page. No production meeting or participant data was used.

## Prototype contents

- `M-018 Meeting Facilitation Workspace` with explicit guidance posture.
- `M-018 Configurable Meeting Guidance` page covering agenda order, attendance/quorum notes, recognition, motion handling, amendments, debate, decision methods, and disposition vocabulary.
- `M-018 Meetings` database with Name, Date, Status, Attendance, Guidance Profile, and Briefing Links.
- `M-018 Decisions` database with Motion Text, Motion Type, Mover, Seconder, Lifecycle, Result, Meeting relation, Parent Motion relation, and Date.
- `M-018 Representative Facilitation Scenario` containing agenda items, attendance, human-note sections, action items, and briefing link.
- A main motion, `Adopt a shared resource index`, plus the amendment `Add source citations before publication`.

## Guidance posture

The guidance is labeled configurable support, not authoritative parliamentary, bylaw, consensus, or legal procedure. It explicitly leaves quorum, amendment precedence, debate limits, abstentions, recording consent, retention, and publication approval to committee-specific decisions.

The guidance is placed near the meeting workflow but separated into its own page so a facilitator can consult it without overwhelming the meeting record. The prototype uses a consensus-friendly test profile only; it does not recommend that profile as a default rule.

## API verification

- Meeting and Decisions records were created and retrieved successfully.
- A relation query from the meeting returned both the main motion and amendment.
- The amendment's `Parent Motion` relation returned the main motion ID.
- Lifecycle states were represented as `Amended` for the main motion and `Disposed` for the amendment; both had explicit `Passed` results.
- The meeting page returned 12 content blocks, including agenda, attendance/follow-up, and human-note sections.
- The guidance page returned 15 content blocks.

## Scenario walkthrough status

The structured scenario is ready for a facilitator or participant walkthrough covering:

1. Agenda creation and confirmation of the selected guidance profile.
2. Call to order, attendance, and meeting conduct.
3. Human note-taking during discussion.
4. Main motion, seconder, amendment, and debate.
5. Amendment disposition followed by final main-motion disposition.
6. Action items and adjournment.

A human usability walkthrough is still pending. It must record confusion, missing steps, guidance that is too intrusive, and facilitation needs rather than treating API success as UX validation.

## Open policy questions

The committee must choose and document its own rules for quorum, recognition, amendment precedence, debate limits, abstentions, consent to recording/transcription, retention, publication approval, and the vocabulary for non-passing outcomes. Native Notion AI Meeting Notes remains optional and plan-gated; M-018 does not depend on it.
