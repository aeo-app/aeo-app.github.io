# AGENTS.md

## What this is
This is a Hugo static site (not an application) hosting AEO/GEO client runbooks. Content lives in `website/content/`. **Never** commit or hand-edit HTML/build output. Deployed to `gh-pages` from CI.

## Build & Dev
- **Hugo version:** `0.165.0 extended` (must match CI)
- **Build from:** `website/` directory: `cd website && hugo --minify`
- **Build time:** <1s. PaperMod emits harmless deprecation WARNs (`.Language.LanguageDirection`/`.LanguageCode`) — ignore them.
- **BuildFuture gotcha:** Hugo has `buildFuture: false` by default. Posts with `date` > build date are silently dropped from the build (no error/warning). Keep plan post dates ≤ today (use start date in body). `hugo list all` shows dropped pages.
- **Local verification:** CI builds from clean checkout; don't trust orphaned files in `website/public/`. For live checks, test against `https://aeo-app.github.io/`.

## Git & CI
- **CI:** `.github/workflows/hugo-deploy.yml` builds with `peaceiris/actions-hugo@v3` (0.165.0 extended) and publishes `website/public/` to `gh-pages` via `GITHUB_TOKEN` on push to `main` (or manual dispatch). `concurrency.group: pages`, `cancel-in-progress: false`.
- **No build output on main:** `website/public/`, `website/resources/`, `website/.hugo_build.lock` are gitignored. Deploys only from CI.
- **DOCX export:** `pandoc` required. Output goes to `output/` (`*.docx` is gitignored; `output/` directory itself is tracked — don't commit non-DOCX files there).

## Repository layout
- `website/hugo.yaml` — Hugo config
- `website/content/posts/` — published plan posts (canonical exemplars live here)
- `website/content/team/` — currently **empty**; the `/team/` menu entry in `hugo.yaml` points to a non-existent page (404). Don't cite team pages.
- `website/lawmatter/about.md` — working material (no frontmatter, mixed with `[UNCONFIRMED]` tags). Do not treat as approved publishable copy.
- `.opencode/skills/` — OpenCode skills only (not `.github/skills/`)

## Skills (important paths)
- Skills live in `.opencode/skills/*.md`. Their exemplar references point to non-existent `projects/` paths; real exemplars are in `website/content/posts/`.
- **markdown-to-docx:** invoked with no path fails. Always pass explicit input file.
- **n-day-plan:** reads exemplars from `projects/` in its text but those files don't exist here — the actual reference posts are under `website/content/posts/`.

## Critical gotchas
- **Future dates vanish silently** (see BuildFuture above). This caused a LawMatter post to disappear when dated to Day 1 after build.
- **Team page dead:** menu links `/team/` but content missing — 404 on live site.
- **External repo reference:** `ploutos` paths in technical roadmap post refer to an external codebase (not in this repo). Don't claim to edit/verify that code here.
- **Plan tables are canonical:** Each client plan file's day-by-day table is the executable source (dates/cadence). The AEO Intel table wins over header text where they conflict.
- **Guardrails are strict:** Never fabricate statistics; resolve `[NOTE TO WRITER]`/`[STAT - VERIFY]` placeholders with real sourced data. Respect phase gates (e.g. AEO Intel Day 14, APAC Day 5, LawMatter Day 6/18/30). For Samudra, Day 10 blocks Phase 2 based on measured site state.
- **LawMatter Claims Ledger is the gate:** Nothing publishes that isn't in the Claims Ledger (needs source URL + verification). Platform link rules differ (IG caption links not clickable by default; GBP bans URLs/phone numbers in post body). 21 pre-existing broken `#` anchors exist in the LawMatter post — don't report them as your edits.
- **Samudra has reserved unsourced rows:** 32 sourced + 8 reserved (LGL-05–LGL-09, MKT-06–MKT-08). If a reserved row remains open on publish date, cut that sentence — don't invent sources.

## When editing
- Prefer executable sources of truth (config/scripts) over prose. If conflicting, trust the executable source.
- After editing content/frontmatter, run `cd website && hugo --minify` to verify it builds and pages aren't dropped.
- Never edit generated files under `website/public/` by hand.
- Keep changes minimal and repo-specific; don't add generic advice.