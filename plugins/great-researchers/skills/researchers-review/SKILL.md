---
name: researchers-review
description: Dispatch one or more researcher personas to peer-review an existing study, claim, or research artifact. Default panel for parallel review — Stephen Jay Gould (synthesis), Carl Sagan (skeptical inquiry), E. O. Wilson (consilience). Override with --personas. Reads the project specification + the target file/directory + relevant bibliography, produces a consolidated review with per-persona verdicts and a single highest-leverage recommendation. Output saves to research/reviews/<slug>.md. NOT ACADEMIC ADVICE — a craft register.
---

# /researchers-review <path> [--personas <list>]

Peer-review an existing study, claim, or research artifact through multiple researcher lenses.

> ⚠️ **NOT ACADEMIC ADVICE.** This review is a reasoning exercise from three persona lenses, not real peer review.

## What this does

Dispatches researcher personas to peer-review the artifact at `<path>`. Default panel runs three personas in parallel (Gould, Sagan, Wilson — synthesis-strong), each with a different lens. Output is a consolidated review with per-persona verdicts and a single highest-leverage recommendation.

## When to use

- A draft study needs scrutiny before being shared.
- An existing claim or finding benefits from a multi-lens review.
- A literature review feels off and needs named diagnoses.
- A research artifact needs synthesis-and-skepticism check.

Not for: real journal peer review (this skill is craft, not actual double-blind review); pixel-level citation nits; debate format (filed for v1.0 as `/researchers-debate`).

## Instructions for Claude

When this skill is invoked with a `<path>` argument and optional `--personas`:

1. **Resolve the project root.** Verify `CLAUDE.md` exists.

2. **Read the project specification:**
   - `CLAUDE.md`, `README.md`, prior studies at `research/studies/`, prior reviews at `research/reviews/`, bibliography at `research/bibliography/`
   - The actual primary sources the target cites
   - `.great-authors/project.md` if cross-craft

3. **Read the target.** May be a single file, directory, or glob. Read in full — adjacent context, sibling files, the bibliography entries the target relies on.

4. **Resolve the panel.** If `--personas` given, use those. Otherwise default panel:

   - `stephen-jay-gould-researcher` — synthesis, essay-form integrity, just-so-story check
   - `carl-sagan-researcher` — skeptical inquiry, Baloney Detection Kit
   - `edward-o-wilson-researcher` — consilience check, biology-anchored synthesis

   Common alternative panels:

   - **Civilizational-claim review:** `jared-diamond-researcher` + `edward-o-wilson-researcher` — synthesis vs. consilience
   - **Clinical / medical case review:** `oliver-sacks-researcher` + `atul-gawande-researcher` — humanism vs. systems
   - **Deep-research narrative review:** `rebecca-skloot-researcher` + `robert-caro-researcher` — relationship-as-method vs. turn-every-page
   - **Public-science communication review:** `carl-sagan-researcher` + `mary-roach-researcher` — wonder vs. immersion
   - **Sociobiology debate:** `edward-o-wilson-researcher` + `stephen-jay-gould-researcher` — the canonical disagreement (group selection, adaptationism)
   - **Cross-craft technical-rigor review:** add `great-engineers:don-knuth-engineer` (cross-plugin)
   - **Cross-craft political-philosophy review:** add `great-counsels:hannah-arendt-counsel` (cross-plugin)

5. **Dispatch the panel in parallel** via the Agent tool. Each persona gets:
   - Full target content
   - Bible context
   - Specific instruction: **Verdict** (one sentence), **Marked passages** (3-8 quoted excerpts), **Hand-off** (if a different persona would serve better).

6. **Consolidate.** Output:

```markdown
---
title: Research review of <path>
slug: <slug>
panel: [<persona-1>, <persona-2>, <persona-3>]
created: YYYY-MM-DD
target: <path>
target_lines: <N>
---

# Research review: <path>

> ⚠️ **Craft register only — not academic advice.** This review is a reasoning exercise from three persona lenses, not actual peer review.

## Per-persona verdicts

### <persona-1>
**Verdict:** <one sentence>

**Marked passages:**
- <quoted excerpt>
- ...

**Hand-off:** <if different persona would serve better; or omit>

### <persona-2>
[same]

### <persona-3>
[same]

## Where they agree

<1-3 points where the panel converges. Strongest signal.>

## Where they disagree

<1-2 points where the panel diverges. Often the most useful section. The Gould-vs-Wilson disagreement on adaptationism is canonical.>

## Highest-leverage change

<ONE recommendation. The single change that, if made, would make the study meaningfully better.>

## Suggested next step

<One of: implement the change, escalate to a domain expert, request a v2 from the study author, run /researchers-debate <topic> if the disagreement is structural, cross-dispatch great-engineers:don-knuth-engineer for technical rigor or great-counsels:hannah-arendt-counsel for political-philosophy framing.>
```

7. **Save** to `research/reviews/<slug>.md`.

8. **Report:**
   ```
   📝 Saved to research/reviews/<slug>.md (review of <path>, <word-count> words).
   ⚠️ Craft register only — not academic advice.

   Panel:           <persona-1>, <persona-2>, <persona-3>
   Convergence:     <one-line summary>
   Highest-leverage change: <one-line summary>

   Next:
   - Address the highest-leverage change
   - For real publication: actual peer review with domain experts
   ```

## What the skill does NOT do

- **Modify the target.** It reviews; the author edits.
- **Approve or reject for publication.** It surfaces findings; the orchestrator decides.
- **Replace journal peer review.**
- **Auto-pick the panel based on opaque heuristics.** The default is documented; alternatives are documented; the user can override.

## Notes

- Parallel dispatch means personas don't see each other's reviews when forming their own. Independent verdicts produce stronger signal at consolidation.
- For very large targets (>2,000 lines), the dispatch brief should ask each persona to focus on the most-critical sections only.
- **The disclaimer block is not optional.** Do not strip the "not academic advice" line from any review.
