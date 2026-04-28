---
name: designers-design-review
description: Dispatch one or more designer personas to review existing UI, a design system, or a draft spec. Default panel for parallel review — Don Norman (cognitive load), Jared Spool (evidence), Dieter Rams (visual restraint). Override with --personas. Reads the project specification + the target file/directory + relevant adjacent context, produces a consolidated review with per-persona verdicts and a single highest-leverage recommendation. Output saves to design/audits/<slug>.md.
---

# /designers-design-review <path> [--personas <list>]

Review existing UI, a design system, or a draft spec.

## What this does

Dispatches design personas to review the artifact at `<path>`. Default panel runs three personas in parallel (Norman, Spool, Rams), each with a different lens. Output is a consolidated review — per-persona verdicts marked up, then a single recommended highest-leverage change.

## When to use

- A pull request with UI changes needs review and you want the constellation's discipline applied.
- A design proposal (a spec or design doc) needs scrutiny before commitment.
- A surface has grown unwieldy and you want a redesign read.
- A design is "working but feels wrong" and you need named diagnoses.

Not for: pixel-level style nits (most projects have linters or design-system tokens); auto-generated documentation; usability testing of live software (run a real user study with `/designers-channel spool` instead, or hand off to a research firm).

## Instructions for Claude

When this skill is invoked with a `<path>` argument and optional `--personas`:

1. **Resolve the project root** the same way `/designers-project-init` does. Verify `CLAUDE.md` exists or warn that the design bible is missing.

2. **Read the project specification:**
   - `CLAUDE.md`, `README.md`, the brand brief at `design/systems/brand.md`, design system docs, any prior audits in `design/audits/`
   - User research artifacts if present (transcripts, analytics summaries)
   - `.great-authors/project.md` if cross-craft project

3. **Read the target.** `<path>` may be:
   - A single file (a spec, a design doc, a screenshot, a Figma export)
   - A directory (a design system, a section of UI)
   - A glob (`design/specs/*.md`)

   Read the target file(s) in full. Read adjacent context — sibling files, the brand brief, the design system tokens, the user research that motivated the design. The reviewer can't review what they can't see.

4. **Resolve the panel.** If `--personas` is given (comma-separated), use those. Otherwise default panel:

   - `don-norman-designer` — cognitive load, mental models, error states
   - `jared-spool-designer` — evidence-first; what would a user test reveal
   - `dieter-rams-designer` — what can be removed; visual restraint

   Common alternative panels:

   - **Data UI / dashboard review:** `edward-tufte-designer` + `dieter-rams-designer` — chartjunk and restraint
   - **Brand / identity review:** `paula-scher-designer` + `dieter-rams-designer` — voice vs. restraint
   - **Icon-system review:** `susan-kare-designer` + `dieter-rams-designer` — pixel craft and restraint
   - **Product-discovery review:** `marty-cagan-designer` + `jared-spool-designer` — risks and evidence
   - **Physical-product review:** `tinker-hatfield-designer` + `dieter-rams-designer` — narrative and form
   - **Design-management / team review:** `julie-zhuo-designer` + `marty-cagan-designer` — taste and outcomes

5. **Dispatch the panel in parallel** via the Agent tool. Each persona gets:
   - The full target content (file or directory contents)
   - The bible context (CLAUDE.md, brand brief, design system, prior research)
   - Specific instruction to produce a review in their voice with: **Verdict** (one sentence top-line), **Marked passages** (3-8 quoted excerpts with strikethroughs for cuts and `[→ replacement]` for substitutions), **Hand-off** (if a different persona would serve better).

6. **Consolidate the parallel returns.** The output is a single review file:

```markdown
---
title: Design review of <path>
slug: <slug>
panel: [<persona-1>, <persona-2>, <persona-3>]
created: YYYY-MM-DD
target: <path>
target_lines: <N>
---

# Design review: <path>

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

<1-2 points where the panel diverges. Often the most useful section — disagreement reveals the genuine trade-off the implementer or author is making.>

## Highest-leverage change

<ONE recommendation. The single change that, if made, would make the work meaningfully better. Not a list. The orchestrator's job is to pick the highest-leverage move; this skill picks it for them.>

## Suggested next step

<One of: implement the change, escalate to author, request a v2 from the spec author, run a usability test with real users, cross-dispatch great-minds:jony-ive-designer for strategic visual taste at the executive register.>
```

7. **Save the review** to `design/audits/<slug>.md`. Slug derived from the target path or from `CLAUDE.md`'s `Current spec:` field.

8. **Report:**
   ```
   📝 Saved to design/audits/<slug>.md (review of <path>, <word-count> words).

   Panel:           <persona-1>, <persona-2>, <persona-3>
   Convergence:     <one-line summary of the agreement>
   Highest-leverage change: <one-line summary>

   Next:
   - Address the highest-leverage change
   - Or cross-dispatch great-minds:jony-ive-designer if the question is strategic-direction-shaped
   ```

## What the skill does NOT do

- Does not modify the target. It reviews; the implementer (human or AI) edits.
- Does not approve or reject. It surfaces the review's findings; the orchestrator decides.
- Does not run real user studies. (For empirical research, dispatch `/designers-channel spool` and run an actual moderated session.)
- Does not auto-pick the panel based on opaque heuristics. The default panel is documented; alternatives are documented; the user can override explicitly.

## Notes

- Parallel dispatch means the personas don't see each other's reviews when forming their own. This is intentional — independent verdicts produce stronger signal at consolidation time.
- For very large targets (>2,000 lines, or a whole design system), the dispatch brief should ask each persona to focus on the most-critical sections only — pick a sample, don't try to review the whole thing. Better to do three high-quality reviews of focused sections than three shallow reviews of everything.
- Review fatigue is real. Don't run `/designers-design-review` on every PR; use it for non-trivial changes where the consolidated lens is worth the dispatch cost.
