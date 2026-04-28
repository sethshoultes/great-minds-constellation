---
name: designers-write-spec
description: Produce a design spec / IA doc / interaction spec for a feature, surface, or system change. Reads the project specification (README, CLAUDE.md, brand brief, design system, user research), then dispatches a designer persona to draft the spec in their register. Default persona auto-selected by signal — Norman for cognitive flows, Spool for usability, Rams for visual restraint, Tufte for data UI, etc. Override with --persona. Output saves to design/specs/<slug>.md. Use when a feature or surface needs a structured proposal before build.
---

# /designers-write-spec <feature> [--persona <name>]

Produce a design spec for a feature, surface, or system change.

## What this does

This skill is the design equivalent of `/engineers-write-spec` — the upstream artifact that downstream work obeys. Before pixels are pushed or code is written, the spec establishes what's being designed, what user is being served, what constraints apply, what alternatives were considered, what trade-offs were accepted, and what's still open.

The spec serves three downstream consumers:

1. The implementer (engineer or designer) — they read the spec and build accordingly.
2. The reviewer — they read both spec and artifact and check whether the implementation matches the proposal.
3. The future maintainer — they read the spec years later to understand why a given choice was made.

## When to use

- A feature, surface, or system change is non-trivial enough that an explicit proposal makes the work easier to review and the future easier to navigate.
- Multiple design directions are possible and the team needs to decide between them.
- A new flow, new component, or new design-system token needs a load-bearing rationale captured before commitment.

Not for: line-level UI nits (use `/designers-design-review`); craft conversations about a single screen (use `/designers-channel <persona>`); auto-generated documentation.

## Instructions for Claude

When this skill is invoked with a `<feature>` argument and optional `--persona`:

1. **Resolve the project root** the same way `/designers-project-init` does. Verify `CLAUDE.md` (or `README.md`) exists and the `design/` directory is present. If `design/` is missing, recommend running `/designers-project-init` first; do not auto-create.

2. **Read the project specification:**
   - `CLAUDE.md` — orchestrator-mode notes, current spec slug, conventions
   - `README.md` — what the project does, who it's for
   - Brand brief at `design/systems/brand.md` (or `BRAND.md`) — voice, color, type, motion
   - Existing design system docs at `design/systems/` — components, tokens, patterns
   - Existing audits at `design/audits/` — what's been observed about the current surface
   - User research artifacts (interview notes, usability transcripts, analytics summaries)
   - `.great-authors/project.md` if cross-craft project
   - The relevant existing UI (paths the user references, or screenshots in `design/audits/`)

3. **Resolve the persona to dispatch.** If `--persona` is given, use it. Otherwise auto-select by signal:

   | Signal in the feature description / project | Default persona |
   |---|---|
   | "cognitive flow", "mental model", "affordance", "user is confused", door problem | `don-norman-designer` |
   | "usability test", "research", "what do users actually do", evidence | `jared-spool-designer` |
   | "design management", "team", "1:1", "calibration", "hiring" | `julie-zhuo-designer` |
   | "restraint", "minimalist", "industrial", "less but better", form | `dieter-rams-designer` |
   | "icon", "glyph", "favicon", "32x32", "16x16", pixel-grid | `susan-kare-designer` |
   | "discovery", "four risks", "opportunity solution tree", outcomes | `marty-cagan-designer` |
   | "typography", "identity", "brand voice", "wordmark", "poster" | `paula-scher-designer` |
   | "physical product", "wearable", "hardware", "athlete", signature object | `tinker-hatfield-designer` |
   | "chart", "dashboard", "data UI", "small multiple", "data-ink" | `edward-tufte-designer` |
   | None of the above | `don-norman-designer` (the cognitive default) |

   Document the choice in the spec's frontmatter.

4. **Dispatch the persona** via the Agent tool with `subagent_type: "great-designers:<persona-slug>-designer"`. The brief must include:
   - The feature description (the user's `<feature>` argument, plus any context the user provided)
   - All bible files read above (paths only — the persona reads them)
   - The relevant existing UI (paths, screenshots, design-system tokens)
   - Any prior research that informs this decision
   - The output target: `design/specs/<slug>.md`
   - The required structure (below)
   - Length target: 600-1,200 words

5. **The output structure:**

```markdown
---
title: <Feature/surface name>
slug: <slug>
persona: <persona-slug>
created: YYYY-MM-DD
status: draft | proposed | accepted | implemented | superseded
---

# Design spec: <Title>

## Problem

<One paragraph. What is the problem? Who has it? Why now? Name the user, not the demographic.>

## User

<One paragraph. The named user (or the closest specific approximation): what they're trying to do, what they bring, what context they're in. The mental model they arrive with.>

## Constraints

<Bulleted list. Real constraints — brand, accessibility, technical, deadline, budget. The constraints are part of the design, not an afterthought.>

## Proposal

<2-4 paragraphs. The proposed solution, described concretely enough that the implementer can build it. Include flow diagrams, key states, copy where it carries the design, components used.>

## Alternatives considered

<2-4 alternatives with one paragraph each. What was rejected and why. The rejections are part of the proposal's defense.>

## Trade-offs

<Bulleted list. What the proposal costs in exchange for what it buys. Honest accounting, not advocacy.>

## Decision

<One paragraph. The chosen path, the reasoning in one or two sentences, the criterion that would invalidate the decision.>

## Open questions

<Bulleted list. Things the spec does not yet answer. The implementer is allowed to make these calls; the spec author is acknowledging the gaps.>
```

6. **Save the doc** to `design/specs/<slug>.md`. If the file exists, ask the user whether to overwrite, save as `<slug>-v2.md`, or skip.

7. **Report:**
   ```
   📝 Saved to design/specs/<slug>.md (<word-count> words, drafted by <persona>).

   Problem:    <one-line summary>
   Decision:   <one-line summary>

   Next:
   - /designers-design-review design/specs/<slug>.md to dispatch a panel for review
   - /designers-channel <other-persona> to refine specific sections (e.g., Tufte on the data UI portion, Scher on the typography)
   ```

## What the skill does NOT do

- Does not implement the UI. The spec describes; the implementer builds. (For UI implementation, dispatch `great-engineers`.)
- Does not deploy, ship, or run. It produces a doc.
- Does not auto-decide between alternatives — the persona presents alternatives, names trade-offs, recommends. The user (or the orchestrator) makes the call.
- Does not invent constraints. Every constraint in the spec must trace to the brand brief, the design system, the manifest, prior research, or stated user requirements. No hallucinated brand requirements.

## Notes

- The spec is the contract. If the implementation diverges from the spec, fix the spec first; then continue. Don't let the UI silently overwrite the design.
- A project may have multiple specs over time (one per surface or system change). Each gets its own slug — set via `CLAUDE.md`'s `Current spec:` field.
- For accessibility-focused specs, use `--persona norman` and request the a11y format explicitly. Filed for v1.0 as `/designers-accessibility-audit`.
