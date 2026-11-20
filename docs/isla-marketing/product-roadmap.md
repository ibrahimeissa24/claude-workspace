# Isla Marketing Hub: Product Roadmap

Status: Phase 1 plan.
Last updated: 2026-11-20

## How to read this roadmap

- Phases are in build order. Sizes (Small, Medium, Large) are rough relative effort, not time promises. Real timing depends on team size and on how fast Isla answers questions and approves platform requests.
- Module codes (M1 to M9) match project-overview.md.
- Two release milestones:
  - First release: Phases 1 to 4, Phase 5A, plus the early parts of Phase 7 needed to go live.
  - Second release: Phase 5B and Phase 6, followed by a second pass of Phase 7.
- Some hosting work (a public website address, privacy policy, terms) must happen earlier than Phase 7 because TikTok and Meta need them during Phase 3.

| Phase | Name | Modules | Size | Release |
|---|---|---|---|---|
| 1 | Foundation | none | Small | Before first release |
| 2 | Core Marketing Hub | M9, M1 | Medium | First release |
| 3 | Direct social account connections | M2 | Large | First release |
| 4 | Analytics dashboard | M3, M8 | Large | First release |
| 5A | Content planning | M4 | Medium | First release |
| 5B | AI studio | M5 | Medium | Second release |
| 6 | Creative review, team workflow and reports | M6, M7 | Large | Second release |
| 7 | Deployment and production readiness | all | Medium | First release, repeated for second release |

## Phase 1: Foundation

Deliverables:

- The six documents in docs/isla-marketing/.
- A list of questions for the owner and decisions needing approval.

Dependencies:

- Owner time to read the documents and answer the essential questions.

Acceptance criteria:

- All six files are committed on feature/initial-workspace and are readable and consistent.
- No secrets, no fake analytics, no invented company facts.
- The owner has reviewed the plan and approved or changed the technology recommendation.

Risks:

- Missing company information delays the Brand Brain. Mitigation: mark NEEDS INPUT and collect answers early.
- Platform rules change after the documents were written. Mitigation: re-check the items marked Needs further verification before Phase 3.

## Phase 2: Core Marketing Hub

Deliverables:

- Project structure, automated checks and a first test website address (staging).
- Login with roles and two-step verification for admins (M9).
- Arabic-first interface with English and right-to-left layout, working on phone and desktop (M9).
- Brand Brain screens to view and edit the content from isla-brand-brain.md (M1).
- Empty states that say 'Not connected' and never show sample data.

Dependencies:

- Owner approves the technology recommendation.
- Owner answers the essential questions in open-questions.md.
- Hosting and budget decision for staging.

Acceptance criteria:

- Users with different roles see only what their role allows.
- Switching between Arabic and English flips the layout correctly on phone and desktop.
- Brand Brain content can be edited and saved, and changes are recorded.
- Automated tests pass and a secrets scan is clean.

Risks:

- Scope creep into analytics before the foundation is stable.
- Arabic layout bugs found late. Mitigation: test in Arabic from the first screen.

## Phase 3: Direct social account connections

Deliverables:

- A Meta developer app owned by Isla and a connection for the Facebook Page and Instagram account (M2).
- A TikTok for Developers app, app review submission with demo video, and a TikTok connection (M2).
- Secure token storage with encryption, scheduled renewal and disconnect (M2).
- A connection health page showing connected, needs reconnect or paused, with last successful update.

Dependencies:

- Phase 2 staging site with a public website address, privacy policy and terms (both platforms need them).
- Owner access: Page admin rights, Instagram Professional account linked to the Page, TikTok account login to approve.
- Completion of the verification checks listed in social-integrations-plan.md section 8.

Acceptance criteria:

- The owner connects each real account through the platform's own login screen; no password is ever entered in the Hub.
- Tokens are stored encrypted on the server only; a repository secrets scan is clean.
- Token renewal works in a test; a failed renewal shows 'Needs reconnect'.
- Simulating a failure on one platform does not affect the others.
- Disconnecting an account deletes its tokens.

Risks:

- TikTok review delay or rejection, including the rule against private or personal use. Mitigation: ask TikTok early; plan the Hub to work with Meta data alone if TikTok is delayed.
- Meta changes to permissions, metrics or versions. Mitigation: pin one API version and monitor changes.
- Account prerequisites not met (for example Instagram not a Professional account).

## Phase 4: Analytics dashboard

Deliverables:

- Scheduled data collection that saves daily snapshots for the metrics marked available in marketing-kpis.md (M3).
- Dashboard views: overview, one page per platform, content performance by format, follower growth, with date filters (M3).
- Every number shows its source platform and last updated time. Unavailable metrics are labelled as such.
- Manual inquiry and booking log with tracked links (M8).

Dependencies:

- Phase 3 connections working with real accounts.
- Owner decisions on engagement rate, booking attribution and link click tool (open-questions.md).

Acceptance criteria:

- The owner checks at least three figures against the platforms' own apps and they agree, allowing for the documented delays.
- No placeholder or invented values anywhere. No data shows as 'No data', never as zero.
- Data collection for one platform can fail without affecting the others.
- Dashboards are usable on a phone in Arabic and English.

Risks:

- Metrics retired or renamed by Meta. Mitigation: keep a metric mapping table that can be updated without rebuilding.
- Small accounts hit minimum follower rules.
- Data history starts only at connection date for some metrics.

## Phase 5A: Content planning

Deliverables:

- Content calendar and board with idea, draft, ready and published states (M4).
- Fields for Arabic and English captions, destination, platform, format, content pillar and tracked link.
- Links to Brand Brain rules while writing.
- Planning only: the Hub does not post to platforms.

Dependencies:

- Brand Brain content filled in by the owner (content pillars and voice).

Acceptance criteria:

- The team can plan a full month of content for all connected platforms.
- Posts can be linked later to their real performance numbers once published.

Risks:

- Team keeps using chats instead of the planner. Mitigation: short training and simple screens.

## Phase 5B: AI studio (second release)

Deliverables:

- Idea and draft caption suggestions in Arabic and English using the Brand Brain (M5).
- Every AI draft is labelled as a draft and needs human approval. No automatic posting.

Dependencies:

- Completed Brand Brain, including real caption examples.
- Decision on AI provider, cost limits and data handling.

Acceptance criteria:

- The owner reviews a sample of 10 AI drafts and at least 8 follow the voice rules without factual errors about Isla.
- AI never states facts that are not in the Brand Brain.

Risks:

- Generic or inaccurate text, weaker Arabic quality, ongoing cost. Mitigation: mandatory human review and usage limits.

## Phase 6: Creative review, team workflow and reports (second release)

Deliverables:

- Review and approval flow: draft, in review, approved, with comments and notifications (M6).
- Roles for designers and editors; file links for creative assets (M6).
- Reports in Arabic and English, monthly and custom date ranges, exportable (M7).

Dependencies:

- First release in real use with at least one full month of saved data.
- Decisions on who approves content and what reports the owner wants.

Acceptance criteria:

- A post cannot be marked approved without the right role.
- A monthly report is created from real saved data and reviewed by the owner.

Risks:

- Reports built before enough data exists. Mitigation: wait for a full month of data.

## Phase 7: Deployment and production readiness

Deliverables:

- Production hosting, domain name and secure connection (HTTPS).
- Automatic backups and a tested restore.
- Monitoring and alerts, including alerts when a connection needs reconnecting.
- Security review: secrets scan, access review, token encryption check.
- Privacy policy and terms published; data deletion process documented.
- Team training material in Arabic and English, and a launch checklist.

Dependencies:

- Hosting, budget and data location decisions.
- Early pieces (domain, privacy policy, terms, staging) are needed during Phase 3.

Acceptance criteria:

- A backup is restored successfully in a test.
- Secrets scan is clean and no tokens appear in logs.
- The owner signs off the launch checklist.

Risks:

- Hosting cost or location constraints.
- Launching before platform approvals finish. Mitigation: launch with the platforms that are ready; each connector is independent.
