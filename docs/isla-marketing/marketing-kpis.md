# Isla Marketing Hub: Marketing KPIs

Status: Phase 1 plan. Based on official documentation read on 2026-11-20 (links and details in social-integrations-plan.md).
Last updated: 2026-11-20

## How to read this document

A KPI is a number the team watches to judge marketing performance. For each one we explain what it means, why it matters, and where the number can come from.

Availability labels used below:

| Label | Meaning |
|---|---|
| Platform (confirmed) | Official documentation shows the platform provides it |
| Platform (verify) | Official documentation mentions it, but it is unclear, conflicting or being retired. Check again before building |
| Hub calculates | The Hub works it out from other numbers or from daily saved snapshots |
| Business tracking | The platform cannot provide it. Isla must record it separately |
| Not available | Not found in the official documentation we read |

Important: platforms define reach, views and impressions differently. Do not add them together across platforms. The Hub will show each platform separately.

## Part A: Platform metrics

### 1. Reach

- Meaning: the number of different people who saw the content.
- Why it matters: shows how big the real audience is, not just how often content was shown.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | Page reach metric (page_impressions_unique). Meta announced a wave of Page metric retirements effective 2026-06-15. Third-party sources say it was replaced by page_total_media_view_unique. Official list not yet read in full | Platform (verify) |
| Instagram | reach in the account and media insights (shown in the official Insights guide) | Platform (confirmed) |
| TikTok | No reach field in the official Display API documentation | Not available |

### 2. Impressions

- Meaning: how many times content was displayed. One person can be counted several times.
- Why it matters: shows total exposure and, compared with reach, how often people see the same content.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | Older impression metrics were retired in waves; replacements are called media views | Platform (verify) |
| Instagram | One official reference page says the impressions metric was retired for all versions from 2025-04-21, while an older official guide still shows it in an example. Use Views instead until verified | Platform (verify) |
| TikTok | Not in the official Display API documentation | Not available |

### 3. Views

- Meaning: how many times a video or post was viewed or played.
- Why it matters: the main measure of attention for video-led platforms.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | Page and post media views (replacement metrics). Names to be confirmed from Meta's official metric list | Platform (verify) |
| Instagram | views and total_views on media insights. total_views also counts ad and Facebook views and is available only with the Facebook Login route | Platform (confirmed) |
| TikTok | view_count on each video (Display API video object) | Platform (confirmed) |

### 4. Engagement

- Meaning: actions people take on content, such as likes, comments, shares, saves and reactions.
- Why it matters: shows whether content makes people care, not just look.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | Post reaction totals (post_reactions_like_total and similar) are shown in the official Page Insights guide. Comments and shares per post need verification | Platform (confirmed) for reactions, Platform (verify) for the rest |
| Instagram | likes, comments and total_interactions on media insights. Shares and saves need verification | Platform (confirmed) for likes and comments, Platform (verify) for the rest |
| TikTok | like_count, comment_count and share_count on each video. No save field in the video object | Platform (confirmed) |

### 5. Engagement rate

- Meaning: engagement as a percentage of how many people saw the content.
- Why it matters: lets the team compare a small post with a big one fairly.
- Source: Hub calculates.
- Proposed formula (needs owner approval): total engagement divided by views, multiplied by 100, per platform. A second view divides by followers. Rates are never averaged across platforms because the definitions differ.

### 6. Followers and follower growth

- Meaning: the number of people following the account, and how that number changes.
- Why it matters: shows audience size and whether it is growing.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | fan_count on the Page (Pages API). A follows metric may also exist and needs verification | Platform (confirmed) for the count |
| Instagram | follower_count insight (not available for accounts under 100 followers). Total followers from the profile needs verification | Platform (confirmed) for the insight |
| TikTok | follower_count, requires the user.info.stats permission. Gives the current total only, not history | Platform (confirmed) |

Growth is calculated by the Hub from daily saved snapshots (Hub calculates). History starts on the day an account is connected, except where a platform supplies past values.

### 7. Profile visits

- Meaning: how many times people opened the account's profile or Page.
- Why it matters: shows curiosity about Isla after seeing content.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | Page view metrics. Some Page view breakdowns were retired in 2024 according to third-party sources | Platform (verify) |
| Instagram | profile_views, shown in the official Insights guide example | Platform (verify) |
| TikTok | Not in the official Display API documentation | Not available |

### 8. Link clicks

- Meaning: how many times people clicked a link, for example to the website or booking page.
- Why it matters: the step between interest and inquiry.

| Platform | Source | Label |
|---|---|---|
| Facebook Page | Click metrics by type are mentioned in Meta's insights reference | Platform (verify) |
| Instagram | Not confirmed in the official pages read | Platform (verify) |
| TikTok | Not in the official Display API documentation | Not available |

Reliable alternative: tracked links (UTM links) created by the Hub, with clicks counted by Isla's own website analytics or a link tool. Label: Business tracking. Which tool Isla will use is an open question.

### 9. Leads and inquiries

- Meaning: people who contact Isla because of marketing (form, WhatsApp, message, phone call).
- Why it matters: shows whether attention turns into real interest.
- Source: Business tracking. Platforms do not provide this through the analytics connections planned for the first release.
- First release: a simple manual log with fields for date, channel, campaign or tracked link, destination and status.

### 10. Booking conversions

- Meaning: bookings that can be traced back to marketing.
- Why it matters: the real return on marketing effort.
- Source: Business tracking. Available only when Isla records bookings with a marketing source, or later when a booking system is connected.
- First release: manual entry linked to an inquiry.
- Decision needed: how to credit a booking (last tracked link, first contact, or the guest's own answer to 'how did you hear about us').

### 11. Content performance by format and platform

- Meaning: comparing how different formats (for example Instagram feed posts, Reels and Stories, Facebook posts and videos, TikTok videos) perform on each platform.
- Why it matters: tells the team what to make more of.
- Source: Hub calculates from the per-post numbers above.
- Notes: Instagram media insights identify the format (feed, Reels, Story). Story numbers are only available for 24 hours, so the Hub must save them within that window. Instagram gives no insights for individual items inside an album.

## Part B: Data timing and storage rules

- Instagram numbers can be delayed by up to 48 hours. Every Hub figure will show a 'last updated' time.
- Platforms keep history for a limited time: Instagram account metrics up to 90 days, Instagram media metrics up to 2 years, Facebook Page metrics 2 years for public Pages. The Hub saves its own daily snapshots from the day of connection.
- When Instagram has no data it returns an empty result, not zero. The Hub must show 'No data' and never show 0 in that case.
- Some Instagram metrics need at least 100 followers.
- A metric that is not available for a platform is shown as 'Not available from this platform', never as a blank or a made-up value.

## Part C: Which KPIs appear in the first release

| KPI | First release |
|---|---|
| Views, engagement, engagement rate | Yes, where the platform provides them |
| Reach and impressions | Only where verified (see labels) |
| Followers and growth | Yes |
| Profile visits | Only if verified |
| Link clicks | Through tracked links; platform clicks only if verified |
| Leads and inquiries | Yes, manual log |
| Booking conversions | Yes, manual entry |
| Content performance by format and platform | Yes |

## Part D: Decisions needed from the owner

1. Approve the engagement rate formula.
2. Choose how bookings are credited to marketing.
3. Choose the tool that counts link clicks (website analytics or a link tool).
4. Confirm which inquiry channels must be logged (website form, WhatsApp, phone, direct messages, booking platforms).
