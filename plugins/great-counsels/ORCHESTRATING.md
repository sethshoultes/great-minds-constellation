# Orchestrating great-counsels

Notes for AI agents (or humans) running a project that uses this plugin's skills as sub-agents.

> ⚠️ **NOT LEGAL ADVICE.** This plugin produces craft-level legal/policy/ethics writing in the voice of canonical figures. It is a writing tool and a reasoning lens, not a substitute for licensed counsel. Any decision with real legal stakes requires a real attorney admitted to practice in the relevant jurisdiction.

## The core distinction

The counsel personas in this plugin are *specialists at the legal/policy/ethics threshold.* They bring craft for constitutional reasoning, litigation strategy, originalism, digital jurisprudence, antitrust, privacy, regulatory design, political philosophy, and ethical theory — the trade-offs that working counselors and ethicists make every day.

The orchestrator (you, when you are running a project) is *not a specialist.* The orchestrator coordinates: reads the question, the prior memos, the relevant precedent or framework, briefs the right counsel persona for the question at hand, integrates the output, ships.

**The single most consequential mistake an orchestrator can make is to write the memo / review / brief yourself.** RBG doesn't think the way Scalia thinks doesn't think the way Rawls thinks. Generic legal/policy/ethics prose is generic — and worse, in this domain, generic prose can be misleading in ways that look authoritative. The fix is always to dispatch the right persona.

If you find yourself reaching for the Write tool to put a memo in `counsel/memos/`, stop. Have you dispatched the right persona for this? If not, that's the next move.

## Who handles what

| Question | Persona to dispatch |
|---|---|
| Is this a constitutional civil rights question? What's the precedent strategy? | `ruth-bader-ginsburg-counsel` |
| What's the long-game litigation play, ten years out? | `thurgood-marshall-counsel` |
| What does the text actually say? What did it mean when it was written? | `antonin-scalia-counsel` |
| How does code, market, or norm regulate this — and which lever is the right one? | `lawrence-lessig-counsel` |
| Is this an antitrust / platform-power question? What's the bigness cost? | `tim-wu-counsel` |
| Is this a privacy question? What does "the right to be let alone" require? | `louis-brandeis-counsel` |
| What's the regulatory design? Where is the choice architecture? | `cass-sunstein-counsel` |
| What's the political-philosophy frame? Is this becoming totalitarian? | `hannah-arendt-counsel` |
| Behind the veil of ignorance, would you choose this institution? | `john-rawls-counsel` |
| Is the question really about Stoic interpersonal mediation? | `great-minds:marcus-aurelius-mod` (cross-plugin) |

When two personas would honestly answer differently — RBG vs. Scalia is the canonical case — that's a debate (filed for v1.0 as `/counsels-debate`).

## A typical orchestration flow

For a memo on a question:

```
1. Read the project — README, CLAUDE.md, prior memos, prior reviews, the actual decision being weighed
2. Read the project bible at .great-authors/ if this is a cross-craft project
3. /counsels-project-init   (if counsel/ doesn't already exist)
4. /counsels-write-memo <question> [--persona <name>]
   → produces counsel/memos/<slug>.md
5. /counsels-review counsel/memos/<slug>.md
   → produces counsel/reviews/<slug>.md (default panel: RBG + Lessig + Rawls)
6. Iterate on the memo based on review
7. Commit incrementally
```

For a review of an existing decision or policy:

```
1. /counsels-review policies/data-retention.md  --personas brandeis,sunstein
   → reviews/data-retention-2026-04-27.md
2. Read the review; address findings
3. Re-review if substantial changes
```

For a values / ethics question:

```
1. Read the existing decision context
2. /counsels-channel rawls
3. Discuss the question directly through the veil of ignorance
4. Optionally /counsels-channel arendt for the political-philosophy check
```

## Brief-writing as leverage

The single best investment you can make as an orchestrator is writing better briefs to the persona.

**A thin brief:**
> "Review our privacy policy."

**A self-contained brief:**
> "Review the privacy policy at `counsel/policies/privacy-2026.md` for civil-liberties soundness and platform-power second-order effects. Read these in order: the policy itself, the prior policy at `counsel/archive/privacy-2024.md`, the data-retention runbook at `operations/systems/data-retention.md`, the incident review at `counsel/reviews/march-2026-disclosure.md`. The policy was redrafted because EU regulators flagged the data-retention period as excessive. Specific concerns: (1) the policy claims 'reasonable efforts' on a class of data we don't actually scope; (2) the consent flow at signup buries the third-party-share disclosure six clicks deep. What to focus on: the gap between what the policy promises and what the system delivers. What to leave alone: the marketing register (already brand-approved). Save to `counsel/reviews/privacy-2026-04-27.md`. Report under 400 words on the two highest-leverage changes."

The thin brief produces a generic review that flatters the existing draft. The self-contained brief produces a review that can be acted on.

## When to write the memo yourself

Two narrow cases:

1. **Mechanical edits.** Surgical fixes — a typo, a citation correction, a section header rename. Surgical, not creative.
2. **The user explicitly asks you to.** *"Just sketch the position here."* Honor that.

In all other cases: dispatch.

## The bible is the spine

For counsel-heavy projects, the "bible" is the project's reasoning context:

- `README.md` — what the project does, who it serves, who it might harm
- `CLAUDE.md` — orchestrator-mode notes for AI agents working in the project
- `counsel/memos/` — prior memos (this plugin writes here)
- `counsel/reviews/` — prior reviews (this plugin writes here)
- `counsel/briefs/` — formal positions (this plugin writes here)
- The actual decision, policy, or practice being analyzed

For cross-craft projects (a writing project with a publishing-rights question, a film with a fair-use question, a SaaS with a data-privacy question), `.great-authors/project.md` is also part of the bible. Counsel personas read both.

When the counsel bible is missing, the FIRST move on any new counsel project is to scaffold one — at minimum a CLAUDE.md and a `counsel/memos/initial-question.md`. The bible is the constellation's coordination layer.

## Cross-plugin orchestration

The counsels plugin composes with the rest of the constellation:

- `great-minds:marcus-aurelius-mod` — Stoic executive mediation when the ethical question is also an interpersonal/orchestration one
- `great-minds:steve-jobs-visionary` — product / vision when the question is "is this product worth building" rather than "is this product permissible"
- `great-engineers:dhh-engineer` — when the legal constraint is engineering-real (the system can't sustain this consent flow, the migration is incompatible with the privacy regime)
- `great-designers:don-norman-designer` — when the consent or disclosure question is a cognitive-flow question (does the user actually understand what they consented to)
- `great-operators:patty-mccord-operator` — when the people-ops question is a workplace-law question (and you also need a real lawyer)
- `great-marketers:rory-sutherland-behavioral` — when the policy question is behavioral (why people consent without reading)

The dispatch syntax: `Agent({subagent_type: "<plugin>:<persona>-persona", ...})`. The orchestrator routes; the personas speak.

## What this plugin does NOT do

- **Give legal advice.** The personas are craft channels in the voice of canonical figures. They do not represent the user. They are not admitted to any bar. They are not a substitute for an attorney.
- **File court documents.** Briefs produced by this plugin are persona-voice writing exercises. Filing them in a court is not a use case the plugin supports.
- **Resolve hard moral calls.** Arendt and Rawls personas can reason through ethical frameworks; they cannot tell you what the right thing to do is for your specific situation.
- **Verify quotations or citations.** Personas reference their subjects' published works as part of voice and reasoning. Treat any quoted line as suggestive, not authoritative; check the source before relying on it.

When a question reaches into another craft, surface it explicitly: *"This is an engineering-feasibility question — let me dispatch DHH in great-engineers."* Don't paper over the gap.

## Anti-patterns

These all produce generic counsel artifacts:

- Writing the memo / review / brief yourself instead of dispatching
- Pattern-matching a persona's voice in your own context (RBG's quiet precision, Scalia's sting, Rawls's veil) without dispatching the actual persona
- Skipping the read of the prior memos / decision / framework before dispatching
- Thin briefs ("review this policy")
- Letting the persona choice get made silently — surface why you picked Brandeis over Wu, or Rawls over Arendt
- Treating persona output as legal advice (the most consequential anti-pattern in this plugin)

The anti-pattern that catches most orchestrators is the first one. Watch for it.
