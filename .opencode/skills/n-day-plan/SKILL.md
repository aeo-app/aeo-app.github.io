---
name: n-day-plan
description: "Create a practical, generic N-day operating plan for a business, project, launch, growth effort, habit, or transformation, where N is the number of days you provide. Use when the user asks for a 30-day sprint, 90-day plan, quarterly roadmap, day-by-day schedule, execution calendar, accountability tracker, or milestone plan."
argument-hint: "[N]"
user-invocable: true
---

# N-Day Plan

Create an N-day plan (N = the duration you provide) that turns a broad goal into sequenced work, named ownership, recurring execution, measurable checkpoints, and a reviewable record of progress.

## Inputs

Collect the following information when available. Ask only for missing details that materially change the plan; otherwise make assumptions and label them.

- **N (number of days):** the plan duration — the single most important input. Read it from the user's request: "30-day sprint" → N=30, "90-day plan" → N=90, "quarterly" → 90 (or 91 if it must span a calendar quarter), unspecified → **N=90**. Confirm N before writing; every phase boundary, checkpoint, gate, target, and the output filename derive from it.
- **Goal:** the outcome the N days should produce
- **Starting point:** current state, constraints, risks, and known blockers
- **Start date:** use the user's date, or propose one
- **People and roles:** who owns decisions, delivery, review, and support
- **Workstreams:** the major areas of work
- **Cadence:** recurring daily, weekly, or monthly activities
- **Resources:** tools, budget, dependencies, and source material
- **Measures:** baseline, leading indicators, outcome metrics, and targets
- **Operating constraints:** capacity, blackout dates, approvals, compliance, or quality requirements

If the user provides an existing plan, preserve its useful operating mechanics while replacing specific names, brands, channels, dates, and assumptions with generic equivalents.

## Planning Method

1. **Define the finish line.** State the primary outcome, secondary outcomes, non-goals, and measurable Day-N targets.
2. **Capture the baseline.** Record what is true before work begins and how it will be measured. Never invent data; mark unknowns as `TBD` or `assumption`.
3. **Assign ownership.** Give every workstream one accountable owner, supporting roles, handoff points, and an approver where needed.
4. **Sequence the phases, scaled to N.** Use this default structure and scale it to N rather than keeping fixed 14/84/90 numbers:
   - **Foundation** = approximately the first **16% of N** days, rounded to a whole number and at least 3 days — baseline measurement, setup, research, risk removal, prioritization, and preparation. Known mappings: N=90 → Days 1-14; N=30 → Days 1-5.
   - **Execution** = the long middle stretch — a repeatable weekly rhythm that delivers the main outputs and records results.
   - **Evaluation** = the final approximately **7% of N** days (at least 4) — re-measure the baseline, review outcomes, document lessons, and decide what continues. Known mappings: N=90 → Days 85-90; N=30 → Days 27-30.
5. **Add gates.** Put a go/no-go or readiness check at the end of the foundation phase (its day = the foundation boundary above). Do not let execution hide an unresolved prerequisite.
6. **Build the cadence.** Define what happens each day or week, which output it produces, who owns it, and what it depends on.
7. **Design feedback loops.** Include a short weekly review, a midpoint checkpoint, and the final review. Each review must lead to a decision or adjustment. For short sprints (N ≤ 30) you may compress to: gate at foundation end, a mid-sprint spot-check, and the final review.
8. **Create the tracking model.** Specify the fields needed to record planned work, actual completion, metrics, blockers, evidence, and next actions.
9. **Add guardrails.** State the quality, ethical, scope, evidence, and escalation rules that must hold under deadline pressure.
10. **Check feasibility.** Remove duplicate work, expose overloaded owners, account for dependencies, and ensure the weekly cadence fits the available capacity.

## Required Output

Produce a **single, self-contained Markdown document** in the same anatomy as the exemplar files this repo ships with:
- `projects/AEO-Intel_Full_Schedule_and_Content_Library.md` — the primary anatomy reference, a full 90-day plan (blog + LinkedIn/IG/FB outlets).
- `projects/APAC-Relocation_30-Day_AEO-GEO_Schedule_and_Content_Library.md` — a keyword-targeted instance that runs as an **aggressive 30-day sprint** (N=30): Days 1-4/5 foundation, Days 6-26 execution, Days 27-30 evaluation, with target queries plus Video/Reels, Reddit/Quora forums and a backlink workstream. Borrow from it for GEO/keyword-driven plans, and use it to see how the phase template above scales to a short N.

Mirror the exemplar's section order and headings; where it names outlets (Blog / LinkedIn / Facebook / Instagram), substitute the current plan's actual workstreams. Name the output `<Brand>_<N>-Day_<Program>_Schedule_and_Content_Library.md` at the repository root (put it in `projects/` if the repo keeps schedules there). Return the plan in this order:

### 1. Header and one-file note

First line: `# <Brand or team> — Full <N>-Day Schedule & Content Library`. Below it, an opening block stating the start date, end date (start + N − 1), and what one file contains: the complete day-by-day task table plus every deliverable's full brief, linked from the table so each scheduled task jumps straight to its brief in the same document.

### 2. Ownership Key

Bullets mapping each named owner to their lane and one-line scope, plus any placeholder roles. Keep names, not titles, so assignments are traceable.

### 3. Notes

Decisions that aren't visible in the table but change how the plan runs — for example, why a channel is owned where it is, why the calendar starts when it does, or a rest-day rhythm. The exemplar documents why the Founder posts from a personal profile rather than the company page; note equivalent "why" decisions here.

### 4. Day-by-Day Schedule table

A full table, one row per day for the whole period (Day 1 … Day N):

| Day | Date | Weekday | Assigned To | <workstream/outlet columns> | Notes / Checkpoint |
|---|---|---|---|---|---|
| 1 … N | | | | | |

- One column per workstream or outlet holding that deliverable's task, with the deliverable title as an in-file link to its brief anchor (e.g. `[The State of AEO in APAC 2026](#week-1)`).
- Phase gates and checkpoints (the foundation go/no-go, the midpoint and final reviews) embedded in the Notes / Checkpoint column, placed on their scaled days.
- Dependencies appear before dependent work.
- Every action concrete enough to mark complete — never `work on X` or `improve Y`.

### 5. Briefs & Prompts (every planned deliverable)

One `### Week N` block per week, holding a `#### <Workstream> — <Tactical/Pillar or sequence>: "<Name>"` entry per deliverable. Every entry — blog, social post, video, forum answer, outreach email, whatever the channel — must be **detailed enough that a non-expert operator can execute without asking the planner what to do**. Use the same per-item shape the APAC exemplar now uses:

- **Topic:** what exactly is being made and which target keyword/query it feeds.
- **Why:** one line of positioning so the operator knows the tone and intent (do not rely on the team to infer it).
- **What to include, in order:** numbered steps of the required elements — line-by-line for a post, slide-by-slide for a carousel, shot-by-shot for a Reel/Short, section-by-section for a video script, step-by-step for a forum answer or outreach session.
- **Ready-to-use copy:** a full copy-paste draft (post text, caption, video title + description, email template, follow-up line, GBP post). Use `[SQUARE-BRACKET PLACEHOLDERS]` (e.g. `[S$ X]`, `[route]`, `[URL]`) for anything needing real facts, and say explicitly that placeholders must be filled with real, Founder-approved values before publishing.
- **Checklist before publishing:** named checks per deliverable (subtitles on Reels, URL last frame, link in first comment, Founder approval, reply windows). If any box is unticked, do not publish.
- Where real data is mandatory, mark the brief `[NOTE TO WRITER: ...]` and say what must be sourced before publish — never leave it as a silent placeholder or a made-up figure.

Reusable per-channel templates to supply inside the briefs (draw on the APAC exemplar):
- **Reel/Short:** 30-second max; hook in first 2 seconds; a shot-by-shot storyboard (timing, what we see, text on screen, what to say aloud); caption; the standing rule that the site URL is held on screen for the final second.
- **LinkedIn/IG/FB post:** line-by-line answer-first copy (keyword phrase in lines 1–2), explicit rule that links go in the first comment, and a comment-driving question at the end.
- **YouTube:** description template that opens with the answer, timestamp list, section-by-section script outline, thumbnail idea, tags.
- **Forums:** the exact "answer-first, disclose, one humble link max" rule, plus a ready-to-use answer skeleton and a log-into-tracker step.
- **Backlinks:** session goal, step-by-step workflow, a copy-paste outreach template and follow-up line, a max-count rule (e.g. 5 emails), and a tracking/approval step.

**Anchor format (critical):** the day-by-day table links each deliverable to its brief via in-file anchors. Write the anchor on its **own line, followed by a blank line, before the heading**:

```markdown
<a id="w1-li-teaser"></a>

#### LinkedIn teaser · Mon 05 Oct (morning) · Shahana
```

Never put the anchor on the same line as the heading (`<a id="..."></a>####`) — CommonMark renders that as an HTML block and the `####` prints literally. Also give the schedule table itself an explicit anchor (`<a id="day-by-day-schedule"></a>`) so "back to table" links resolve in any renderer, not just GitHub's auto-generated heading slugs.

### 6. Notes Before Publishing

Guardrails that apply to every deliverable before it goes live: fact accuracy and sourced data, brand/visual consistency, and publish-on-assigned-day discipline.

### 7. Notes on the Rhythm

Explain the pacing and verification rationale. The exemplar explains why its foundation phase runs one task per day (dependencies, verify-before-building-on, pacing discipline over speed) and when tasks may be combined as long as the phase gate is never skipped. Mirror that reasoning for the current plan, and reflect how N shortened or stretched the default phase template.

### 8. Supporting Tasks

Prerequisites and parallel work that don't get their own rows but block others if forgotten: accounts/logins ready before Day 1, asset prep, tracker setup, owner availability.

### 9. Sources Referenced

Every external source and any internal data the plan relies on, as URLs, so nothing is asserted without a reference.

## Quality Checks

Before finalizing, verify that:

- The goal and Day-N outcome are measurable.
- N was taken from the user (or defaulted to 90 with the assumption labeled); phase boundaries, gates, and the filename all use N consistently.
- The baseline and measurement method are explicit.
- Every workstream has one accountable owner and no unowned deliverables.
- Dependencies appear before dependent work; the foundation gate can genuinely block execution.
- The cadence is feasible for the stated capacity.
- Dates, weekdays, and day numbers are internally consistent (Day 1 = start date, Day N = start date + N − 1).
- Every row that schedules a deliverable links to its brief; every brief has an owner.
- Every brief is executable by a non-expert: it includes topic/why, the content in order (line/slide/shot/section-level), ready-to-use copy with square-bracket placeholders, and a publish checklist.
- Every `<a id="..."></a>` anchor sits on its own line with a blank line before its heading; the schedule table has an explicit anchor for "back to table" links.
- Every `[NOTE TO WRITER]`-style placeholder names the real data required.
- Metrics distinguish activity from outcome; checkpoints produce decisions, not just status updates.
- Unknown facts are labeled instead of invented.
- The document mirrors the exemplar anatomy (sections 1-9 above) so it can be run without the planner present.

## Adaptation Rules

- The exemplar files `projects/AEO-Intel_Full_Schedule_and_Content_Library.md` (canonical anatomy, N=90) and `projects/APAC-Relocation_30-Day_AEO-GEO_Schedule_and_Content_Library.md` (keyword/GEO variant, N=30) are the output-format references — borrow their structure, but replace names, brands, channels, dates, metrics, and assumptions with the new context.
- Scale every fixed calendar number to N: foundation ≈ 16%, evaluation ≈ 7%, gates and review days on the scaled boundaries, and `<N>-Day` in the header intelligence and filename.
- For a content or growth plan, keep the outlet columns (blog/social/etc.), per-item briefs (build them at the APAC exemplar's detail level — topic/why, line-by-line copy, storyboards, checklists), and repurposing chains exactly as in the exemplar; connect each content activity to an audience, distribution route, and measurable response.
- For a non-content plan, swap the outlet columns for workstream columns and reduce §5 to "Task Briefs & Prompts" — one exact, executable instruction per deliverable — while keeping the same single-file anatomy.
- For a solo operator, replace roles with responsibility areas and reduce the cadence before reducing quality controls.
- For a team, preserve a single accountable owner per deliverable and make handoffs explicit.
- For a launch, make readiness, rollback, support, and post-launch review first-class workstreams.
- For a learning or habit goal, treat practice as the output and demonstrated capability as the outcome.
- For regulated, financial, health, or safety-sensitive work, add domain review and approval gates before recommending execution.