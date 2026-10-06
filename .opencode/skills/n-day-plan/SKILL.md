---
name: n-day-plan
description: "Create an N-day operating plan — a day-by-day schedule with owners, gates, ledgers and one detailed brief per deliverable, written so a stranger can execute it without asking a question. Use for growth sprints, launches, migrations, campaigns and AEO/GEO client runbooks. Runs an adversarial red/blue review and records every contested decision in a Decision Log."
argument-hint: "[N] [brand/goal]"
user-invocable: true
---

# N-Day Operating Plan

Produce **one self-contained Markdown document** that an operator who has never met the client can execute end to end: a schedule that is the executable source of truth, and one detailed brief behind every cell that has work in it.

This skill writes plans at the standard of the exemplars in `website/content/posts/`:
- `lawmatter-46-day-aeo-geo-summit-schedule-content-library.md` — the full AEO/GEO profile: 13-channel table, Claims Ledger, compliance gate, funnel maths, UTM convention, pre-approved gate cut lists, 3,271 lines
- `samis-60-day-aeo-geo-schedule-content-library.md` — the same anatomy plus a measured technical baseline, a reserved-row register and a `CONFLICT` row
- `apac-relocation-30-day-schedule-content-library.md` — N=30, keyword/GEO-led
- `aeo-intel-90-day-schedule-content-library.md` — N=90, lighter table, Day-number convention

Read at least one of those before writing. They are the format authority; this file is the method.

---

## 0. Three rules that come before the method

1. **Measure before you plan.** Never write a target, a baseline, a competitor number, an audience size or a conversion rate you did not fetch, `curl`, or read on a named page. If it was not measured, it is `[SQUARE-BRACKET PLACEHOLDER]` or `TBD — Day N`, never a plausible figure. The Samudra exemplar opens with a full technical baseline measured by `curl` because that is what made every later decision arguable.
2. **Inventing is a defect, not a rounding error.** One unsourced number in a published asset is an incident: pull the asset, find every other asset sharing the number, correct them all, log it in the close report. A placeholder must never be filled with a guess, softened with "up to", or attributed vaguely.
3. **The schedule table is canonical.** If prose and table disagree about a date, an owner or a cadence, the table wins and the prose gets fixed.

---

## 1. Inputs

Collect what is available; ask only for gaps that materially change the plan. Assume and label the rest.

| Input | Notes |
|---|---|
| **N** | The plan's spine. From the user: "30-day sprint" → 30, "90-day" → 90, "quarter" → 90/91. Unspecified → **90**. Confirm it before writing: every phase boundary, gate, checkpoint, target and filename derives from N. |
| **Goal** | One outcome, phrased as a change someone else can observe. |
| **Start date** | The user's, or propose one and label it. |
| **Starting point** | What is true today, how it was measured, constraints, risks, blockers. |
| **People and roles** | Names, not titles. One accountable owner per deliverable. Where a task needs two, the first name owns. |
| **Workstreams** | The major lines of work → the table's channel columns. |
| **Cadence** | Recurring daily/weekly/monthly activity, and the publishing rhythm it implies. |
| **Resources** | Budget, tools, access, dependencies, source material. |
| **Measures** | Baseline, leading indicators, outcome metrics, targets. **Targets are targets, not forecasts.** |
| **Operating constraints** | Capacity, blackout dates, approvals, compliance, legal exposure, quality requirements. |
| **Fixed dates** | An event, launch, renewal, deadline. Non-movable dates reshape the whole ramp. |
| **Audience and buyer** | Who actually buys, where they are, what they already believe. Drives channel ranking. |

---

## 2. Pick the profile

Ask which one, or infer it and say which you used.

- **`aeo-geo` — the marketing runbook profile.** Everything in §6 applies at full weight: the ledger, the publication gate, funnel maths, UTMs, platform mechanics, standing playbooks, the full channel table. Use for a client-facing growth, campaign or visibility plan.
- **`generic` — the operating plan profile.** Same anatomy, one column per workstream instead of per channel. §6's marketing sections are **not dropped silently** — each is either filled with the equivalent for the domain (ledger → decision/assumption register; compliance gate → pre-merge/pre-launch checklist; funnel maths → capacity maths; UTMs → source-of-truth and reporting convention) or marked `**Not applicable to this plan — and here is why.**`

Never mix the two halfway without saying so in the header notes.

---

## 3. Research pass — do this before drafting the table

Spend real effort here; the plan's credibility is the research.

**If you have web access, use it.** Fetch the live site, the platform docs, the primary sources, the regulator, the competitor's own page. Record for each: the URL, what it returned, the date. That record becomes the Sources section and the ledger rows.

**The minimum research set for a marketing plan:**

| Research | Method | Feeds |
|---|---|---|
| Technical baseline | `curl -I`, view-source, per-route byte counts, user-agent swap, `robots.txt`, `sitemap.xml`, `llms.txt`, schema presence, analytics IDs | The foundation phase, the gate checks, whether a crawlability phase is needed at all |
| Asset inventory reconciliation | Count the same asset three ways (sitemap vs. on-page index vs. `llms.txt`) | Discrepancies are high-leverage findings |
| Primary source pass | Regulator, standard-setter, official register — for every regulated claim | Ledger rows, compliance gate |
| Commercial source pass | The client's own pricing, product and about pages | Ledger rows, positioning |
| Third-party entity pass | The event organiser, the association, the trade press | Event and partner claims |
| Audience reality | Where the buyer actually is, in their words | Channel ranking, which channels get a row |
| Competitor pass | Competitor's **own** public page, quoted, with a link and a date | Attributed comparisons only |
| Search & AI visibility baseline | `site:` query; a fixed prompt panel across the named engines, same prompts on Day 1 and at the close | The primary outcome metric |
| Conversion instrumentation | Which tags, pixels and conversion events actually fire today | The tracking brief, the budget-shift thresholds |
| Capacity | Real hours per owner per week | Rest days, the cut list |

**Where a source disagrees with another source, do not pick a winner silently.** Write a `CONFLICT` row into the ledger with both figures, the tell that explains them, and the rule for what may be published. The Samudra exemplar does this for a bunker statistic whose two figures measure different fuel scopes.

**Where one source underwrites many rows, name the concentration risk.** "All four statutory rows trace to one law-firm commentary" is a finding, and it creates a research task in the plan.

**If you cannot verify something, reserve a row for it** (§8) rather than dropping the claim or inventing it.

---

## 4. The adversarial round

Run this **before** drafting the table. A plan written without it inherits whatever the first draft assumed.

### The personas

Six roles. You play all of them; state which one is speaking whenever a persona's objection changes the draft.

| Persona | Mandate | The question they always ask |
|---|---|---|
| 🔴 **RED — The Destroyer** | Assume the plan fails and name how. Finds the unsourced number, the compliance exposure, the dependency that collapses on Day 12, the promise the work cannot keep. **Holds a veto on anything fabricated or unlawful.** | "What exactly breaks, on which day, and who pays for it?" |
| 🔵 **BLUE — The Operator** | Make the metric move with the least waste. Argues sequencing, the cheapest leverage, what to double, what to cut. **Must answer Red with a mechanism, never with confidence.** | "What is the cheapest thing that produces this result, and why is it first?" |
| 👤 **THE BUYER** | Reads the planned copy cold, as the real buyer, with no briefing. | "Does this answer my actual question, and do I believe it?" |
| 🛠 **THE OPERATOR ON THE DAY** | Four hours, no context, Day 19. | "Can I execute this brief without asking anyone a question?" |
| 🔍 **THE AUDITOR** | Every number, name, date and superlative back to a source URL and a retrieval date, or it is cut. | "Show me the row, or cut the sentence." |
| ⚖️ **THE REFEREE** | You, the planner. Rules. Accepts Red's objection or states the **mechanism** that overcomes it — a mechanism is a process, a deadline, a cut or a source, never a feeling. Records the outcome. | "Red is right, or here is exactly what defeats Red." |

### The protocol

For each of the load-bearing decisions below, in this order:

1. **RED opens** with a specific, named objection. Not "this is risky" — "the paid budget buys traffic to a page that has no `Event` schema until Day 3, so Day 7 spend lands on a page that cannot rank."
2. **BLUE replies** with a mechanism and a cost. Not "it'll be fine" — "hold paid to zero until the Day 6 gate passes; the site is verified 200 with server-rendered HTML, so the risk is the event page, not the domain."
3. **RED re-tests.** Does the mechanism actually close the objection? If not, it stands.
4. **REFEREE rules**: accept, or state the mechanism that overcomes it, or send it back as a research task with a deadline and an owner.
5. **Record it** in the Decision Log.

Run at least these decisions. Each one is a decision a real plan got wrong:

1. **Length and start date.** Is N long enough for the work to be *retrievable*, not merely produced? Is there a fixed date forcing the shape?
2. **Foundation length.** Default is ~16% of N. Deviate only with a stated measurement of the current state, and say which way and why. "The site does not need unblocking" is the strongest justification in the exemplars.
3. **Which channels earn a row.** If the client asked for a channel that cannot reach their buyer, keep it, rank it last, and say plainly why — do not pad it with filler to hit a number, and do not silently drop it.
4. **Paid budget.** Spend, hold, or hold-until-gated, and what specific evidence flips it.
5. **The highest-risk asset.** Does the comparison/differentiation asset ship at all, and what sign-off gates it.
6. **Cadence versus capacity.** State the rest days. Say plainly when the plan is tight.
7. **Objective ranking.** Which outcome wins when two outcomes want the same hour.
8. **Peak placement.** Where the pressure lands relative to any fixed date.
9. **Follow-up length.** How much of the value sits after the deadline.
10. **Measurement.** What is the baseline, what is the target, and what would make the close report say "this did not work".

### The Decision Log

Mandatory section in the artifact. This is the part that makes the plan reviewable:

```
| # | Decision | Red's objection | Blue's reply | Ruling | Reverses if |
|---|---|---|---|---|---|
| D1 | Is the client a vendor, a regulated practitioner, or both? | Two sources imply two regimes; picking one risks a conduct breach | Apply the **strictest combination** of both to every asset | Strictest combination. Costs one lane; removes a class of legal exposure | Client confirms a practising certificate, or counsel rules |
| D2 | Paid spend from Day 1 or after the gate? | ... | ... | ... | ... |
```

Also record decisions the plan makes on the client's behalf in a **Decisions, assumptions and open questions** section: each decision `D#` with its default applied and what would change it; each assumption labelled as a hypothesis to be tested at a named checkpoint.

---

## 5. Structure — scale to N, do not copy 14/84/90

| Phase | Length | Contains |
|---|---|---|
| **Foundation** | ~16% of N, rounded, minimum 3 days | Baseline measurement, access, research, blockers, the ledger, instrumentation. Known: N=90 → Days 1–14; N=30 → Days 1–5. |
| **Execution** | The middle | A repeatable weekly rhythm producing the outputs and recording results. |
| **Evaluation** | ~7% of N, minimum 4 days | Re-measure the baseline identically, compare to target, decide. N=90 → Days 85–90; N=30 → Days 27–30. |

Scale proportionally and state the numbers you chose. Deviating from 16% is allowed and sometimes right — **justify it with a measurement of the current state, and name what the deviation costs.**

Every plan needs: a **foundation go/no-go gate**, a **mid-sprint checkpoint**, and a **final review** that produces a decision rather than a status update. Compress for N ≤ 30: gate at the foundation boundary, one mid-sprint spot-check, the final review.

**Capacity is a real input.** For every owner, count the load-bearing days they own and compare it to their actual weekly hours. If it does not fit, **cut in the plan and say what was cut and what it costs** — do not ship a schedule that silently assumes overtime. Name the rest days explicitly.

---

## 6. Anatomy — mandatory sections, each with a stated fallback

Produce these in this order. The fallback column is what you do when a section does not apply — **always write the fallback, never silently omit.**

| § | Section | AEO/GEO profile | Generic fallback |
|---|---|---|---|
| 1 | **What this document is** | Three things it contains: the strategy, the schedule as executable truth, the library. Who it is for, how to read it, which sections override which. | Same |
| 2 | **The goal, as an outcome** | One paragraph, then a **ranked** outcome table: what we want, how it is measured, **where the number comes from**. The ranking decides every hour of conflict. | Same, ranked by impact |
| 3 | **Glossary** | Every abbreviation, defined once. Include your own channel and framework terms. | Same |
| 4 | **How every brief is written** | The 7-part brief contract (§7). | Same |
| 5 | **Where the information comes from** | A source-type table: direct verification / primary source / ledger / **operating assumption**, and how each is marked in the text. The last row is what stops an assumption reading as a fact. | Rename rows to your domain; keep the assumption row |
| 6 | **What this plan will not do** | 4–6 explicit non-goals, each tied to a named failure it prevents. | Same |
| 7 | **Click-through library** | Every deliverable in one list, each an in-file link to its own brief, grouped by phase/week, with the override sections listed first. | Same, grouped by workstream |
| 8 | **Ownership** | Per owner: lane, scope, **Definition of Done**, and what they own alone. One accountable owner per deliverable, no exceptions. Placeholders named as placeholders. | Same |
| 9 | **Why this plan is shaped this way** | One bullet per structural choice a reader would question: why this length, why this start, why these channels, why this order, why the ramp is longer than the brief asked. **The reasoning, not the instruction, is the point.** | Same |
| 10 | **Decision Log** | The adversarial record (§4). | Same |
| 11 | **Decisions, assumptions, open questions** | `D#` decisions with defaults · assumptions labelled as hypotheses with a test date · an **open-question table**: `#`, question, **answer needed by Day N**, **what it blocks**. "None of these may be invented. If the answer misses its deadline, the dependent asset ships without the claim." | Same |
| 12 | **Verification rules** | 8–12 numbered rules. Each names **the specific failure it exists to prevent** — that is what makes them rules rather than style. | Same |
| 13 | **The ledger** | §8. The publication gate: no row, no claim. | Decision/assumption/constraint register with the same columns |
| 14 | **Publication gate** | A checkbox table: `Check`, `What the rule requires`, `Evidence of sign-off`. Rule: an asset that has not passed it is **drafted, not scheduled**. "Any unticked box stops the asset. No exceptions, no event-day exceptions." | Pre-merge/pre-launch/release checklist, same 3 columns |
| 15 | **Capacity / funnel maths** | Stage-by-stage arithmetic with **blank client inputs**. "A funnel built on invented conversion rates is a lie with a percentage sign on it." Missing numbers are gate conditions. | Capacity maths: hours available vs. hours required per owner per week |
| 16 | **Tracking convention** | UTM naming pattern in a code block, conversion events to configure, attribution window, **one named source of truth for reporting** and who updates it when. | Source-of-truth and reporting convention |
| 17 | **The day-by-day schedule** | §9. | Same, workstream columns |
| 18 | **How to read every brief below** | Restate the contract. "If a brief does not follow that shape, that is a problem with the brief, not with you." Add the copy-block rule. | Same |
| 19 | **Standing playbooks** | Per-channel templates that hold for every asset: brand rules, the answer-first rule, an asset checklist with *why* and *where to verify* per item, per-channel templates, the entity/authority rule, the outreach rule. State that behavioural claims here are **assumptions to test**, not research findings. | Per-workstream standing rules |
| 20 | **Phase and week briefs** | §7. | Same |
| 21 | **Gates and checkpoints** | §9. | Same |
| 22 | **Notes before publishing** | 10–14 numbered non-negotiables, each one line, written to be read on the day. | Same |
| 23 | **Platform publishing notes** | The mechanics that differ per surface and cause the classic failures — especially **link mechanics**. | Per-system mechanics |
| 24 | **Rhythm notes** | Why the phases fall where they do, why the foundation is its real length, why the peak is where it is, why the follow-up is its real length, and **an honest feasibility paragraph** naming the cuts to apply if the team is smaller. | Same |
| 25 | **Supporting tasks** | Before Day 1 (Day 0) · accounts, logins and access with the **verified state** of each · tracker and measurement with the exact file/folder structure · **in parallel, not in the table** — work that runs alongside the calendar and is expensive to start late. | Same |
| 26 | **Sources** | Grouped: the client's own site (what each URL returned, with the date) · third parties/primary sources · **blocked until verified** · competitor material, attribution only · internal · the compliance references, each flagged *a pointer to a rule, not advice*. | Same, adapted |

**Length floor:** the smallest defensible artifact is a complete §1–§26 with real content. A thin section that says "TBD" is a defect — either fill it, or write the fallback sentence explaining why it does not apply.

---

## 7. The brief contract

Every deliverable brief has these seven parts, in this order, every time:

1. **What this is.** One paragraph: the asset, the channel, the funnel stage or audience question it serves.
2. **Why we are doing it.** The intention, and the reasoning for choosing *this* asset over another.
3. **Why it matters.** The consequence of skipping it or doing it badly. Named failure, not vibes.
4. **What result we expect.** The observable outcome, so the close report can say whether it worked.
5. **Where the facts come from.** Every claim's source, every figure that must be confirmed, and every blocked claim that must not appear.
6. **The content, the copy, or the table.** The deliverable itself. **Ready-to-use copy in a block quote** — that is the wording to publish, not a suggestion. Shape it to the medium: numbered steps for processes, slide-by-slide for carousels, shot-by-shot with timings for video, line-by-line for posts, the actual body for emails, a design prompt for visuals.
7. **Before you publish — checklist.** Conditions that must all be true, written so a reviewer ticks them one by one. If any box is unticked, it does not publish.

**Header line, immediately under the heading:** `Owner: <name>. Day <N>, <Weekday> <date>.` plus a target (word count, duration, slide count) where one exists.

**Add, where the medium needs it:** a `[NOTE TO WRITER — mandatory before publish]` line naming the real data required and the source it must come from; subject-line A/B variants with the send split and the day the winner is picked; a note on where a risky statement came from and what it does not claim.

**The placeholder rule, stated in §1 and obeyed everywhere:** every `[LIKE THIS]` is an unknown, a needed fact, or an approval. Fill it with an approved real value or delete the sentence containing it. **Never replace a placeholder with a plausible-sounding guess.** A sentence you cannot complete is a sentence you delete.

---

## 8. The ledger — the mechanism that makes "don't make things up" checkable

A principle is not a control. The ledger is.

**Columns:** `Claim ID | Claim (exact wording to publish) | Source URL | Source owner | Retrieved | Status | Blocks if unverified`

**Status values:** `VERIFIED` (fetched it; the number is exactly as stated) · `VERIFIED-QUALIFIED` (true, but only publishable with the stated qualifier attached — keep the source's own hedges) · `UNVERIFIED` (do not publish) · `CONFLICT` (sources disagree) · `PROHIBITED` (never, no source can fix it) · `WITHDRAWN`.

**Rules:**

- **No row, no publish.** Not "publish then check" — the rule is inverted.
- **Cite the primary source.** A regulator beats a law-firm commentary, which beats a vendor blog. A competitor's blog is never a source for a regulatory fact.
- **A competitor's numbers are their claims.** Quotable with attribution and a link. Never restated as fact, never used as a comparison baseline, never a characterisation of their quality.
- **Statutory citations are the highest-risk row type.** Flag them and require the statute, not the commentary.
- **Time-sensitive facts decay.** Name the re-verification days in the row and in the schedule.
- **A blocked claim is blocked in every asset.** Name the assets it blocks. Deleting one sentence from one post is not compliance.

**Reserved rows.** Claims later briefs depend on that you could not source. Reserve an ID, name the source required and where it must come from, name what it blocks, and write the **fallback behaviour if it cannot be sourced** — usually "cut the sentence", sometimes "publish the method without the number". State plainly that **if the row is still open on the publish date, the sentence is cut and the asset ships without it — that is the intended behaviour, not a failure.** Put a deadline on closing them in the foundation: research closed on Day 8 is a different piece of work from research closed the day before publish.

---

## 9. The schedule and the gates

### The table

`Day | Date | Wd | Assigned To | <one column per workstream or channel> | Notes / Checkpoint`

- One row per day for the whole period, Day 1 … Day N.
- **Every channel cell that has work holds a link to that deliverable's brief.** A cell with no link is empty; a cell with an unlinked instruction is a defect.
- **Notes / Checkpoint** carries gates, deadlines, open-question due dates, rest days, and the reason for the day's shape.
- Explain the weighting above the table: why this content is early and that pressure is late, why weekends are light.
- Give the schedule table its own explicit anchor (below).
- **Day numbers, not dates**, when the start date may move — state the conversion rule once (`date of Day N = agreed Day 1 + (N − 1)`) and derive only the weekday column. Otherwise use real dates.

### The gate

A gate is a real blocker. If it cannot stop the work, it is a label; delete it.

- **Owner and date** on the line under the heading.
- **A numbered checklist with a "verified how" column** — every check names a tool or page a named owner can open. Not a matter of opinion. A `☐` per row.
- **What this gate can stop, and why it is worth stopping there** — the cost of deciding late.
- **The GO path:** what happens the next morning, and where the result is recorded.
- **The NO-GO path, pre-approved and ordered.** This is the part people skip, and skipping it is why gates fail in practice. **A gate that needs a debate at midnight will be made badly.** So the cut list is agreed in advance and *executes* rather than deliberates: what comes out, in what order, what the cut is worth, and **what is never cut.** **The plan continues smaller rather than slipping.**
- **The contingency branch** for the catastrophic check (e.g. if line 1 fails, the ramp compresses and paid does not start).

Put the gate in the table on its day, linked.

---

## 10. Lanes, repurposing and platform mechanics

- **Rank channels against the buyer, and schedule what the client asked for anyway** — ranked last, with the honest reason stated ("an Instagram Reel will not acquire that buyer"). Never pad a lane to hit a number: **post nothing rather than filler.**
- **A repurposing lane** takes the week's strongest ledger-verified asset, re-cuts it, and posts. State the cadence, the re-cut routine as numbered steps, and a worked example: design prompt, per-platform captions (they are **not** the same text), hashtags, alt text, on-card footer, log fields. **A new claim never originates in a repurposing lane** — the claim discipline is inherited from the source. If a re-cut needs a claim the source lacks, it is cut, not softened.
- **Link mechanics are where these channels actually fail.** Check the real rule per surface and put it in a table (`Is a caption link clickable?` / `Where does the link go?` / `Reach consequence`). Known: Instagram caption URLs are not tappable (bio link or Stories sticker); LinkedIn links go in the first comment on both pages; Facebook links go in the post body; Google Business Profile takes links in the CTA button field only, never the body, and bans phone numbers there. **Verify current policy before asserting it.**
- **Conditional surfaces stay conditional.** If a profile, licence, address or permission is required and unconfirmed, it is an open question, and the answer "no" **drops that subsection rather than triggering an improvised substitute.**
- **Transcripts are the asset** for any video, in an AI-citation plan: upload corrected captions, never auto-generated.
- **Never schedule a time-critical asset far ahead.** Event and pricing facts decay. Name the re-verification window relative to the send.
- **Timezone and daylight-saving traps** belong in the brief that sends: check the send window against the audience's zone, and check the DST change date for the period.
- **One CTA per asset**, and name which CTA by asset type.

---

## 11. Publishing into this Hugo site

Default output is `website/content/posts/<brand>-<N>-day-<program>-schedule-content-library.md`. **Ask where to write before creating the file**, and confirm before writing into the site if the user named another path.

- **Ask where to write.** If the answer is "publish it", use `website/content/posts/`, add Hugo frontmatter (`title`, `date`, `lastmod`, `tags`, `categories`, `summary`), and run the build checks in §12. If the answer is "draft it for review", write to `output/` instead — it is the working-artifact directory, and non-DOCX files there are tracked, so name the file for the client.
- **`date` must be ≤ today.** `buildFuture` is unset, so a future-dated post is dropped from the build with no error and no warning. The plan's own start date belongs in the body, not the frontmatter. Confirm with `hugo list all`.
- **Anchors.** Goldmark only auto-generates IDs for headings, so any link into a **table row** needs an explicit anchor. Put it **on its own line, followed by a blank line, before the heading** — on the same line as the heading, CommonMark emits an HTML block and the `####` prints literally. Raw HTML renders because `markup.goldmark.renderer.unsafe: true` is set.

  ```markdown
  <a id="b1"></a>

  #### BLOG #1 (pillar) — "…"
  ```

  Give the schedule table and every cross-cutting section an explicit anchor too, so "back to table" links resolve in any renderer rather than relying on a host's heading slugs.
- **Never hand-edit `website/public/`.** CI builds from a clean checkout; a local build may hold orphaned files from earlier runs.

---

## 12. QA pass — run before you declare the artifact done

Build and structural checks, when the artifact lands in the site:

```sh
cd website && hugo --minify            # must build with no ERRORs; the PaperMod deprecation WARNs are harmless
cd website && hugo list all            # the post must be listed; a missing post is usually a future date
```

Then audit anchors against the built HTML (minified attributes are unquoted, hence the two regex groups):

```sh
python3 - <<'EOF'
import re, pathlib
for p in sorted(pathlib.Path("public/posts").glob("*/index.html")):
    h = p.read_text(encoding="utf-8")
    ids  = {m[1] for m in re.findall(r'\bid=("?)([A-Za-z0-9_-]+)\1', h)}
    anc  = {m[1] for m in re.findall(r'href=("?)#([A-Za-z0-9_-]+)\1', h)}
    bad  = sorted(anc - ids)
    print(f"{p.parent.name}: {len(bad)} broken / {len(anc)} anchors {bad if bad else ''}")
EOF
```

**Then check, by reading your own artifact:**

- [ ] N came from the user (or defaulted to 90 and labelled); every phase boundary, gate and filename uses N consistently.
- [ ] **Dates, weekdays and day numbers are internally consistent** (Day 1 = start, Day N = start + N − 1; weekend and DST rows correct).
- [ ] Every measurable outcome is ranked, and each names **where its number comes from** — a measurement, a client input, or a ledger row.
- [ ] **Every number, name and date in the artifact traces to a source URL + retrieval date, or is `TBD`/bracketed.** Search the artifact for unsourced figures; this is the check that matters most.
- [ ] Every placeholder names the real data required and the source it must come from.
- [ ] Every ledger row has a status; every `UNVERIFIED`/`PROHIBITED`/`CONFLICT` row names what it blocks.
- [ ] Reserved rows each carry a deadline and a fallback behaviour.
- [ ] Every scheduled deliverable links to a brief; every brief has exactly one accountable owner, a day, and a Definition of Done.
- [ ] Every brief has all 7 contract parts, and the copy blocks are real publishable copy rather than descriptions of copy.
- [ ] Every gate has a checklist with a "verified how" column, a GO path, and a **pre-approved ordered cut list naming what is never cut**.
- [ ] Rest days and capacity limits are stated honestly, and the cut list for a smaller team exists.
- [ ] The Decision Log has an entry for each of the 10 contested decisions, with a ruling and a reversal condition.
- [ ] Every section in §6 is present; non-applicable sections carry their fallback sentence.
- [ ] Sources are grouped, dated, and include the blocked list and the compliance references flagged as pointers, not advice.
- [ ] No anchor is on the same line as a heading; the anchor audit reports no new breakage.

---

## 13. Anti-patterns

Each of these is a way the exemplars got better.

- **Planning before measuring.** A plan written without a fetched baseline argues from adjectives. Measure first; the baseline is what makes every later trade-off arguable.
- **Inventing a conversion rate to complete a funnel.** Leave the inputs blank and make filling them a gate condition.
- **A gate with no pre-approved NO-GO path.** It becomes a status meeting, and the plan slips anyway.
- **Padding a channel to justify its existence.** Rank it, schedule it honestly, or state why it is there. Filler destroys the credibility of the whole document.
- **Publishing an assumption in the voice of a fact.** Label it, give it a test date, and test it at a named checkpoint.
- **Softening a blocked claim into a guess** ("up to", "around", "industry data suggests"). Cut the sentence.
- **Picking a winner between two conflicting sources without recording the conflict.** Ledger row, both figures, the rule.
- **A brief that says "create content about X".** The operator should never need to ask what to write.
- **Treating the follow-up as optional.** It is routinely where the value is, and routinely the first thing cut.
- **Concluding with "it depends".** The close report names a decision, with evidence, and a next-N list.
- **Shipping a schedule that silently assumes overtime.** Cut in the plan and price the cut.

---

## Output

Return: the file path written, the **Decision Log in full** (it is the part the user most needs to review), the sections that carried a fallback or a placeholder rather than content, and the QA results. Name anything you could not verify and where it is reserved in the ledger.
