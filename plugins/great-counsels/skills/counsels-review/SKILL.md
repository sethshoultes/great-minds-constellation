---
name: counsels-review
description: Dispatch one or more counsel personas to review an existing decision, policy, practice, or draft memo. Default panel for parallel review — Ruth Bader Ginsburg (constitutional / civil-rights lens), Lawrence Lessig (regulability / digital-context lens), John Rawls (justice-as-fairness lens). Override with --personas. Reads the project specification + the target file/directory + relevant adjacent context, produces a consolidated review with per-persona verdicts and a single highest-leverage recommendation. Output saves to counsel/reviews/<slug>.md. NOT LEGAL ADVICE — a craft register.
---

# /counsels-review <path> [--personas <list>]

Review an existing decision, policy, practice, or draft memo through multiple counsel lenses.

> ⚠️ **NOT LEGAL ADVICE.** This skill produces craft-register writing in the voice of canonical figures. The review is a reasoning exercise, not a representation by counsel. Real legal questions require real lawyers.

## What this does

Dispatches counsel personas to review the artifact at `<path>`. Default panel runs three personas in parallel (RBG, Lessig, Rawls), each with a different lens. Output is a consolidated review — per-persona verdicts marked up, then a single recommended highest-leverage change.

## When to use

- A draft memo needs scrutiny before being committed.
- An existing decision or policy benefits from a multi-lens review.
- An ethical question is "working but feels wrong" and needs named diagnoses.
- A practice has accumulated and needs an audit before further investment.

Not for: live legal review of a real matter (the disclaimer is not optional); pixel-level policy nits; parallel-debate format (filed for v1.0 as `/counsels-debate`).

## Instructions for Claude

When this skill is invoked with a `<path>` argument and optional `--personas`:

1. **Resolve the project root** the same way `/counsels-project-init` does. Verify `CLAUDE.md` exists or warn that the counsel bible is missing.

2. **Read the project specification:**
   - `CLAUDE.md`, `README.md`, prior memos at `counsel/memos/`, prior reviews at `counsel/reviews/`, briefs at `counsel/briefs/`
   - The actual decision, policy, or practice the target concerns (if not already in the target)
   - `.great-authors/project.md` if cross-craft project

3. **Read the target.** `<path>` may be:
   - A single file (a memo, a policy, a draft brief)
   - A directory (a body of memos, a policy library)
   - A glob (`counsel/memos/*.md`)

   Read the target file(s) in full. Read adjacent context — sibling files, the underlying decision the memo addresses, prior reviews that motivated the current draft. The reviewer can't review what they can't see.

4. **Resolve the panel.** If `--personas` is given (comma-separated), use those. Otherwise default panel:

   - `ruth-bader-ginsburg-counsel` — constitutional / civil-rights / equal-protection lens
   - `lawrence-lessig-counsel` — regulability / digital-context / four-modalities lens
   - `john-rawls-counsel` — justice-as-fairness / veil-of-ignorance lens

   Common alternative panels:

   - **Constitutional debate:** `ruth-bader-ginsburg-counsel` + `antonin-scalia-counsel` — progressive vs. originalist (the canonical disagreement)
   - **Privacy / antitrust review:** `louis-brandeis-counsel` + `tim-wu-counsel` — the right to be let alone meets the curse of bigness
   - **Regulatory review:** `cass-sunstein-counsel` + `lawrence-lessig-counsel` — choice architecture meets code-as-law
   - **Ethics / political philosophy review:** `hannah-arendt-counsel` + `john-rawls-counsel` — the public realm meets the veil
   - **Civil rights litigation strategy review:** `thurgood-marshall-counsel` + `ruth-bader-ginsburg-counsel` — the long game and the test case
   - **Digital-platform review:** `lawrence-lessig-counsel` + `tim-wu-counsel` — code is law meets the Cycle

5. **Dispatch the panel in parallel** via the Agent tool. Each persona gets:
   - The full target content (file or directory contents)
   - The bible context (CLAUDE.md, prior memos, prior reviews, the underlying decision)
   - Specific instruction to produce a review in their voice with: **Verdict** (one sentence top-line), **Marked passages** (3-8 quoted excerpts with strikethroughs for cuts and `[→ replacement]` for substitutions), **Hand-off** (if a different persona would serve better).

6. **Consolidate the parallel returns.** The output is a single review file:

```markdown
---
title: Counsel review of <path>
slug: <slug>
panel: [<persona-1>, <persona-2>, <persona-3>]
created: YYYY-MM-DD
target: <path>
target_lines: <N>
---

# Counsel review: <path>

> ⚠️ **Craft register only — not legal advice.** This review is a reasoning exercise from three counsel lenses. It is not a representation by counsel. Real legal questions require real lawyers.

## Per-persona verdicts

### <persona-1>
**Verdict:** <one sentence top-line reaction>

**Marked passages:**
- <quoted excerpt 1>
- <quoted excerpt 2>
- ...

**Hand-off:** <if a different persona would serve better; or omit>

### <persona-2>
[same structure]

### <persona-3>
[same structure]

## Where they agree

<1-3 points where the panel converges. The strongest signal — when independent reviewers with different lenses flag the same thing, it's almost certainly real.>

## Where they disagree

<1-2 points where the panel diverges. Often the most useful section — disagreement reveals the genuine trade-off the author is making. RBG vs. Scalia disagreements in particular are load-bearing, not noise.>

## Highest-leverage change

<ONE recommendation. The single change that, if made, would make the artifact meaningfully better. Not a list. The orchestrator's job is to pick the highest-leverage move; this skill picks it for them.>

## Suggested next step

<One of: implement the change, escalate to a real attorney, request a v2 from the memo author, run /counsels-debate <topic> <persona-A> <persona-B> if the disagreement is structural (RBG vs. Scalia is the canonical example), cross-dispatch great-minds:marcus-aurelius-mod if the question is interpersonal/Stoic-mediation-shaped.>
```

7. **Save the review** to `counsel/reviews/<slug>.md`. Slug derived from the target path or from `CLAUDE.md`'s `Current memo:` field.

8. **Report:**
   ```
   📝 Saved to counsel/reviews/<slug>.md (review of <path>, <word-count> words).
   ⚠️ Craft register only — not legal advice.

   Panel:           <persona-1>, <persona-2>, <persona-3>
   Convergence:     <one-line summary of the agreement>
   Highest-leverage change: <one-line summary>

   Next:
   - Address the highest-leverage change
   - Or run /counsels-debate <topic> <a> <b> if the disagreement is structural
   - For real legal stakes: retain a licensed attorney
   ```

## What the skill does NOT do

- **Modify the target.** It reviews; the author (human or AI) edits.
- **Approve or reject.** It surfaces the review's findings; the orchestrator decides.
- **Provide legal representation.** The personas are craft channels, not licensed counsel.
- **Auto-pick the panel based on opaque heuristics.** The default panel is documented; alternatives are documented; the user can override explicitly.

## Notes

- Parallel dispatch means the personas don't see each other's reviews when forming their own. This is intentional — independent verdicts produce stronger signal at consolidation time.
- For very large targets (>2,000 lines, or a whole policy library), the dispatch brief should ask each persona to focus on the most-critical sections only — pick a sample, don't try to review the whole thing. Better to do three high-quality reviews of focused sections than three shallow reviews of everything.
- Review fatigue is real. Don't run `/counsels-review` on every memo; use it for non-trivial decisions where the consolidated lens is worth the dispatch cost.
- **The disclaimer block is not optional.** Do not strip the "Craft register only — not legal advice" line from any review.
