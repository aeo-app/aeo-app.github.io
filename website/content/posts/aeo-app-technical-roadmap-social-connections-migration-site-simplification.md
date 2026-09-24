---
title: "Technical Items"
date: 2026-09-24T10:00:00+10:00
lastmod: 2026-09-24T10:00:00+10:00
tags: ["AEO", "technical roadmap", "social publishing", "Facebook", "LinkedIn", "YouTube", "Google Business", "DynamoDB", "Cognito"]
categories: ["AEO Intel"]
summary: "The live technical roadmap for the AEO Intel product platform (ploutos): finish Facebook, LinkedIn, YouTube and Google Business connections, ship DynamoDB + Cognito migration scripts, and simplify the marketing website."
---

## What this is

A public, versioned technical roadmap for the AEO Intel product platform — the FastAPI + React + AWS application behind aeo-app.ai. This post tracks only **non-content engineering work** and is grounded in the actual codebase state (paths below are real files in the `ploutos` repository).

| # | Item | Current state (from codebase) | What "done" means |
|---|---|---|---|
| 1 | Facebook connection | Posting service implemented (`backend/services/social_publish/meta_service.py`, covers Facebook **and** Instagram on one Meta app) | Production Meta app review passed, long-lived page tokens rotate, invite flow end-to-end verified |
| 2 | LinkedIn connection | Personal-profile posting implemented (`backend/services/social_publish/linkedin_service.py`, self-serve `w_member_social`) | Company Page posting (`w_organization_social`) reviewed and wired, invite flow verified |
| 3 | YouTube connection | **Not started** — no YouTube code anywhere in the repo | YouTube Data API v3 OAuth + video upload available, scheduled publishing works |
| 4 | Google Business | Posting service implemented (`backend/services/social_publish/google_business_service.py`, LocalPosts API) | Map Business Profile API access granted, locations discovered, posts go live through the scheduler |
| 5 | DynamoDB & Cognito migration scripts | **Not started** — no `scripts/` or migration tooling in the repo | Repeatable export/import + cutover scripts with verification for both stores |
| 6 | Marketing website simplification | Single long landing page with 14 stacked sections (`frontend/src/pages/landing/LandingPage.js`) | A small multi-section site mapped to the simplified page plan below |

The social-publishing backend already shares one execution pipeline (`backend/services/social_publish/publish_service.py` + `scheduler.py` on APScheduler), so every "connection" item is mostly **new OAuth + a platform adapter against that shared pipeline** — not a new architecture each time.

---

## 1. Facebook connection

**Codebase state:** `backend/services/social_publish/meta_service.py` is a complete, current-doc-verified implementation of the Meta OAuth flow and both Facebook Page photo posts and the two-step Instagram container publish. Facebook page-posting was tested and committed ("fb connected and able to post").

**What's left to call this complete:**

- **Meta App review** — the production Facebook app must pass review for the permissions the code uses (page posting, media, read engagement, linked Instagram Business account).
- **Long-lived page access tokens** (`grant_type=fb_exchange_token`) with a refresh path, so a connected page stays postable beyond 60 days.
- **End-to-end verification through the invite flow** — the connect-invite path (`backend/routers/social_publish_router.py`) already maps `meta` → Facebook + Instagram; confirm a scheduled post with an image actually lands on the page from a real account before calling it done.
- Store the picked Page + linked Instagram Business account selection (the flow already returns a page picker — persist the chosen `instagram_business_account` id with the connection).

---

## 2. LinkedIn connection

**Codebase state:** `backend/services/social_publish/linkedin_service.py` implements OAuth + image upload + post creation for the **authenticated member's own personal profile**, using the self-serve `w_member_social` scope (no partner-program review required). Connection creation via the invite flow is wired.

**What's left to call this complete:**

- Decide the target: **personal profile** (works today, self-serve) or **Company Page** (`w_organization_social` — needs Community Management API partner approval). The APAC Relocation runbook posts to a brand, not a person, so plan for the company page and start the review early — it's the long pole.
- Wire the company-page variant into the invite/selection flow (the frontend `ConnectPageApprovalPage.js` already lists LinkedIn; extend it to a page picker like the Meta flow).
- Verify image upload → post create sequence against a real company page, plus the token-expiry reconnect path already centralised in `publish_service.py`.

---

## 3. YouTube connection

**Codebase state:** nothing. No YouTube service, no API adapter, no UI entry. This is the only truly greenfield build in the list.

**Suggested build, mirroring the existing `google_business_service.py` shape:**

1. **Enablement** — create a Google Cloud project, enable the **YouTube Data API v3**, configure an OAuth consent screen.
2. **Scopes** — `https://www.googleapis.com/auth/youtube.upload` (upload) and `youtube.readonly` (channel/playlist listing); `youtube.force-ssl` only if the roadmap adds playlist management. Use the same refresh-token pattern `google_business_service.py` already uses (Google access tokens are ~1 hour; refresh before every scheduled publish).
3. **Upload path** — `POST https://www.googleapis.com/upload/youtube/v3/videos?uploadType=resumable` for the video bytes (channels publish videos, not images — media handling differs from the photo-post flow, reuse `media_upload_service.py`'s S3 storage for the source file).
4. **Wire into the shared pipeline** — add to `PLATFORM_LABELS` in `publish_service.py`, add YouTube to the scheduler (`scheduler.py` needs zero change — it calls `publish_to_platforms` by platform list), extend the backend `connect_group` map and the frontend `PLATFORM_META` in `ConnectPageApprovalPage.js`.
5. **Definition of done** — a channel admin authorizes via invite, a scheduled post uploads a video to the right channel, status reflects "posted".

Note: uploading video through a store-and-forward scheduler needs the video URL to stay publicly reachable (S3, public-read — same requirement as the existing image uploads, already enforced by the bucket policy in `backend/infra/s3_bucket.tf`).

---

## 4. Google Business

**Codebase state:** `backend/services/social_publish/google_business_service.py` implements OAuth (business.manage scope), account + location discovery, and LocalPosts creation (My Business API v4), including the short-lived-token refresh helper used by the scheduler. It carries an explicit caveat: **Google Business Profile API access is not fully self-serve** — it has historically required requesting access via a form, separate from the normal Cloud API enablement.

**What's left to call this complete:**

- **Get the API access grant** — submit and confirm the access request in the Google Cloud console for the My Business APIs (this is a human/process step, not a code step; the code already exists).
- **Verify the account/location discovery** against the real business, persist the agreed location id with the connection, and confirm a scheduled LocalPost appears on the live profile (including the CTA/link variant the service already supports).
- Confirm the invite-flow wiring (`google_business` is already an invitable connect group) with the same real-account test as Facebook.

---

## 5. Scripts for DynamoDB & Cognito migration

**Codebase state:** infra is Terraform (`backend/infra/`): a single DynamoDB table `apac_seo_analyses` (single-table design, `gsi_analysis_id` GSI), a Cognito user pool + admins group, and an S3 media bucket. Payments already live in their own table (`db/payments_dynamo.py`). **No migration tooling exists anywhere in the repo.**

### Why migration scripts are needed

- **Cognito schema is immutable.** Custom attributes (`company_name`, `domain`) can only be added at pool creation (documented in `backend/infra/cognito.tf`). An existing deployed pool that predates these attributes cannot accept them in place — the only paths are provisioning a new pool with the schema and migrating users, or falling back to storing that data in DynamoDB only. A scripted migration removes the manual, error-prone version of either path.
- **DynamoDB table structure drifts from Terraform.** The table holds analyses, domain locks, canva PKCE state, scheduled posts, social connections and invites in one single-table layout. Any schema/GSI change (new attributes, new item types, a rename) needs a scripted copy rather than in-place alter.

### Minimum script set (`scripts/` can be empty today)

| Script | What it does | Verification |
|---|---|---|
| `cognito_user_export.py` | Lists all users with attributes + groups via AdminListUsers/AdminGetUser into a line-delimited JSON | Row count matches Cognito console |
| `cognito_user_import.py` | Creates users in the new pool with AdminCreateUser + AdminSetUserPassword (email auto-verified), re-applies groups | Per-user status = EXPIRED/FORCE_CHANGE or reset; OTP login works |
| `cognito_backup_restore.py` | Restores any single user/attribute group from a snapshot for rollback | Undo a test migration end-to-end |
| `dynamo_export.py` | Scans `apac_seo_analyses` (+ payments table) in DynamoDB streams-consistent chunks to JSON lines | Item count + checksum match the live table |
| `dynamo_import.py` | Writes into the new table/name, preserving PK/SK, GSI keys and TTL | Spot-check high-value items (latest analyses, scheduled posts, active social connections) |
| `migrate_verify.py` | Compares record counts, key ranges, GSI counts, and TTL expiry on old vs new | Zero-diff report |

**Cutover notes:** the table name is already an env var (`APAC_SEO_TABLE`, default `apac_seo_analyses`) and Cognito settings are env vars (`COGNITO_USER_POOL_ID` / `CLIENT_ID` / `SECRET`), so a cutover is just repointing environment variables after a scripted verify — design the scripts so the verify runs *before* the env flip, and the import runs twice (dry-run, then final). Back up Terraform state (`backend/infra/terraform.tfstate`) before any `terraform apply` against the new pool — the existing file is empty, so nothing is confirmed in state today.

---

## 6. Marketing website simplification

**Codebase state:** the visitor-facing site is one long React landing page with 14 stacked sections (`frontend/src/pages/landing/LandingPage.js`: Hero, Logos, Stats, Bento, Demo, Solutions, ThreeUp, MarketingAgent, Pricing, Quotes, Blog, Contact, Footer). One page trying to do everything buries the actual product: the output table from the analysis, and how the AI-facing angle (getting *seen and cited* by ChatGPT/Gemini/Perplexity/Claude) works.

**Goal:** every page answers exactly one job, loads fast, and points to exactly one next step. That's what gets the product *crawled, understood, and cited* — which is the whole point of the operation.

### Recommended page map (one-liners)

| Page | Purpose — one line |
|---|---|
| **Home** | One-screen value proposition ("Get found and cited by AI search engines") + a single primary CTA (book a demo / start free) with one real output sample. |
| **Product / How it works** | Three blocks only — AI SEO intelligence, AI content generation, one-click social publishing (the 5 platforms above) — each with a real screenshot, not marketing copy. |
| **Pricing** | The 2–3 real plans with what each unlocks and an honest "free preview → pay to unlock" note; no feature bloat or fake urgency. |
| **About** | Who builds this, who it's for (APAC companies / relocation + SEO agencies), and the link to these public runbooks as proof of method. |
| **Blog / Resources** | The SEO/AEO content library — this Hugo site is already the crawlable, citable proof vault; link to it from a visible footer and nav link. |
| **Contact / Book a demo** | One form, one email, one demo slot; nothing else. |
| **Legal** | Privacy policy + terms (a `privacy-policy.html` already exists in the frontend — give it a real page and a URL). |

Login / signup stay app routes (they already exist as `frontend/src/pages/auth/*`) — they are part of the product, not the marketing site, and should be linked, not duplicated.

### Quick-win simplification tips

- **One page per job.** If a visitor's question ("what does it cost?", "does it post to LinkedIn?") can't be answered on the page they land on, that section is in the wrong place.
- **Cut the filler sections.** Logos walls, generic testimonials, and unverified stats read as noise to AI evaluators and people alike; until there's real proof (real logos, real case-study numbers), drop the section. Real numbers or it doesn't publish.
- **Lead with the output.** The strongest thing this product ships is the analysis results table / generated content. Show that first; explain the engine later.
- **Keep every page under ~500 words.** Short pages are faster, clearer, and cite better in AI answers than 3,000-word single-scroll monoliths.
- **Reduce the 14-section landing page to ~6 sections at most** (Hero, What you get, How it works, Pricing, Proof/runbooks link, Contact) — the rest becomes separate pages from the map above.
- **Internal-link the proof.** Each runbook post on this site is evidence for the method; link them from Blog and About so the site and the product cross-cite each other.

---

## Sequencing note

Items 1–4 share the same publish pipeline, so they can proceed in parallel **per platform** (one engineer per platform, or platform-by-platform in the same order as the codebase maturity: Facebook → Google Business → LinkedIn review → YouTube last because it's the only build-from-scratch). Item 5 sits parallel with any infra change and must land before any new-pool Terraform apply touches production. Item 6 is independent and cheap — it can ship first to make everything after it look credible.