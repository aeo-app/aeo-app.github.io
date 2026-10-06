# n-day-plan

Generates a **day-by-day operating plan** with named owners, real gates, a sourced claims ledger, and one detailed brief behind every scheduled deliverable — written so an operator who has never met the client can execute it without asking a question.

It runs an **adversarial red/blue review** before drafting and records every contested decision in a written Decision Log that ships inside the artifact.

The output standard is the exemplars in `website/content/posts/` — the LawMatter 46-day runbook (3,271 lines, 13-channel table, Claims Ledger, compliance gate, pre-approved gate cut lists) is the reference.

---

## How to invoke

Three ways, all equivalent:

1. **Ask in plain language.** "Write me a 46-day AEO/GEO plan for…" — the skill's `description` is what makes it discoverable, so a matching request loads it automatically.
2. **Ask for it by name.** "Use the `n-day-plan` skill to write a 46-day AEO/GEO plan for…"
3. **Tell the agent to load it:** "load the `n-day-plan` skill"

**About the frontmatter.** The skill ID is `n-day-plan` (from the directory name). The `argument-hint` and `user-invocable` fields in `SKILL.md` are carried for portability — **Copilot does not interpret them** — so put N and the goal in your message rather than expecting them to be parsed.

```
.github/skills/n-day-plan/
├── SKILL.md      ← the skill itself (what the agent loads)
└── README.md     ← this file
```

**Where this is discovered.** Copilot, the Copilot CLI, and cloud agent read `.github/skills/`. OpenCode does not — it looks in `.opencode/skills/`, `.claude/skills/`, and `.agents/skills/`. To use this skill from both, point OpenCode at `.agents/skills/` and keep `.github/skills/` for Copilot.

---

## The two profiles

Pick one per run, or let the skill infer it and say which it used.

| | `aeo-geo` — marketing runbook | `generic` — operating plan |
|---|---|---|
| **Use for** | Client-facing growth, campaign, visibility and content plans | Launches, migrations, migrations of tooling, enablement, habits, anything non-marketing |
| **Table columns** | One per channel: SEO/Site/Schema · Blog · LinkedIn · YouTube · Email & Outbound · Paid & Partners · Instagram · Facebook · GBP · GEO/AI-Citation | One per workstream |
| **Ledger** | Claims Ledger: exact wording, source URL, status, what it blocks | Decision / assumption / constraint register, same columns |
| **Publication gate** | Compliance/claims gate, 10-ish checks with evidence of sign-off | Pre-merge / pre-launch / release checklist, same three columns |
| **Funnel maths** | Stage-by-stage arithmetic with blank client inputs | Capacity maths: hours available vs. hours required per owner |
| **UTM convention** | Full UTM pattern + conversion events + attribution window | Source-of-truth and reporting convention |

**Both profiles emit the same 26 sections.** Where a marketing section does not apply to a generic plan, it is written as `**Not applicable — and here is why.**` rather than silently dropped. The skill is told never to omit a section quietly.

---

## What to put in your request

The skill asks only for gaps that materially change the plan. Supplying these up front saves a round trip and materially improves the output:

| Give it | Why it matters |
|---|---|
| **N** (the number of days) | The spine. Every phase boundary, gate and filename derives from it. Unspecified → 90, labelled. |
| **Goal** | One outcome someone else can observe. |
| **Start date**, or a fixed external date | A non-movable date (an event, a renewal) reshapes the entire ramp. |
| **Who owns what** — real names | One accountable owner per deliverable. Placeholders get flagged as placeholders. |
| **The URL** | If you have web access at the skill will `curl` it, view-source it, and measure a real baseline. That measurement is what makes every later trade-off arguable. |
| **Budget and its status** | "Spend from day 1" and "hold until the gate passes" produce very different plans. |
| **Constraints** | Capacity, approvals, legal exposure, blackout dates, things that must not be said. |
| **Where to write** | Defaults to `website/content/posts/` for publishing, or `output/` for a draft. The skill asks. |

**No web access?** Say so. The plan still works — every unverifiable claim lands as a bracketed placeholder or a reserved ledger row instead of a fabricated number. That is the designed behaviour, not a degraded mode.

---

## Examples

### 1. The main event — a new AEO/GEO client

```
@n-day-plan Write a 46-day AEO/GEO plan for a new client: Meridian Law, a 12-person Melbourne
family law firm going after AI-assist discovery in wills and estate planning. Site:
https://meridianlaw.example — you can fetch it. Starts 12 January 2027.

Owners: Priya (founder, approves every claim), Dan (content), Mei (part-time tech).
Budget: A$4,000/mo in paid, LinkedIn + Google Search only.
Write it to website/content/posts/ and give me the Decision Log.
```

**You'll get:** a 13-channel day-by-day table, a seeded Claims Ledger with Priya's open questions as tracked rows, a Day 7 foundation gate with a pre-approved cut list, weekly brief blocks with publishable copy, and a build-verified Hugo post.

### 2. Same client, follow-on sprint

```
@n-day-plan The LawMatter 46-day sprint ends 14 Nov 2026. Write the 90-day always-on plan
that follows it — same owners, same ledger, profile aeo-geo. Reuse the standing playbooks
from the existing post rather than reinventing them. No fixed event this time.
```

**You'll get:** an always-on cadence with no event spike, the same owners and DoD, the existing ledger carried forward with new research rows appended, and phase gates keyed to the 90-day structure rather than a countdown.

### 3. A fixed date and a thin team

```
@n-day-plan 21 days to the Singapore FinTech Expo, 3 March. AEO/GEO plan for it. It's me plus
one freelancer, and the budget is fixed at S$6,000 — I can't spend more.
```

**You'll get:** an aggressive ramp that spends *before* the expo, an honest feasibility paragraph, rest days sized for two people, and a cut list that says what to drop if the freelance budget disappears. Red will push back on spending against a site with no event entity yet; Blue will answer with a mechanism.

### 4. Generic profile — engineering, not marketing

```
@n-day-plan 30-day plan to migrate our docs from Notion to Docusaurus. Five engineers, but
only two can write docs at a time and we have a release in week 4. Generic profile.
Draft it to output/ — not ready to publish.
```

**You'll get:** workstream columns instead of channels, a capacity-vs-load table showing two writers cannot cover the weeks you asked for, the ledger as a decision register, and each marketing section carrying its explicit fallback sentence.

### 5. Habit / enablement

```
@n-day-plan 21-day plan to get a 12-person team onto our AEO standard — baseline audit in week
1, then one habit a day, review every Friday. Generic profile.
```

**You'll get:** Days 1–3 for the baseline audit, a daily cadence with named owners, a Day 4 gate that can genuinely stop the ramp, and a close that re-runs the same audit and compares.

---

## What comes back

**In the artifact** — 26 sections, grouped:

| Block | Sections |
|---|---|
| **Framing** | What this document is · goal as a ranked outcome · glossary · how every brief is written · where the information comes from · what this plan will not do · why this plan is shaped the way it is |
| **Governance** | Decision Log · decisions, assumptions and open questions · verification rules · the ledger · publication gate · capacity/funnel maths · tracking convention |
| **Execution** | The day-by-day schedule · how to read every brief · standing playbooks · phase and week briefs · gates and checkpoints |
| **Closing** | Notes before publishing · platform publishing notes · rhythm notes · supporting tasks · sources |
| **Throughout** | Every deliverable brief: what it is · why · why it matters · expected result · where the facts come from · the copy itself · pre-publish checklist |

**In the reply** — the file path written, **the Decision Log in full**, anything that shipped as a placeholder rather than content, and the QA results.

### The Decision Log

The part you will most want to review. Each load-bearing decision is argued in both directions, then ruled on with a reversal condition:

| # | Decision | Red's objection | Blue's reply | Ruling | Reverses if |
|---|---|---|---|---|---|
| D1 | Is the client a vendor, a regulated practitioner, or both? | Two sources imply two regulatory regimes; picking one risks a conduct breach | Apply the **strictest combination** of both to every asset | Strictest combination. Costs one lane; removes a class of legal exposure | Client confirms a practising certificate, or counsel rules |
| D2 | Paid spend from Day 1, or after the foundation gate? | Spend lands on a page with no event entity until Day 3 | Hold to zero until the gate passes; site verified 200 with server-rendered HTML | Hold to zero, release at the gate | Gate passes early |

Red's veto covers anything fabricated or unlawful. Blue may not answer with confidence — only with a mechanism: a process, a deadline, a cut, or a source.

---

## Where the output lands

The skill asks, then:

- **`website/content/posts/<brand>-<N>-day-<program>-schedule-content-library.md`** for publishing. Hugo frontmatter is added, `date` is kept on or before today, and the build is verified:
  ```sh
  cd website && hugo --minify     # must build clean
  cd website && hugo list all     # the post must be listed
  ```
  A future-dated post is dropped from the build silently, so this check is not optional.
- **`output/`** for a draft awaiting review. Non-DOCX files there are tracked, so name it for the client.

Companion skill: **`markdown-to-docx`** converts the result for sending out — it requires an explicit input path.

```sh
pandoc "website/content/posts/<file>.md" --from=gfm --to=docx --output="output/<name>.docx"
```

---

## Code implementation

Three shapes, pick one. All three end in the same artifact and the same build gate.

The snippets below are patterns, not a runnable file. `SYSTEM_PROMPT`, `BRIEF`, `GRADER_PROMPT`, `AUDITOR_PROMPT`, `thread_id`, `log`, and `anchor_audit` are yours to supply; `hugo_gate` is the only helper shown in full, because it is the one that encodes this repo's rules.

| Shape | Use when | Skill is discovered by |
|---|---|---|
| **A. Call an agent server over HTTP** | You already run OpenCode and want the harness that produced these posts | OpenCode — but see the prerequisite below |
| **B. `deepagents.create_deep_agent`** | Your artifact generator is a Python service | DeepAgents, via `skills=[...]` |
| **C. Supervisor + rubric gate** | The output is client-facing and you need a recorded pass/fail | Either, plus a grader |

### A. Drive the existing harness from Python

`opencode serve` exposes the agent over HTTP, so the caller only needs a session and a message.

**Prerequisite:** OpenCode does not read `.github/skills/`. To use this shape, mirror the skill to a directory OpenCode scans — `.agents/skills/n-day-plan/` (also read by Copilot, so one copy serves both) — and run the server from the repo root.

```sh
opencode serve --port 4096 --hostname 127.0.0.1
```

```python
import os

import httpx

BASE = "http://127.0.0.1:4096"
AUTH = ("opencode", os.environ["OPENCODE_SERVER_PASSWORD"])

with httpx.Client(base_url=BASE, auth=AUTH, timeout=1800) as client:
    assert client.get("/global/health").json()["healthy"]
    session = client.post("/session", json={"title": "n-day-plan"}).json()
    prompt = "Use the n-day-plan skill. 46 days, aeo-geo, write to website/content/posts/"
    response = client.post(
        f"/session/{session['id']}/message",
        json={"agent": "build", "parts": [{"type": "text", "text": prompt}]},
    )
    response.raise_for_status()
```

Notes for shape A:

- Run the server with the repo root as the working directory, otherwise the skill is not discovered.
- Set `OPENCODE_SERVER_PASSWORD` to enable HTTP basic auth. The username is `opencode` unless `OPENCODE_SERVER_USERNAME` overrides it.
- `parts` is a discriminated union; the text part is `{"type": "text", "text": ...}`. Generate a client from the OpenAPI 3.1 spec at `http://127.0.0.1:4096/doc` rather than hand-rolling it.
- `POST /session/{id}/prompt_async` returns `204` and streams progress on `GET /event`, which emits `server.connected` first. Use that for long runs instead of blocking.
- `GET /session/{id}/diff?messageID=` and `GET /file/content?path=` are how the caller confirms the artifact landed.

### B. `deepagents.create_deep_agent` with the skill mounted

```python
from pathlib import Path

from deepagents import FilesystemBackend, create_deep_agent
from langchain.chat_models import init_chat_model

REPO_ROOT = Path("/home/ai/Code/aeo-app/aeo-app.github.io")

agent = create_deep_agent(
    model=init_chat_model("anthropic:claude-sonnet-4-5"),
    system_prompt=SYSTEM_PROMPT,
    skills=[".github/skills"],
    backend=FilesystemBackend(root_dir=REPO_ROOT, virtual_mode=True),
)

result = agent.invoke({
    "messages": [{
        "role": "user",
        "content": "Use the n-day-plan skill. @n-day-plan 46 days, aeo-geo, "
                   "write to website/content/posts/.",
    }],
})
```

The `skills` argument is the part that bites people:

- **Each path must be a directory that *contains* skill directories.** `.github/skills` is correct; `.github/skills/n-day-plan` loads nothing, with no error. A path pointing straight at a `SKILL.md` is silently ignored.
- Paths use forward slashes and resolve relative to the backend root, so `FilesystemBackend(root_dir=REPO_ROOT)` plus `.github/skills` lines up with this repo.
- Discovery is progressive disclosure: `name` and `description` are always in context, the full `SKILL.md` is read when the task matches, and supporting files such as `README.md` are read only if the agent wants them.
- `SKILL.md` frontmatter needs `name` and `description`. Ours also carries `argument-hint` and `user-invocable`, which DeepAgents ignores, so the request must state **N** and the goal in prose. `@n-day-plan` alone is an OpenCode convention, not a DeepAgents one.
- With `virtual_mode=True` the agent is sandboxed to `root_dir`. Lock the skill directory read-only so a run cannot rewrite its own instructions:
  ```python
  from deepagents import FilesystemPermission

  READ_ONLY_SKILLS = FilesystemPermission(
      operations=["write"],
      paths=["/skills/**"],
      mode="deny",
  )
  ```
  then pass `permissions=[READ_ONLY_SKILLS]` to `create_deep_agent`.
- Skill tools (`read_skill`, `activate_skill`) need `SkillsMiddleware(backend=..., sources=[...])` and `deepagents>=0.7.22`. Reload metadata mid-run by sending `skills_metadata: None` in the input, which needs `>=0.7.16`.

### C. Supervisor and the quality gate

Two independent layers. The deterministic gate is not optional, and the grader never gets to override it.

**Layer 1 — deterministic gate.** Expose the build as a tool so the gate is execution, not opinion:

```python
def hugo_gate(artifact_path: str) -> dict:
    """Build the site, confirm the post is listed, diff in-page anchors against emitted ids."""
    build = subprocess.run(["hugo", "--minify"], cwd="website", capture_output=True, text=True)
    listed = subprocess.run(["hugo", "list", "all"], cwd="website", capture_output=True, text=True)
    anchors = anchor_audit(Path("website/public"), Path(artifact_path))
    return {
        "build_clean": build.returncode == 0,
        "post_listed": slug(artifact_path) in listed.stdout,
        "broken_anchors": anchors.broken,
    }
```

**Layer 2 — LLM-as-judge over the skill's own QA section.** `RubricMiddleware` runs the grader, feeds back revisions, and stops:

```python
from deepagents import RubricMiddleware
from langgraph.checkpoint.memory import InMemorySaver

RUBRIC = """
Every section in SKILL.md §6 is present and labelled.
Every factual claim carries a source URL and a verified status.
Every gate has a named owner, window, threshold, and no-go outcome.
The red/blue round records each contested decision in the Decision Log.
No statistic appears without a source or an explicit unknown label.
"""

agent = create_deep_agent(
    model=init_chat_model("anthropic:claude-sonnet-4-5"),
    system_prompt=SYSTEM_PROMPT,
    skills=[".github/skills"],
    backend=FilesystemBackend(root_dir=REPO_ROOT, virtual_mode=True),
    checkpointer=InMemorySaver(),
    tools=[hugo_gate],
    middleware=[
        RubricMiddleware(
            model=init_chat_model("anthropic:claude-haiku-4-5"),
            tools=[hugo_gate],
            max_iterations=3,
            system_prompt=GRADER_PROMPT,
            on_evaluation=lambda evaluation: log(evaluation),
        )
    ],
)

result = agent.invoke(
    {"messages": [{"role": "user", "content": BRIEF}], "rubric": RUBRIC},
    config={"configurable": {"thread_id": thread_id}},
)
if result.get("_rubric_status") != "satisfied":
    raise SystemExit(f"quality gate failed: {result.get('_rubric_status')}")
```

Behaviour worth knowing before you ship this:

- The `rubric` string is passed **at invoke time**, not at construction. Omit it and the middleware does nothing.
- Verdict values are `satisfied`, `needs_revision`, `max_iterations_reached`, `failed`, `grader_error`. Gate on `satisfied` and nothing else.
- `result["_rubric_evaluations"]` holds each `RubricEvaluation` (`iteration`, `verdict`, `feedback`, `rubric`, `grader_model`), and `_rubric_iterations` the count. Both are what you persist as the audit trail.
- A smaller, cheaper grader model is usually the right call. `RubricMiddleware` is **beta** and needs `deepagents>=0.6.5`.
- `on_evaluation` gives you per-iteration callbacks. For live progress, stream with `stream_events(..., version="v3")` and a `CustomTransformer`; `rubric_evaluation_start` and `rubric_evaluation_end` arrive as `stream.custom` events.
- Checkpointing is required for the filesystem tools. `InMemorySaver` is fine for a one-shot job; use a durable saver if a revision loop must survive a restart.

**Supervisor variant.** When you want the review to be independent of the drafting context, register the reviewer as a subagent instead of relying on the rubric loop. Skill state is fully isolated per subagent, so give the reviewer its own `skills` list:

```python
create_deep_agent(
    model=init_chat_model("anthropic:claude-sonnet-4-5"),
    system_prompt=SYSTEM_PROMPT,
    skills=[".github/skills"],
    backend=FilesystemBackend(root_dir=REPO_ROOT, virtual_mode=True),
    subagents=[{
        "name": "auditor",
        "description": "Reviews a draft runbook against SKILL.md and rejects it on any gate failure.",
        "system_prompt": AUDITOR_PROMPT,
        "skills": [".github/skills"],
        "tools": [hugo_gate],
    }],
)
```

Use the rubric loop when you want the same agent to self-correct against a fixed list. Use the supervisor subagent when you want a second context, and optionally a second model, to sign off. For client-facing work, run both.

### Version floors

| Feature | Minimum `deepagents` | Stability |
|---|---|---|
| `RubricMiddleware` | `0.6.5` | beta |
| Skill metadata reload (`skills_metadata: None`) | `0.7.16` | stable |
| Skill tools (`SkillsMiddleware`) | `0.7.22` | stable |

### Where to put this code

Keep the Python outside `website/content/` — Hugo renders that tree. Also mind the repo-root `.gitignore`, which still carries entries from a pre-Hugo manual publish: a file written at the repo root named `index.html`, `posts/`, `assets/`, `tags/`, `categories/`, `404.html`, `robots.txt`, `sitemap.xml`, or `page/` is silently ignored and will not show up in `git status`.

---

## Tuning the skill

Edit `SKILL.md` directly. The levers that change output most:

| To change | Edit |
|---|---|
| Phase split (default: foundation ~16% of N, evaluation ~7%) | §5 |
| Which channels get a table column | §2 profile choice, §9, §10 |
| The persona set or the debate protocol | §4 |
| Which sections are mandatory, and their fallbacks | §6 |
| The brief contract (the 7 parts) | §7 |
| Ledger statuses and the reserved-row rules | §8 |
| Gate rigour and the cut-list requirement | §9 |
| The QA checklist | §12 |

The exemplars in `website/content/posts/` are the format authority. If the skill and an exemplar disagree, the exemplar wins — update the skill.

---

## Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Post missing after build | `date` is in the future and `buildFuture` is unset. Check `hugo list all`; put the real start date in the body. |
| In-page links into table rows jump nowhere | Goldmark only auto-generates IDs for headings. Add `<a id="..."></a>` **on its own line, with a blank line, before the heading.** Run the anchor audit in §12 of the skill. |
| `####` printing literally | The anchor was placed on the same line as the heading; CommonMark parsed it as an HTML block. |
| Too many fabricated-looking numbers | The skill had no web access. Give it URLs, or accept the reserved-row output as correct behaviour. |
| Schedule is unrealistically full | Give it real weekly hours per owner. It cuts in the plan and prices the cut rather than assuming overtime. |
| Skill not offered by the agent | Its `description` did not match the request. Mention `@n-day-plan` explicitly. |