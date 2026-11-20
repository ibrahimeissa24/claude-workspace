# Isla Marketing Hub: Project Overview

Status: Phase 1, documentation only. Nothing is built yet.
Last updated: 2026-11-20
Branch: feature/initial-workspace

## 1. What Isla Marketing Hub is

Isla Marketing Hub is a private website for Isla's marketing team. It brings Isla's social media into one place so the team can see how content performs, plan what to post next, and understand whether marketing leads to inquiries and bookings.

It will connect to Isla's own official accounts using the platforms' official connection methods (called APIs). It will never ask anyone for a password and will never copy data from the platforms by unofficial means.

In simple terms, the Hub will:

- Read numbers from Isla's accounts (for example views, likes, followers).
- Save those numbers over time so trends can be seen.
- Help the team plan and organise content in Arabic and English.
- Keep Isla's brand knowledge in one place so everyone writes in the same voice.

## 2. Problems it should solve

These are the problems we expect. They are assumptions and must be confirmed by Isla's owner (see open-questions.md).

| # | Expected problem | How the Hub helps |
|---|---|---|
| 1 | Performance numbers live in each app separately, so comparing Facebook, Instagram and TikTok is slow | One dashboard with all channels side by side |
| 2 | It is hard to know which kinds of content work best | Performance by platform and content format |
| 3 | It is hard to connect marketing activity to inquiries and bookings | Tracked links and a simple inquiry and booking log |
| 4 | Content ideas and plans are scattered across chats and notes | A single content planner |
| 5 | Different people may write in different styles, in two languages | A Brand Brain that records voice, rules and examples |
| 6 | Reports take manual effort | Ready-made reports (later release) |

## 3. Who will use it

Proposed roles (to be confirmed):

| Role | What they do in the Hub |
|---|---|
| Owner | Sees everything, approves decisions, connects official accounts |
| Marketing manager | Plans content, reads analytics, manages the Brand Brain |
| Content creator or social media specialist | Drafts and plans posts |
| Designer or video editor (later release) | Uploads and reviews creative work |
| Sales or reservations team (later release) | Logs inquiries and bookings that came from marketing |
| Viewer | Read-only access to dashboards and reports |

## 4. Main modules

| Code | Module | Plain-language description | Release |
|---|---|---|---|
| M1 | Brand Brain | One place for Isla's company facts, voice, audiences and rules | First release |
| M2 | Account Connections | Safely connect Isla's Facebook Page, Instagram account and TikTok account | First release |
| M3 | Analytics Dashboard | Shows performance numbers from the connected accounts | First release |
| M4 | Content Planner | A calendar and board to plan content (planning only, no automatic posting) | First release |
| M5 | AI Content Studio | Suggests ideas and draft captions in Isla's voice, always reviewed by a person | Second release |
| M6 | Review and Approvals | Draft, review and approve content with comments | Second release |
| M7 | Reports | Monthly and custom reports in Arabic and English | Second release |
| M8 | Inquiry and Booking Tracking | A simple log of inquiries and bookings with tracked marketing links | First release (manual entry) |
| M9 | Users, Roles and Language | Login, permissions, Arabic-first interface with English support | First release |

## 5. First release and what is postponed

The first release focuses on marketing analytics and content operations, as requested.

Included in the first release:

- Login, roles, Arabic-first interface with English and right-to-left layout (M9)
- Brand Brain (M1)
- Read-only connections to Facebook Page, Instagram and TikTok (M2)
- Analytics dashboard using real data only (M3)
- Content planner without automatic posting (M4)
- Manual inquiry and booking log with tracked links (M8)

Postponed to the second release:

- AI Content Studio (M5)
- Review and Approvals workflow (M6)
- Reports (M7)

Postponed beyond the second release (not planned yet):

- Posting directly to social platforms from the Hub
- Paid advertising management
- Automatic link to a booking system (including Isla Business OS)
- Other platforms (for example YouTube, X, LinkedIn)
- Reading and answering direct messages
- Native mobile apps (the Hub will work well in a phone browser)
- Automated competitor tracking

## 6. Architecture principles

- A standalone web application.
- Modular and maintainable: each module can change without breaking the others.
- Official platform APIs only for direct social integrations.
- API secrets never appear in frontend code or in GitHub.
- Access tokens are stored securely on the server side only.
- One platform failing must not break the others.
- Real data only: when an account is not connected, the Hub shows 'Not connected' instead of numbers. No sample or placeholder results.
- Arabic-first interface, with English support and right-to-left layout.

## 7. Recommended technology (plain language)

This is a recommendation for the owner to approve. No software has been installed and no code has been written.

| Need | Recommendation | Why, in plain words | Alternatives |
|---|---|---|---|
| Website and server logic | Next.js with React and TypeScript | One toolset for what people see and what runs behind the scenes; large community, easy to find developers | Nuxt (Vue), Laravel (PHP), Django (Python) |
| Arabic and right-to-left | Tailwind CSS with logical spacing, plus next-intl for translations | The page layout flips automatically for Arabic instead of being built twice | Plain CSS with a custom RTL layer |
| Database | PostgreSQL | Reliable place to store brand content, saved numbers and users | MySQL |
| Scheduled data collection | pg-boss (a job queue inside the same database) | Pulls numbers from each platform on a schedule without anyone clicking; no extra service to run | BullMQ with Redis, or a cloud scheduler |
| Protecting connection tokens | Encryption in the database, with the master key kept in a cloud secret manager | A stolen database copy would not reveal usable tokens | HashiCorp Vault or the cloud provider's secrets service |
| Login | Better Auth or Auth.js, with two-step verification for admins | Proven login system; we do not invent our own | A managed login service |
| Hosting | A managed web host with a managed database, or one cloud server running containers | Keeps operations simple; final choice needs owner input on budget and data location | Any major cloud provider |
| Charts | Recharts | Clear charts that work on phones | Chart.js, Tremor |
| AI features (second release) | Claude API called from the server | Keeps AI keys private and lets us require human review | Other AI providers |
| Quality checks | Vitest and Playwright | Automatic tests, including Arabic layout checks | Jest, Cypress |

## 8. Relationship to Isla Business OS

Isla Business OS (a separate plan in the same repository workspace) also includes a Marketing Management module. This plan keeps the Marketing Hub independent so it can later either stay separate or be connected. Which option is preferred is an open question for the owner.

## 9. What this phase does not include

- No application code and no installed dependencies.
- No sample analytics or invented results.
- No passwords, tokens or API secrets stored in GitHub.

## 10. Related documents

- isla-brand-brain.md
- marketing-kpis.md
- social-integrations-plan.md
- product-roadmap.md
- open-questions.md
