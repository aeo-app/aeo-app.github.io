# Copilot instructions

## Build and checks

- This repository is a Hugo static site. Use Hugo `0.165.0` extended, matching CI.
- Build from the site root: `cd website && hugo --minify`.
- There is no configured test suite, per-post test runner, or linter. To check whether a specific post is recognized by Hugo, run `cd website && hugo list all | grep -F 'content/posts/<post-file>.md'`; then build the site to validate rendering.
- Hugo excludes future-dated pages by default without warning. When a post is missing, check its frontmatter date with `cd website && hugo list all`.
- DOCX export requires Pandoc and an explicit Markdown input path: `pandoc "<input.md>" --from=gfm --to=docx --output="output/<name>.docx"`. DOCX files are ignored by Git; do not put unrelated files in `output/`.

## Architecture

- This is a content-first Hugo site, not an application. Markdown in `website/content/posts/` is the published source; `website/hugo.yaml` configures PaperMod, menus, taxonomies, and Markdown rendering, while `website/assets/css/extended/custom.css` contains site-specific styling.
- `.github/workflows/hugo-deploy.yml` builds with Hugo 0.165.0 extended and publishes `website/public/` to the `gh-pages` branch. Build output and Hugo caches are generated and ignored; never hand-edit or commit them.
- Most long-form posts are operational client runbooks: their day-by-day tables define the schedule and cadence, with linked briefs supplying execution details. Treat the table as the executable source of truth when prose conflicts with it.
- `AGENTS.md` contains client-specific source, phase-gate, and publishing constraints. Read it before editing a client plan. OpenCode workflows are in `.opencode/skills/`; the exemplars they cite are the posts in `website/content/posts/`.

## Content conventions and guardrails

- Keep a plan post's frontmatter date on or before the intended build date so Hugo includes it. Verify content changes with the site build; do not rely on pre-existing files in `website/public/` as evidence of a clean build.
- Never invent statistics or publish unresolved `[NOTE TO WRITER]`, `[STAT - VERIFY]`, or equivalent placeholders. Use verifiable source material; mark unknowns rather than guessing.
- For LawMatter, the Claims Ledger is the publication gate: every factual claim needs a source URL and verified status. Respect the plan's phase gates and platform-specific rules; for example, Instagram caption links are not clickable by default, and Google Business Profile post bodies must not contain URLs or phone numbers.
- Respect each plan's operational gates instead of treating phase transitions as automatic. In particular, AEO Intel uses its Day 14 gate, APAC Relocation its Day 5 gate, LawMatter its Day 6/18/30 gates, and Samudra's Day 10 site-state measurement can block Phase 2. Samudra's reserved unsourced ledger rows must be removed from publishable copy if still unresolved by the publish date.
- Keep edits in Markdown, Hugo configuration, or source assets as appropriate. Do not create or modify generated HTML by hand.
