# Orchestrating great-designers

Notes for AI agents (or humans) running a design project that uses this plugin's skills as sub-agents.

## The core distinction

The designer personas in this plugin are *specialists at the design threshold.* They bring craft for cognitive flows, usability research, design systems, typography, icon work, product discovery, information design — the trade-offs that working designers make every day.

The orchestrator (you, when you are running a project) is *not a specialist.* The orchestrator coordinates: reads the brand brief, the user research, the existing surface, briefs the right designer for the question at hand, integrates the output, ships.

**The single most consequential mistake an orchestrator can make is to write the spec / audit / system doc yourself.** Norman doesn't think the way Rams thinks doesn't think the way Tufte thinks. Generic design prose is generic. The fix is always to dispatch the right persona.

If you find yourself reaching for the Write tool to put a design doc in `design/specs/`, stop. Have you dispatched the right persona for this? If not, that's the next move.

## Who handles what

| Question | Persona to dispatch |
|---|---|
| Why don't users find this? What mental model are they using? | `don-norman-designer` |
| How do we build taste in a design team without flattening it? | `julie-zhuo-designer` |
| What does the user actually do — not what do they say? | `jared-spool-designer` |
| What can we remove? Where does the design call attention to itself? | `dieter-rams-designer` |
| How do we say this in 32×32 pixels? What metaphor carries weight? | `susan-kare-designer` |
| What's the riskiest assumption, and how do we test it cheaply? | `marty-cagan-designer` |
| What does the brand sound like, in type? Where does the system flex? | `paula-scher-designer` |
| What story does this object tell? Who is the user as a person, not a use case? | `tinker-hatfield-designer` |
| What does the data argue? Where is the chart lying with chartjunk? | `edward-tufte-designer` |
| Does this fit the strategic product direction at the executive register? | `great-minds:jony-ive-designer` (cross-plugin) |

When two personas would honestly answer differently, that's a debate (filed for v1.0 as `/designers-debate`).

## A typical orchestration flow

For a design feature:

```
1. Read the project — README, CLAUDE.md, brand brief, existing design system docs, user research notes
2. Read the project bible at .great-authors/ if this is a cross-craft project
3. /designers-project-init   (if design/ doesn't already exist)
4. /designers-write-spec <feature> [--persona <name>]
   → produces design/specs/<slug>.md
5. /designers-design-review design/specs/<slug>.md
   → produces design/audits/<slug>.md (default panel: Norman + Spool + Rams)
6. Iterate on the spec based on review
7. Commit incrementally
```

For a design audit of an existing UI:

```
1. /designers-design-review src/ui/  --personas norman,spool,rams
   → audits/ui-2026-04-27.md
2. Read the audit; address findings
3. Re-audit if substantial changes
```

For a brand / typography question:

```
1. Read the existing brand brief, type scale, voice guide
2. /designers-channel scher
3. Discuss the typography directly; substantive proposals save to design/systems/
4. Optionally /designers-channel rams for a restraint check
```

## Brief-writing as leverage

The single best investment you can make as an orchestrator is writing better briefs.

**A thin brief:**
> "Review this UI."

**A self-contained brief:**
> "Audit the new onboarding flow at `design/onboarding-v3-figma.md` for cognitive load, evidence of usability problems in the recent test sessions (transcripts at `research/2026-04/onboarding-tests/`), and visual restraint. Read these in order: the brand brief at `design/systems/brand.md`, the type scale at `design/systems/type.md`, the test transcripts, the figma spec. The flow was redesigned three weeks ago in response to a 38% drop-off at step 3. Specific concerns: (1) step 3 now has eight form fields where v2 had four; (2) the new color hierarchy has a light-blue CTA on a light-gray background. What to focus on: structural cognitive load and visual hierarchy, not micro-copy. What to leave alone: brand voice (already locked). Save to `design/audits/onboarding-v3-2026-04-27.md`. Report under 400 words on the two highest-leverage changes."

The thin brief produces a generic audit. The self-contained brief produces an audit that can be acted on.

## When to design yourself

Two narrow cases:

1. **Mechanical edits.** Surgical fixes — a typo in a label, a token rename, a single-pixel alignment, a copy update. Surgical, not creative.
2. **The user explicitly asks you to.** *"Just sketch the wireframe here."* Honor that.

In all other cases: dispatch.

## The bible is the spine

For design-heavy projects, the "bible" is the project's design specification:

- `README.md` — what the project does, who the user is
- `CLAUDE.md` — orchestrator-mode notes for AI agents working in the project
- `design/systems/brand.md` — voice, color, type, motion
- `design/systems/components.md` — component library, patterns, tokens
- `design/specs/` — feature-level design specs (this plugin writes here)
- `design/audits/` — design reviews (this plugin writes here)
- User research artifacts — interview transcripts, usability test recordings, analytics

For cross-craft projects (a writing project with an attached app, a film project with a custom interface), `.great-authors/project.md` is also part of the bible. Designer personas read both.

When the design bible is missing, the FIRST move on any new design project is to scaffold one — at minimum `design/systems/brand.md` and a CLAUDE.md note. The bible is the constellation's coordination layer.

## Stall-resolution dispatch (canonical) — Cagan

Marty Cagan lives in this plugin (`great-designers:marty-cagan-designer`) because product discovery is a design-management craft. He is also the **canonical decision-forcer for autonomous pipelines** that have fallen into the analysis-as-progress fallacy — agents generating scout reports, risk matrices, requirements analyses while no code commits land.

When a stall-detector daemon (in a consumer system like Shipyard AI) flags a stalled project, the dispatch shape is:

```js
Agent({
  subagent_type: "great-designers:marty-cagan-designer",
  prompt: "Stall detected for project <slug>. Reasons: <reasons>. Read the analysis docs in <path>. Write a build-or-kill recommendation to <path>/cagan-decision.md. Recommend ONE of: BUILD (with smallest viable scope and the riskiest assumption to test first), PIVOT (with the new framing), or KILL (with the reason the four risks failed). Do NOT defer. Do NOT request more analysis."
})
```

His "four risks" frame (value / usability / feasibility / viability) is the question that ends the analysis loop. *"Which of the four did the analyses test, and was the answer no?"*

**Hard cap (load-bearing):** `max_per_project_per_week: 2`. After two dispatches on the same project without progress, the third is the **escalation to the human**, not another Cagan dispatch. Don't loop.

The pattern is documented in detail in [`great-minds-plugin/docs/STALLED-PIPELINES.md`](https://github.com/sethshoultes/great-minds-plugin/blob/main/docs/STALLED-PIPELINES.md) and worked end-to-end in [`shipyard-ai/docs/PRODUCT-MANAGEMENT-GAP.md`](https://github.com/sethshoultes/shipyard-ai/blob/main/docs/PRODUCT-MANAGEMENT-GAP.md). When designing a consumer system that uses constellation personas in a pipeline, build the stall-detector + Cagan loop as infrastructure in your consumer system; do NOT add it inside this plugin or any other constellation plugin (that would bend the constellation's "channels vs. infrastructure" rule — see `brain/projects/great-minds-ai-company-constellation.md`).

## Cross-plugin orchestration

The designers plugin composes with the rest of the constellation:

- `great-minds:jony-ive-designer` — strategic visual taste at the executive register. When the question is "is this product worth building" rather than "is this UI usable," dispatch Ive.
- `great-minds:steve-jobs-visionary` — product / vision direction when the design question is really a product question
- `great-engineers:dhh-engineer` — when the design constraint is engineering-real (the framework can't render this, the budget for animation is N kb)
- `great-engineers:sandi-metz-engineer` — when the design system is a code-architecture question
- `great-authors:hemingway-persona` — when the UI copy needs muscle and concision
- `great-marketers:rory-sutherland-behavioral` — when the design question is behavioral (why aren't users converting)
- `great-publishers:chip-kidd-designer` — when the design crosses into book or print form

The dispatch syntax: `Agent({subagent_type: "<plugin>:<persona>-persona", ...})`. The orchestrator routes; the personas speak.

## What this plugin does NOT do

- **Build the UI.** This plugin produces specs, audits, system docs. The actual implementation is engineering's. Cross-dispatch to `great-engineers` when the design needs to become code.
- **Replace strategic visual direction.** Jony Ive handles that, in great-minds. Cross-dispatch when the question is "what is this product, visually" at the executive level.
- **Write product copy.** That's `great-marketers`. When the design needs voice/tone work, dispatch back.
- **Design the book cover.** That's `great-publishers:chip-kidd-designer`. When the design work crosses into print/publication form, dispatch.

When a question reaches into a different craft, surface it explicitly: *"This is a behavioral-economics question — let me dispatch Sutherland in great-marketers."* Don't paper over the gap.

## Anti-patterns

These all produce generic design artifacts:

- Writing the spec / audit / system doc yourself instead of dispatching
- Pattern-matching a persona's voice in your own context (Norman's clarity, Rams's restraint, Tufte's density) without dispatching the actual persona
- Skipping the read of the brand brief / research / existing system before dispatching
- Thin briefs ("audit this UI")
- Letting the persona choice get made silently — surface why you picked Norman over Spool, or Rams over Scher
- Designing the UI yourself (beyond the two narrow cases above)

The anti-pattern that catches most orchestrators is the first one. Watch for it.
