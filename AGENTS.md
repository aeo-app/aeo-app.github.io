# AGENTS.md — AEO Intel + APAC Relocation AEO/SEO/GEO Growth Operations

Single source of truth for how the two operations in this repo run (AEO Intel for `aeo-app.ai`, APAC Relocation for `apacrelocation.com`). On any conflict: **this file wins on operating rules; each plan file wins on its own dates and day-by-day assignments** (its table is the executable source — the cadence descriptions below are summaries).

## What this repo is

Not a classic software repo — there is no application code, test suite, or lint setup. It holds two runbooks + reusable skills, and it also builds a public Hugo site (`website/`) served at `https://aeo-app.github.io/` via GitHub Pages so the runbooks stay crawlable and citable — which is the point of the whole operation. No raw HTML ever gets committed or edited by hand; all site content lives as Markdown under `website/content/`.
1. **AEO Intel** — fixing `aeo-app.ai`'s AI/search visibility over 90 days: ~20 blog posts (10 tactical + 10 pillar), a repurposed LinkedIn/Instagram/Facebook cadence, and a weekly progress tracker that becomes a public case study at Day 90. Runs on a fixed clock starting **04 Sep 2026**.
2. **APAC Relocation** — fixing `apacrelocation.com`'s visibility in ChatGPT/Gemini/Perplexity/Claude/Copilot across 5 relocation keywords, as an **aggressive 30-day sprint** (Day 1 = 22 Sep → Day 30 = 21 Oct 2026) that also runs Instagram Reels + YouTube, Reddit/Quora forum distribution, and a backlink workstream. Same single-file anatomy and owner trio.

Do not redraft either calendar; look up the current date in the plan's schedule table and follow what is assigned that day.

## Files

| Path | What it is |
|---|---|
| `website/content/posts/aeo-intel-90-day-schedule-content-library.md` | The single in-repo source of truth for the whole operation — published as a Hugo post at `/posts/aeo-intel-90-day-schedule-content-library/`: day-by-day table, all 20 blog prompts, all carousel/poster prompts (Content Briefs section), ownership key, and a sources/references section. Has Hugo frontmatter; editing it changes the published site. |
| `.opencode/skills/n-day-plan/SKILL.md` | Generic, reusable skill for building a NEW plan of any length (`n-day-plan`, where N is an input — e.g. 90 days or a 30-day sprint) that is **not** either running operation below. Its output is a single self-contained file in the same anatomy as the `website/content/posts/aeo-intel-90-day-schedule-content-library.md` exemplar (header → ownership key → notes → day-by-day table → briefs & prompts → publishing notes → rhythm notes → supporting tasks → sources), named `<Brand>_<N>-Day_<Program>_Schedule_and_Content_Library.md`. Loads only from `.opencode/skills/` — don't move it back to a root `skills/` folder or opencode won't see it. Invoke with the day count as its argument, e.g. `n-day-plan 60`. |
| `.opencode/skills/markdown-to-docx/SKILL.md` | pandoc-based Markdown → DOCX export skill. |
| `website/content/posts/apac-relocation-30-day-schedule-content-library.md` | A SECOND, separate plan (published as a Hugo post at `/posts/apac-relocation-30-day-schedule-content-library/`), built via the n-day-plan skill at N=30 (built via the n-day-plan skill at N=30): fixing `apacrelocation.com`'s visibility in ChatGPT/Gemini/Perplexity/Claude/Copilot across 5 keywords (international movers Singapore, overseas moving company, global relocation services, international moving quotes, best international relocation companies in singapore). Run as an aggressive **30-day sprint**, not 90 — it layers Instagram Reels + YouTube, Reddit/Quora forum distribution, and a backlink workstream onto the same single-file anatomy and owner trio — Nivedya (blog), Shahana (social/media/carousels/video), Vaishnavi (technical/forums/backlinks). Same rules: trust the day-by-day table, real numbers or it doesn't publish, Day 5 gate. |
| `output/` | Generated document exports (DOCX from the markdown-to-docx skill). `.gitignore` ignores `*.docx`, so exports stay untracked. |
| `website/` | The Hugo site source: `content/` (the two runbook posts), `themes/PaperMod`, `hugo.yaml`. Build output `website/public/` is gitignored and never committed. |
| `website/content/team/` | Public role pages for Nivedya, Shahana, Vaishnavi, and Founder (`_index.md` + one page each: `nivedya.md`, `shahana.md`, `vaishnavi.md`, `founder.md`). Each page summarizes that person's responsibilities from the runbooks and carries their day-by-day task-target tables, linked back to both runbooks' schedule and brief anchors. Listed from `website/content/team/_index.md`, which is linked from the Team menu in `website/hugo.yaml`. |
| `.github/workflows/hugo-deploy.yml` | CI: on push to `main`, builds the Hugo site and publishes `website/public` to the `gh-pages` branch via the built-in `GITHUB_TOKEN` (the old SSH `AEO_APP_PAGES_DEPLOY_KEY` secret is broken and unused). `main` must never contain build output. |
| `AGENTS.md` (this file) | Operating roles, cadence, checkpoints, guardrails. |

**Companion files are absent:** `AGENTS.md` and the schedule originally referenced three more files — `aeo-app-ai_AEO-SEO-GEO_Growth_Report.md`, `AEO-Intel_Content_Prompts_Library.md`, and `AEO-Intel_90-Day_Day-by-Day_Activity_Calendar.xlsx`. None exist in this repo (the schedule file's own Supporting tasks note, ~line 345, confirms this). Don't invent their content; if asked, say they aren't in the repo. The merged schedule file already contains everything those would have held, so for content work it's a non-issue; the missing spreadsheet is the reason the Progress Tracker has nowhere to log results yet.

## Gotchas

- **Blog days are Wed (tactical) + Fri (pillar).** The contradiction lives *inside* the schedule file: its rhythm note and every brief header say "Tactical (Tue)" / "Pillar (Thu)", but the executable day-by-day table assigns tactical to Wednesdays and pillar to Fridays. Publish on the table's day, never the header's day.
- **Never fabricate statistics.** Every `[NOTE TO WRITER]` in the prompts must be resolved with real, sourced data before that post goes live; sources live at the bottom of the schedule file. AI engines down-weight caught fakes — this is the top guardrail.
- **Skills live only in `.opencode/skills/`.** Never restore or edit anything under `.github/skills/` — opencode only loads from `.opencode/skills/`.
- **Website deploys from `gh-pages`, never `main`.** The Hugo build output (`website/public/`) is force-pushed to the `gh-pages` branch by CI; never commit HTML or build output to `main`. All site content lives as Markdown in `website/content/`.
- **DOCX export** needs `pandoc` on PATH; output goes to `output/`. The root `.gitignore` excludes `*.docx` only — not the `output/` directory — so don't drop any non-DOCX file there expecting it to stay uncommitted.

## Document and profile synchronization

- Whenever anything is added to or changed in either runbook, review the affected pages in `website/content/team/` in the same change.
- Update a team page when the change affects that person's responsibilities, deliverables, ownership, cadence, approvals, or handoffs; update all affected pages when ownership is shared.
- Add a new team page when a new named employee is introduced, add it to `website/content/team/_index.md`, and add that page to `website/hugo.yaml` if the person should appear in the public Team menu.
- Each team profile carries day-by-day task tables linked into the runbook schedule and brief anchors; when a schedule's day assignments change, update the affected person's daily table in the same change.
- Do not add invented biographies, credentials, metrics, or personal details. Team pages are operational profiles derived from the runbooks, not employee resumes.
- Run a Hugo build after synchronized edits. Never edit generated files under `website/public/` by hand.

## Mission (two success conditions)

1. The site becomes crawlable and citable by AI engines. The site originally served almost no HTML to plain fetches (SPA-like), so **Phase 1 is entirely technical and gates everything else** — the reason for the Day 14 go/no-go.
2. Publishing cadence holds for 90 days without gaps. Consistency is the deliverable; a late Phase 1 beats an invisible Phase 2.

## Roles

Each person owns a lane; don't cross lanes without noting a handoff.

- **Nivedya — Blog.** Writes all 20 posts from the prompts in the schedule file. DoD per post: direct-answer first sentence, exact word count/structure from the prompt, FAQPage/Article schema, 2-3 internal blog + 2-3 product links, no fabricated stats, published on its assigned day (not batched late). Weekly: ~2 posts (Wed tactical, Fri pillar). Same lane for the APAC plan's blog.
- **Shahana — Carousels & Visuals.** All LinkedIn Company / Instagram / Facebook output, repurposing that week's blog post — no original topics outside the calendar without Founder sign-off. Visual system: indigo `#4F46E5` + white, bold type, no stock photography, minimal geometric accents. Weekly: 3 LI Company posts, 3-4 IG feed posts, ~5 IG stories, 2-3 Facebook posts — exact days in the schedule table (they are not all Mon/Wed/Fri; Saturdays carry the case-study posts).
- **Founder — placeholder, no real person assigned.** Product strategy, Phase 1 technical decisions, personal LinkedIn POV posts, weekly tracker review, Day 30/60/90 sign-off and Phase 2 go/no-go. If a real person is named for this role, replace "Founder" everywhere in one pass.
- **Vaishnavi — developer lane, Phase 1.** Shares ownership of the Day 1-14 technical tasks with the Founder (SSR/prerender, schema, llms.txt, robots.txt) — needs codebase access to `aeo-app.ai`; without them the foundation phase can't complete.

## Phases

- **Phase 1 — Days 1-14, Foundation (no publishing).** One technical task/day, verifiable at each step (fetch/curl test, indexing confirmation). Exit: Day 14 go/no-go — never start Phase 2 unless crawlability is confirmed.
- **Phase 2 — Days 15-84.** A weekly rhythm; follow the day-by-day table for exact assignments. Social content repurposes that week's blog, so blog timeliness gates Shahana's work.
- **Phase 3 — Days 85-90.** Re-run the AI-citation baseline test, pull analytics, refresh weak posts, write the Day 90 results (basis of the Week 9 capstone post).

## Weekly definition of done & checkpoints

A week is "on track" only if, by Sunday: both blog posts published on their assigned days; all 3 LI Company posts; Founder's LI posts; IG feed + stories held; Facebook reposts went out; Progress Tracker row filled in (even partially) by Friday. More than one slip → flag at the Friday review. Two consecutive missed weeks → reassess the cadence itself.

- **Friday weekly:** 15-min cadence review.
- **Day 30:** system-running check — indexing status, early follower growth, engagement trend. No traffic/AI-citation expectations yet.
- **Day 60:** first real signal — keyword rankings, traffic trend, re-run the AI-citation test vs. Day 1 baseline.
- **Day 90:** full report + decision to continue, adjust, or expand for the next cycle.

## Guardrails (hold regardless of deadline pressure)

- Never publish a fabricated statistic; resolve every `[NOTE TO WRITER]` with real data.
- Never skip Phase 1's exit check — publishing onto unreadable pages wastes the quarter.
- Keep visual brand consistent (indigo `#4F46E5` + white, no stock photography) on every asset Shahana produces.
- No mid-cycle topic invention — the 20-post arc is sequenced deliberately (technical → structuring → data/authority → APAC → measurement → case study → capstone); check the Content Library first.
- Log the tracker even in a bad week — a missing row is worse than an honest slow week; the tracker is the Day 90 case study.

## Skills

- For any plan that is **not** one of the two running operations (AEO Intel, APAC Relocation), use `.opencode/skills/n-day-plan/SKILL.md`, passing the desired duration as its argument (e.g. `n-day-plan 60`). It is generic; its output must mirror the single-file anatomy of either exemplar (`website/content/posts/aeo-intel-90-day-schedule-content-library.md`, or `website/content/posts/apac-relocation-30-day-schedule-content-library.md` for keyword/GEO plans — note that one runs as a 30-day sprint), swapping in the new plan's brands/channels/dates.
- For exporting Markdown content (schedules, reports, the content library) to Word, use `.opencode/skills/markdown-to-docx/SKILL.md`; it needs `pandoc` on PATH and writes to `output/`.