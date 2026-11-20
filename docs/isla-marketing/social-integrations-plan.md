# Isla Marketing Hub: Social Integrations Plan

Status: Phase 1 feasibility plan. Nothing has been built or connected.
Last updated: 2026-11-20
Source access date: 2026-11-20 (all links below were opened on this date)

## 1. Summary for the owner

- The Hub can connect to Isla's Facebook Page, Instagram account and TikTok account using the platforms' official connections.
- Meta (Facebook and Instagram) provides detailed performance numbers. Because Isla would connect only its own accounts, Meta's documentation indicates the simpler 'Standard Access' level should be enough, which may avoid a full Meta app review. This must be confirmed when the app is created.
- TikTok provides profile, follower and per-video numbers (views, likes, comments, shares). The official documents we read do not show reach, impressions, profile visits or audience details for TikTok.
- TikTok requires an app review with a demo video and a public website. Its rules also say apps must not be for private or personal use, so it needs to be confirmed with TikTok that an internal company tool is acceptable.
- Meta retired many Facebook Page metrics in 2026, so every metric must be re-checked against the current official list before building.
- The owner will need to approve the connections by logging in once per platform. The Hub never sees passwords.

## 2. Classification used in this plan

| Label | Meaning |
|---|---|
| Confirmed by official documentation | An official page we opened states it |
| Requires app review or approval | The platform requires review, verification or approval before real use |
| Needs further verification | Official sources are unclear or conflicting, or only unofficial sources mention it |
| Not currently supported or unknown | Not found in the official documentation we read |

Unofficial sources (blogs and data-tool help pages) were used only as hints and never to mark something Confirmed.

## 3. Rules that apply to every platform

1. Official APIs only. No scraping, no password sharing, no unofficial tools.
2. Secrets (app secrets, tokens) live only on the server, never in frontend code and never in GitHub.
3. Each platform has its own connector with its own schedule, error handling and health status. A failure in one connector must not stop the others.
4. Ask for the smallest set of permissions needed (read-only in the first release).
5. Real data only. If an account is not connected or a metric is unavailable, the Hub says so.
6. Every connection can be disconnected by the owner, which also deletes the stored tokens.

## 4. Meta: Facebook Pages and Instagram

### 4.1 What Isla needs before connecting

| Requirement | Status |
|---|---|
| A Meta developer account (owned by the company, not a personal throwaway account) | Needs input from the owner |
| A Meta developer app created for the Hub | Confirmed as required by official documentation |
| A Facebook Page that Isla owns and administers | Confirmed as required for the Facebook Login route |
| An Instagram Professional account (Business or Creator) | Confirmed: the insights documentation covers Business and Creator accounts |
| Instagram account linked to the Facebook Page | Needs further verification for the exact steps (required for the Facebook Login route) |
| The person who connects must be able to perform the Analyze task on the Page | Confirmed (Page Insights guide) |
| A business portfolio (Meta Business account) | Needs further verification |
| A public Privacy Policy URL for the app | Needs further verification |

### 4.2 Two connection routes for Instagram

The official Instagram Platform overview describes two routes:

| | Instagram API with Instagram Login | Instagram API with Facebook Login |
|---|---|---|
| Token type | Instagram user token | Facebook user or Page token |
| Server address | graph.instagram.com | graph.facebook.com |
| Facebook Page needed | Not required | Required |
| Insights permissions | instagram_business_basic, instagram_business_manage_insights | instagram_basic, instagram_manage_insights, pages_read_engagement |
| total_likes, total_comments, total_views metrics | Not available | Available |

Proposed choice: the Facebook Login route, because the Hub needs both the Facebook Page and Instagram, one connection covers both, and it provides the total_* metrics. Final choice to be approved. Status: Confirmed by official documentation for the table; the recommendation is a design decision.

### 4.3 Authorization flow (OAuth) in plain steps

1. The owner clicks 'Connect Meta' in the Hub.
2. The Hub sends the owner to Meta's own login and permission screen. The owner chooses which Page and Instagram account to share.
3. Meta sends the Hub a one-time code.
4. The Hub's server exchanges the code for a short-lived token (valid about one hour for Instagram, per the official overview).
5. The server exchanges it for a long-lived token (valid 60 days and refreshable before expiry, per the official overview).
6. For Facebook, the server uses the user token to list the owner's Pages and obtain a Page access token (the official Page Insights get-started guide shows the call to /me/accounts).
7. The server stores the tokens encrypted and starts scheduled data collection.

Status: steps 4 and 5 Confirmed by official documentation for Instagram. Step 6 Confirmed for Page tokens. Lifetime and renewal rules for Page tokens: Needs further verification.

### 4.4 Permissions and review

| Permission | Purpose | Status |
|---|---|---|
| pages_show_list | List the Pages the person manages | Confirmed |
| pages_read_engagement | Read Page content and engagement; required for Page insights | Confirmed |
| read_insights | Read Page insights | Confirmed |
| instagram_basic | Basic Instagram account access (Facebook Login route) | Confirmed (listed in the insights guide) |
| instagram_manage_insights | Instagram insights (Facebook Login route) | Confirmed (listed in the insights guide) |
| instagram_business_basic, instagram_business_manage_insights | Equivalent permissions for the Instagram Login route | Confirmed |
| business_management | May be needed if the Page is managed through a business portfolio | Needs further verification |
| pages_manage_posts, instagram content publishing permissions | Posting from the Hub | Requires app review or approval; postponed, not requested in the first release |

Access levels (official Instagram Platform overview):

- Standard Access: enough if the app serves only the company's own Instagram professional account or accounts it manages. Status: Confirmed.
- Advanced Access: needed to serve accounts the company does not own or manage, or users with no role on the app. It requires App Review and Business Verification. Status: Requires app review or approval.
- Expectation for Isla: Standard Access should be sufficient because only Isla's own accounts are connected. Whether Facebook Page permissions behave the same way, and whether the app must be in Live mode: Needs further verification when the app is created.

### 4.5 Insights and media endpoints to investigate

| Endpoint | Purpose | Status |
|---|---|---|
| GET /{ig-user-id}/insights | Instagram account metrics | Confirmed |
| GET /{ig-media-id}/insights | Instagram per-post, Reel and Story metrics | Confirmed |
| GET /{ig-user-id}/media | List Instagram posts | Needs further verification |
| Instagram profile fields such as follower count | Total followers | Needs further verification |
| GET /me/accounts | List Pages and Page tokens | Confirmed |
| GET /{page-id}?fields=id,name,about,fan_count | Basic Page details and fan count | Confirmed |
| GET /{page-id}/insights | Facebook Page metrics | Confirmed (but many metrics retired, see below) |
| GET /{page-post-id}/insights | Facebook post metrics | Confirmed |
| Listing Facebook Page posts and videos | Post inventory | Needs further verification |
| Instagram webhooks (story insights) | Capture Story numbers before they expire after 24 hours | Confirmed for the Facebook Login route; not supported for the Instagram Login route |

### 4.6 Known limits and changes

| Limit or change | Status |
|---|---|
| Instagram follower_count and online_followers are not available for accounts under 100 followers | Confirmed |
| Instagram insights data can be delayed up to 48 hours | Confirmed |
| Instagram account metrics are kept up to 90 days; media metrics up to 2 years | Confirmed |
| Instagram Story metrics are only available for 24 hours; Stories with very few viewers return an error | Confirmed |
| Instagram returns an empty result, not zero, when data is missing | Confirmed |
| Instagram album items have no insights | Confirmed |
| Facebook Page metric data is kept 2 years for public Pages; 5 days for unpublished Pages | Confirmed |
| Meta states that a number of Facebook Page Insights metrics are retired by 2026-06-15 for all API versions and return an invalid metric error | Confirmed |
| Which exact Page metrics survive, and their replacement names (for example page_total_media_view_unique) | Needs further verification. Only unofficial sources were read; the official deprecated-metrics page must be checked |
| Instagram impressions metric: one official page says retired in 2025, an older guide still shows it | Needs further verification |
| Facebook Page insights may require a minimum of 100 likes | Needs further verification (unofficial source only) |
| The Graph API version is changing (official pages show different latest versions) | Needs further verification. The Hub will pin and track one version |

### 4.7 Token storage and renewal

- Tokens are stored only on the server, encrypted, with the encryption key held in a secret manager and never in GitHub.
- A daily job checks every token. Instagram long-lived tokens are refreshed well before the 60-day expiry (for example when 14 days remain, a design choice).
- If a refresh fails, the connection is marked 'Needs reconnect', the owner is notified in the Hub, and data collection for that platform pauses. Other platforms keep running.
- Tokens are never written to logs. The owner can disconnect at any time, which deletes the stored tokens.

### 4.8 Meta feature summary

| Feature | Classification |
|---|---|
| Connect Facebook Page and Instagram with OAuth | Confirmed by official documentation |
| Read Instagram account and media insights | Confirmed by official documentation |
| Read Facebook Page and post insights | Confirmed by official documentation (metric list needs verification) |
| Use Isla's own accounts without full app review | Needs further verification (Standard Access indicated by official documentation) |
| Serve accounts Isla does not own | Requires app review or approval |
| Post to Facebook or Instagram from the Hub | Requires app review or approval (postponed) |
| Facebook Page reach and impressions metrics after the 2026 retirement | Needs further verification |
| Read direct messages or comments in bulk | Not currently supported or unknown for this plan (postponed) |

## 5. TikTok

### 5.1 What Isla needs before connecting

| Requirement | Status |
|---|---|
| A TikTok for Developers account | Confirmed as required |
| A registered app with Login Kit and Display API products | Confirmed |
| The TikTok account (@isla.egypt) controlled by Isla, with a person who can approve the connection | Needs input from the owner |
| A public, fully developed website with Privacy Policy and Terms of Service visible without opening a menu | Confirmed as an app review criterion. Isla's site status: needs input |
| A registered web redirect URI | Confirmed |
| App review with a detailed explanation and at least one end-to-end demo video (sandbox for first-time apps) | Requires app review or approval |
| Apps still in development or testing are not approved | Confirmed. The demo must show a working integration, so TikTok review comes late in Phase 3 |
| Apps must not be for private or personal use | Confirmed as a rule. Whether an internal company tool qualifies: Needs further verification with TikTok |

### 5.2 Authorization flow (OAuth) in plain steps

1. The owner clicks 'Connect TikTok' in the Hub.
2. The Hub sends the owner to TikTok's authorization page (https://www.tiktok.com/v2/auth/authorize/), including a random state value to prevent forgery. The owner approves the requested permissions and may approve only some.
3. TikTok sends a code to the registered redirect address.
4. The Hub's server exchanges the code at https://open.tiktokapis.com/v2/oauth/token/ using the app key and secret, which stay on the server.
5. The server stores the tokens encrypted.

Status: Confirmed by official documentation.

### 5.3 Permissions (scopes) for the first release

| Scope | What it allows | Status |
|---|---|---|
| user.info.basic | Avatar, display name, IDs. Added by default with Login Kit | Confirmed |
| user.info.profile | Bio, profile link, username, verified status | Confirmed. Requires app review or approval |
| user.info.stats | Likes total, follower count, following count, video count | Confirmed. Requires app review or approval |
| video.list | Read the account's public videos | Confirmed. Requires app review or approval |
| video.publish, video.upload | Post or draft videos | Confirmed to exist. Requires app review or approval. Postponed, not requested |

TikTok also publishes a bulletin about migrating the user info scopes. Status: Needs further verification before building.

### 5.4 Data available

| Data | Source | Status |
|---|---|---|
| Profile (name, avatar, username, bio, verified) | GET /v2/user/info/ | Confirmed |
| Follower, following, total likes, video count (current values) | GET /v2/user/info/ with user.info.stats | Confirmed. No history is returned, so the Hub saves daily snapshots |
| List of public videos (20 per page, newest first) | POST /v2/video/list/ | Confirmed. Public videos only |
| Per-video views, likes, comments, shares | Video object fields view_count, like_count, comment_count, share_count | Confirmed |
| Video details (title, description, duration, cover image, share link) | Video object | Confirmed. Cover image links expire after 6 hours, so the Hub must re-fetch rather than store them as permanent links |
| Refresh frequency suggested by TikTok's own tutorial | Update recent videos every 12 hours | Confirmed (tutorial guidance) |

### 5.5 Analytics limitations

| Metric | Status |
|---|---|
| Reach, impressions, profile visits, link clicks | Not currently supported or unknown: not found in the official Display API documentation read |
| Audience demographics, watch time, traffic sources | Not currently supported or unknown: not found |
| Daily history of followers | Not provided; the Hub builds it from snapshots |
| Saves or favourites per video | Not currently supported or unknown: not in the video object |
| TikTok analytics through other TikTok products (for example business or advertising products) | Needs further verification. Not reviewed in this phase |

### 5.6 Token storage and renewal

- The access token is valid for 24 hours. The refresh token is valid for 365 days. Both Confirmed by official documentation.
- The server refreshes the access token on a schedule without asking the owner again.
- The refresh response can contain a different refresh token. The Hub must always save the newest one. Confirmed by official documentation.
- Tokens are stored encrypted on the server only. Disconnecting calls TikTok's revoke endpoint and deletes the stored tokens.
- If refresh fails or the 365 days pass, the connection shows 'Needs reconnect'.

### 5.7 TikTok feature summary

| Feature | Classification |
|---|---|
| Login and OAuth with refreshable tokens | Confirmed by official documentation |
| Profile and follower numbers | Confirmed by official documentation; scopes require app review or approval |
| Per-video views, likes, comments, shares | Confirmed by official documentation; scope requires app review or approval |
| Reach, impressions, demographics, watch time | Not currently supported or unknown |
| Internal company tool allowed under the review rules | Needs further verification |
| Posting from the Hub | Requires app review or approval (postponed) |
| Webhooks for TikTok events | Needs further verification |

## 6. Keeping one platform's failure from breaking the others

- One connector per platform, each with its own schedule, queue and error handling.
- Retries use waiting periods that grow longer; after repeated failures the connector pauses and flags the problem.
- The dashboard shows partial data with a clear warning, for example 'TikTok data last updated 3 days ago'.
- A health page shows each connection: connected, needs reconnect, or paused, plus the last successful update.
- The code for each platform lives in its own module so a change to one cannot break another.

## 7. Security checklist

- No secrets, tokens or passwords in GitHub. A secrets scan runs before every commit and release.
- Secrets in environment settings or a secret manager. Never in frontend code.
- Tokens encrypted in the database. Access limited to the server.
- Least privilege: read-only permissions in the first release.
- Audit log of who connected or disconnected an account.
- A privacy policy and a data deletion process are in place before real accounts are connected (platform requirements for the exact deletion process: Needs further verification).

## 8. Checks to complete before Phase 3

1. Read Meta's official deprecated Page metrics list and the current metric tables for Instagram. Fix the final metric list.
2. Create the Meta app and confirm in the dashboard: Standard Access scope, Live versus Development mode, business portfolio need, privacy policy requirement.
3. Confirm Page token lifetime rules.
4. Ask TikTok support or check TikTok's FAQ whether an internal company tool is eligible, and read the user info scope migration bulletin.
5. Confirm whether any TikTok product provides account analytics beyond what is listed here.
6. Confirm the Instagram account is a Professional account linked to the Facebook Page.

## 9. Sources

All opened on 2026-11-20.

Meta (official):

- Instagram Platform overview: https://developers.facebook.com/documentation/instagram-platform/overview
- Instagram account insights reference: https://developers.facebook.com/documentation/instagram-platform/api-reference/instagram-user/insights/
- Instagram media insights reference: https://developers.facebook.com/documentation/instagram-platform/reference/instagram-media/insights
- Instagram insights guide: https://developers.facebook.com/docs/instagram-platform/insights
- Facebook Pages API: https://developers.facebook.com/documentation/pages-api
- Get Page Insights guide: https://developers.facebook.com/documentation/pages-api/platforminsights/page.md
- Page insights get started: https://developers.facebook.com/docs/insights/get-started
- Page insights reference: https://developers.facebook.com/docs/graph-api/reference/page/insights/

TikTok (official):

- Manage user access tokens: https://developers.tiktok.com/docs/en/login-kit-manage-user-access-tokens
- User access token management (OAuth): https://developers.tiktok.com/docs/en/oauth-user-access-token-management
- Scopes overview: https://developers.tiktok.com/docs/en/scopes-overview
- Scopes reference: https://developers.tiktok.com/docs/en/tiktok-api-scopes
- Display API get started: https://developers.tiktok.com/docs/en/display-api-get-started
- Get user info: https://developers.tiktok.com/docs/en/tiktok-api-v2-get-user-info
- List videos: https://developers.tiktok.com/docs/en/tiktok-api-v2-video-list
- Video object: https://developers.tiktok.com/docs/en/tiktok-api-v2-video-object
- App review guidelines: https://developers.tiktok.com/docs/en/app-review-guidelines

Unofficial (hints only, never used to mark a feature Confirmed):

- Supermetrics help page on Facebook field changes: https://docs.supermetrics.com/docs/facebook-insights-field-changes-june-30-2026-1
- Improvado note on Facebook Pages metric changes: https://improvado.io/product-updates/connectors/facebook-pages-metrics-deprecation
- Unified.to Facebook OAuth guide: https://docs.unified.to/skills/facebook-oauth-app/SKILL.md
