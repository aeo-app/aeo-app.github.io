# AGENTS.md

## What this is
A Hugo static site (not an application) publishing AEO/GEO client runbooks. Markdown in `website/content/posts/` is the product. Deployed to `gh-pages` from CI. Never hand-edit or commit `website/public/`.

Also read `.github/copilot-instructions.md` — it covers build/architecture/content conventions for non-OpenCode agents. This file holds the client-specific constraints.

## Build & verify
- Hugo **0.165.0 extended** must match CI. Local: `cd website && hugo --minify` (~1s). PaperMod's `.Language.LanguageDirection`/`.LanguageCode` deprecation WARNs are harmless.
- **No tests, linter, or typecheck exist.** A build + the checks below is the whole verification story.
- **Future-dated posts vanish silently.** `buildFuture` is unset (false) in `hugo.yaml`, so a post with `date` > build date is dropped with no error. Keep frontmatter dates ≤ today; put the real start date in the body. Detect drops: `cd website && hugo list all`.
- **In-page anchors:** Goldmark only auto-generates IDs for headings, so links into table rows (`#b3`, `#li-post-2`) break unless the row carries an explicit `<a id="..."></a>`. Put that anchor on **its own line, followed by a blank line, before the heading** — on the same line as the heading, CommonMark emits an HTML block and the `####` prints literally. Raw HTML renders because `markup.goldmark.renderer.unsafe: true` is set. Audit after edits: build, then diff `#anchor` targets against `id=` values in `website/public/posts/*/index.html`. **Baseline: 21 broken in the LawMatter post, 0 in the other four** — those 21 are pre-existing, not yours.
- `website/public/` may hold orphaned files from earlier builds. CI builds from a clean checkout, so verify against a fresh build or the live `https://aeo-app.github.io/`, not stale local output.

## Git & CI
- `.github/workflows/hugo-deploy.yml` builds with `peaceiris/actions-hugo@v3` (0.165.0 extended) and publishes `website/public/` to `gh-pages` on push to `main` or manual dispatch. `concurrency.group: pages`, `cancel-in-progress: false`, `fetch-depth: 0`. Default branch is `main`.
- **Trap:** `.gitignore` still carries root-level entries from a pre-Hugo manual publish (`/index.html`, `/posts/`, `/assets/`, `/tags/`, `/categories/`, `/404.html`, `/robots.txt`, `/sitemap.xml`, `/page/`). A file created at repo root under one of those names is silently ignored — `git status` won't show it. Build output only ever goes to `website/public/`, which is ignored.
- **`output/` is tracked and already holds non-DOCX artifacts** (`Samudra_Blog_Content_List.txt`, `lawmatter-46-day-plan-summary-for-signoff.md`). Only `*.docx` is ignored, so DOCX exports are invisible to git but other working files there are not. Don't delete existing files in `output/` on the assumption they're leftovers.
- **PaperMod is vendored into git** under `website/themes/PaperMod/` (no `.gitmodules`). Don't add or upgrade it as a submodule.
- **pandoc** exports: `pandoc "<in.md>" --from=gfm --to=docx --output="output/<name>.docx"`, then `test -s` it.

## Layout & skills
- `website/hugo.yaml` — config, menus, taxonomies. `website/assets/css/extended/custom.css` — site styling (9 lines; nav width 1200px, table cell padding).
- `website/content/team/` is an **empty dir** — git never tracked it, no `team/` in build output, and the `/team/` menu entry 404s on the live site. Don't cite or link team pages.
- **`website/lawmatter/about.md` is gitignored** (`.gitignore:20`) and untracked. The LawMatter post cites it as the source for client-supplied facts and prohibitions, but a clean checkout/CI clone won't have it. It's working material with `[UNCONFIRMED]`/`[INFERRED]` evidence tags, never publishable copy — and where the post says "`about.md` is out of date", trust the post.
- `n-day-plan` writes LawMatter-grade runbooks: it measures a baseline before planning, runs a red/blue adversarial round whose rulings land in a Decision Log, and emits 26 mandatory sections (ledger, publication gate, funnel maths, UTMs, pre-approved gate cut lists, standing playbooks). Two profiles — `aeo-geo` and `generic`; non-applicable sections get an explicit fallback, never a silent drop. It asks where to write and defaults to `website/content/posts/`.
- `markdown-to-docx` requires an explicit input path; it fails rather than guessing one.

## Content guardrails (client-specific, verified)
- **Plan tables are canonical.** The day-by-day table in each post is the executable source for dates and cadence; where header prose conflicts, the table wins.
- **Never fabricate statistics.** Resolve every `[NOTE TO WRITER]` / `[STAT - VERIFY]` placeholder with real sourced data or label it unknown. Placeholders currently live in all four runbook posts (4–11 each).
- **Phase gates are real blockers, not labels:** AEO Intel Day 14 (go/no-go for Phase 2), APAC Relocation Day 5, LawMatter Days 6/18/30, Samudra Day 10 (site-state measurement can block Phase 2).
- **LawMatter:** the Claims Ledger is the publication gate — every factual claim needs a source URL and verified status, otherwise it is `Prohibited`. Platform rules: Instagram caption links aren't clickable by default (first comment instead); GBP post bodies must not contain URLs or phone numbers. Reserved/banned items already recorded include the 7-year retention period, the 31 Mar 2027 report date, and any penalty amount — don't reintroduce them.
- **Samudra:** the Source Ledger has 32 sourced rows plus 8 reserved ones (`LGL-05`–`LGL-09`, `MKT-06`–`MKT-08`). If a reserved row is still open at publish time, cut the sentence — don't invent a source. This post is written on **Day numbers, not calendar dates**, on purpose (start-date-independent); other posts use dates.
- **`ploutos`** paths in `aeo-app-technical-roadmap-...md` describe an external FastAPI/React/AWS codebase that is not in this repo. You cannot edit or verify that code here.

## When editing
- Edit Markdown, `hugo.yaml`, or `website/assets/` — nothing else. After any content or frontmatter change, `cd website && hugo --minify`, confirm the post still appears in `hugo list all`, and re-check anchors if you touched table rows or headings.