# AGENTS.md — AEO/GEO Growth Operations (AEO Intel, APAC Relocation, LawMatter)

Operating rules for the client growth plans held in this repo. On conflict: **this file owns operating rules; each plan file owns its own dates and day-by-day assignments** (its table is the executable source — any cadence summary here is a summary).

## What this repo is

Not a software repo. **No application code, no tests, no lint, no typecheck.** It holds client runbooks + two reusable skills, and builds a public Hugo site (`website/`) published at `https://aeo-app.github.io/` via GitHub Pages so the runbooks stay crawlable and citable — the point of the whole operation. All site content is Markdown under `website/content/`. Never commit or hand-edit HTML or build output.

Three separate client operations, each a single self-contained plan file:

| Plan | Client / site | Run | Post |
|---|---|---|---|
| AEO Intel | aeo-app.ai | 90 days from **04 Sep 2026** | `website/content/posts/aeo-intel-90-day-schedule-content-library.md` |
| APAC Relocation | apacrelocation.com | 30-day sprint, **22 Sep → 21 Oct 2026** | `website/content/posts/apac-relocation-30-day-schedule-content-library.md` |
| LawMatter / Comply.LM | complylm.com.au + Lawyers Weekly LawTech: AI Summit (30 Oct 2026, Sydney) | 46 days from **30 Sep → 14 Nov 2026** | `website/content/posts/lawmatter-46-day-aeo-geo-summit-schedule-content-library.md` |

To act on "today": find the plan whose start date ≤ today, compute the day number, and read that row of its table. Do not redraft a calendar.

## Build and deploy

- **Build locally:** `hugo --minify` run from `website/` (Hugo **0.165.0 extended**, matching CI). Builds in <1s; run it after any content or `hugo.yaml` edit. `markup.goldmark.renderer.unsafe: true` is on, so inline HTML/anchors in the Markdown pass through.
- PaperMod emits two deprecation WARNs (`.Language.LanguageDirection` / `.LanguageCode`) on every build. **Harmless — ignore them**, and don't "fix" them in the theme.
- **CI:** `.github/workflows/hugo-deploy.yml` — on push to `main` (or manual dispatch) builds with `peaceiris/actions-hugo@v3` pinned to 0.165.0 extended, then force-publishes `./website/public` to the **`gh-pages`** branch via the built-in `GITHUB_TOKEN`. Concurrency group `pages`, `cancel-in-progress: false`. The old SSH `AEO_APP_PAGES_DEPLOY_KEY` secret is broken and unused.
- **`main` must never contain build output.** `website/public/`, `website/resources/`, and `website/.hugo_build.lock` are gitignored; deploys only ever come from CI.
- `website/public/` is **not** purged of unknown files by a default build, so local output can hold orphaned pages from deleted content (see the `/team/` gotcha). CI builds from a clean checkout, so what's live is what CI generated — verify with `curl -o /dev/null -w '%{http_code}'` against `aeo-app.github.io`, not against local `public/`.
- **DOCX export** needs `pandoc` (installed). Use the `markdown-to-docx` skill; output lands in `output/`. `*.docx` is gitignored, but the `output/` **directory** is not — don't drop non-DOCX files there expecting them to stay uncommitted.

## Gotchas

- **Every post needs Hugo YAML frontmatter.** The two older posts have it; the **LawMatter 46-day post does not** — it starts at `# LawMatter — …`. Consequence, verified by build: the page renders with the *site* title (`<title>AEO Intel Operations</title>`), no date, no summary, and no tags/categories. It has been renamed to kebab-case, so its URL is now `/posts/lawmatter-46-day-aeo-geo-summit-schedule-content-library/`. Still add frontmatter (`title`, `date`, `lastmod`, `tags`, `categories`, `summary`) to match the other two. Note the post is **uncommitted and 404s live**.
- **`website/lawmatter/about.md` is working material, not publish-ready content.** It has no frontmatter, opens with a stray `## About the Company` line above its own H1, and mixes verified research with `[UNCONFIRMED]` tags, a 12-question client list, and a raw marketing-prompt block at the end. Do not treat its claims as approved and do not copy them into published copy — the LawMatter plan's **Claims Ledger** is the gate (see below). It renders publicly at `/lawmatter/about/` as-is.
- **The `team` menu entry is dead.** `website/hugo.yaml` links `/team/` and `website/content/team/` is **empty** — the four role pages there were never committed and are gone. `https://aeo-app.github.io/team/` returns **404** today. Either drop the menu entry or restore the pages; don't assume team pages exist or cite them.
- **Both skills point at exemplar paths that don't exist.** `n-day-plan` and `markdown-to-docx` reference `projects/AEO-Intel_Full_Schedule_and_Content_Library.md` and `projects/APAC-Relocation_30-Day_AEO-GEO_Schedule_and_Content_Library.md`. There is no `projects/` directory; the real exemplars are the two posts in `website/content/posts/`. So `markdown-to-docx` invoked with no path argument **fails** — always pass an explicit input, and fix the skill's paths when you touch it.
- **The `ploutos` codebase is not in this repo.** `website/content/posts/aeo-app-technical-roadmap-social-connections-migration-site-simplification.md` documents real file paths in the external `ploutos` repo (the FastAPI + React + AWS app behind aeo-app.ai) and is a *roadmap document*, not work you can perform here. Never claim to have edited or verified that code from this repo.
- **Skills live only in `.opencode/skills/`.** opencode loads from nowhere else — don't restore anything under `.github/skills/`. `.opencode/.gitignore` excludes `node_modules`/`package*.json`/`bun.lock`, so only the `SKILL.md` files are tracked.
- **Three referenced companion files still don't exist** — `aeo-app-ai_AEO-SEO-GEO_Growth_Report.md`, `AEO-Intel_Content_Prompts_Library.md`, `AEO-Intel_90-Day_Day-by-Day_Activity_Calendar.xlsx`. The AEO Intel plan confirms this at its "Supporting tasks" note (line ~345). Don't invent their content; the missing spreadsheet is why the Progress Tracker has nowhere to log results.

## Cross-plan guardrails

These hold for every plan regardless of deadline pressure:

- **Never publish a fabricated statistic.** Resolve every `[NOTE TO WRITER]` / `[STAT - VERIFY]` / square-bracket placeholder with real, sourced data first. AI engines down-weight caught fakes — this is the top guardrail in the whole repo.
- **Publish on the table's day, not the header's day.** The AEO Intel file contradicts itself: its rhythm note and every brief header say "Tactical (Tue)" / "Pillar (Thu)", but the executable table assigns tactical to **Wednesdays** and pillar to **Fridays**. The table wins.
- **Never skip a phase gate.** The AEO Intel Day 14 go/no-go (crawlability) and the APAC Day 5 gate block their execution phases — publishing onto unreadable pages wastes the sprint. LawMatter uses Day 6 / 18 / 30 gates.
- **No mid-cycle topic invention.** Each plan's 20-post / deliverable arc is deliberately sequenced. Check the plan's Content Library before proposing anything new.
- **Log the tracker even in a bad week.** A missing row is worse than an honest slow week.
- **Visual brand:** AEO Intel and APAC use indigo `#4F46E5` + white, bold type, no stock photography. LawMatter/Comply.LM instead uses the client's dark navy/black tech look with cyan/teal accents (see `lawmatter/about.md`) — do not apply the indigo system to LawMatter assets.
- **Don't add invented people.** The AEO Intel/APAC owner trio (Nivedya, Shahana, Vaishnavi, Founder) and the LawMatter roles (Founder, Content–SEO, Paid–Social, Tech–GEO) are placeholders or real names as written. Never invent bios, credentials, metrics, or personal details for anyone.

## LawMatter-specific: the Claims Ledger gate

The newest and strictest plan. Its own §"Non-negotiables" is authoritative — read those four sections before producing anything for this client:

- **Nothing publishes that isn't in the Claims Ledger** (source URL + verification status required per claim). No row → no claim.
- **AUSTRAC facts come from AUSTRAC**, not a vendor blog and not us. Event facts come from `lawyersweekly.com.au`, not from us. A competitor's blog is never a source for a regulatory fact.
- **Speaker names get re-verified twice** (Day 24 and Day 29) and never published in the gap. `about.md` already records one named speaker who is absent from the organiser's current page.
- Owner is **Founder only** — one accountable owner per deliverable, no exceptions.

## Synchronization

When you change a plan, check the same change:

- Schedule post ↔ its own `Ownership Key`, phase boundaries, gate days and checkpoints (dates must stay internally consistent: Day 1 = start date, Day N = start + N − 1).
- AEO Intel/APAC changes touching a person's lane → the technical-roadmap post if it's engineering scope; there are no longer team pages to update (see the `/team/` gotcha).
- LawMatter plan changes ↔ `website/lawmatter/about.md` and the plan's Claims Ledger.
- Anything added to `website/content/` needs Hugo frontmatter to be publishable, and a `hugo --minify` build to verify.
- Never edit generated files under `website/public/` by hand.
