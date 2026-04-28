# Orchestrating great-researchers

Notes for AI agents (or humans) running a research project that uses this plugin's skills as sub-agents.

> ⚠️ **NOT ACADEMIC ADVICE.** Personas in this plugin are craft channels in the voice of canonical figures. The plugin is a writing and reasoning tool, not a substitute for peer-reviewed research, a graduate advisor, or a domain expert in your field.

## The core distinction

The researcher personas in this plugin are *specialists at the research threshold.* They bring craft for science communication, essay-as-research, immersive investigation, clinical case studies, multi-disciplinary synthesis, deep narrative research, and investigative biography — the trade-offs that working researchers make every day when deciding what to read, what to cite, and how to present a finding.

The orchestrator (you, when you are running a project) is *not a specialist.* The orchestrator coordinates: reads the question, the prior studies, the bibliography, briefs the right researcher persona for the question at hand, integrates the output, ships.

**The single most consequential mistake an orchestrator can make is to write the study yourself.** Sagan doesn't think the way Caro thinks doesn't think the way Sacks thinks. Generic research prose is generic — and worse, in this domain, generic prose can be subtly misleading because it sounds authoritative. The fix is always to dispatch the right persona.

If you find yourself reaching for the Write tool to put a study in `research/studies/`, stop. Have you dispatched the right persona for this? If not, that's the next move.

## Who handles what

| Question | Persona to dispatch |
|---|---|
| Communicate this scientific finding to a public audience without dumbing it down | `carl-sagan-researcher` |
| Use the essay form to make a research argument; build it like a McPhee piece | `stephen-jay-gould-researcher` |
| Investigate this topic immersively — spend a week in the lab, the morgue, the zero-G plane | `mary-roach-researcher` |
| Write up this case as humanist literature, not as a chart | `oliver-sacks-researcher` |
| Analyze this medical or healthcare system; what's the systems-level diagnosis | `atul-gawande-researcher` |
| Synthesize across geography, biology, history to explain a civilizational outcome | `jared-diamond-researcher` |
| Anchor this synthesis in biology; what does evolution say about this | `edward-o-wilson-researcher` |
| Recover the human story behind this scientific or medical artifact | `rebecca-skloot-researcher` |
| Investigative biography; turn every page; what's actually in the archive | `robert-caro-researcher` |
| Technical-mathematical writing rigor at the academic register | `great-engineers:don-knuth-engineer` (cross-plugin) |
| Political philosophy / political science research register | `great-counsels:hannah-arendt-counsel` (cross-plugin) |

When two personas would honestly answer differently — Gould vs. Wilson on sociobiology is the canonical case — that's a debate (filed for v1.0 as `/researchers-debate`).

## A typical orchestration flow

For a study:

```
1. Read the project — README, CLAUDE.md, prior studies, prior reviews, the actual primary sources
2. Read the bibliography at research/bibliography/ if present
3. Read the project bible at .great-authors/ if cross-craft project
4. /researchers-project-init   (if research/ doesn't already exist)
5. /researchers-write-study <topic> [--persona <name>]
   → produces research/studies/<slug>.md
6. /researchers-review research/studies/<slug>.md
   → produces research/reviews/<slug>.md (default panel: Gould + Sagan + Wilson)
7. Iterate on the study based on review
8. Update bibliography with any new sources cited
9. Commit incrementally
```

For a peer review of an existing study:

```
1. /researchers-review path/to/study.md  --personas gould,gawande
   → reviews/<slug>-2026-04-27.md
2. Read the review; address findings
3. Re-review if substantial changes
```

For an immersive investigation:

```
1. Read the existing literature on the topic
2. /researchers-channel roach
3. Plan the field trip / lab visit / interview list
4. Write up the immersive piece directly with Roach in the room
5. Substantive output saves to research/studies/
```

## Brief-writing as leverage

The single best investment you can make as an orchestrator is writing better briefs to the persona.

**A thin brief:**
> "Write a study on CRISPR ethics."

**A self-contained brief:**
> "Write a 1500-word study on the off-target-effect debate in CRISPR-Cas9 germline editing as it stood in late 2025. Read these sources in order: `research/bibliography/crispr-2025.md` (the annotated bibliography), `research/studies/crispr-history.md` (the prior background piece in this project), and the three Doudna/Charpentier follow-ups linked in the bibliography under 'primary sources.' The study's job: synthesize the off-target debate without flattening it. Specific concerns: (1) the He Jiankui case is too easy a hook and should NOT carry the argument; (2) the Lulu/Nana follow-ups are weakly cited in the prior study and need to be primary-sourced. Format: Gould-style essay (build the argument across the piece, not as an abstract up front). Output: `research/studies/crispr-off-targets-2026.md`. Cite using the bibliography keys; flag any claim that lacks a primary source rather than papering over."

The thin brief produces a generic study. The self-contained brief produces a study that can be peer-reviewed without embarrassment.

## When to write the study yourself

Two narrow cases:

1. **Mechanical edits.** Surgical fixes — a citation correction, a typo, a section header rename. Surgical, not creative.
2. **The user explicitly asks you to.** *"Just sketch the abstract here."* Honor that.

In all other cases: dispatch.

## The bible is the spine

For research-heavy projects, the "bible" is the project's research context:

- `README.md` — the project's research question and scope
- `CLAUDE.md` — orchestrator-mode notes
- `research/bibliography/` — the citation foundation (this plugin writes here, indirectly — studies cite from here)
- `research/studies/` — prior studies (this plugin writes here)
- `research/reviews/` — prior peer reviews (this plugin writes here)
- The actual primary sources — papers, books, archives, interview transcripts — that the studies cite

For cross-craft projects, `.great-authors/project.md` is also part of the bible. Researcher personas read both.

When the research bible is missing, the FIRST move on any new research project is to scaffold one — at minimum a CLAUDE.md and a `research/bibliography/initial.md` with the seed sources. The bibliography IS the spine of a research project; everything cites back to it.

## Cross-plugin orchestration

The researchers plugin composes with the rest of the constellation:

- `great-engineers:don-knuth-engineer` — formal mathematical/technical writing rigor; literate programming
- `great-counsels:hannah-arendt-counsel` — political philosophy research register
- `great-counsels:john-rawls-counsel` — ethical-framework reasoning when the research has policy stakes
- `great-authors:mcphee-persona` — when the research is becoming long-form narrative nonfiction (Gould's lens; can also dispatch McPhee directly)
- `great-authors:gottlieb-persona` — editorial pass on a study before peer review
- `great-minds:steve-jobs-visionary` — when the research finding is also a product-vision question
- `great-minds:phil-jackson-orchestrator` — when synthesizing a multi-persona research panel and they disagree

The dispatch syntax: `Agent({subagent_type: "<plugin>:<persona>-<suffix>", ...})`.

## What this plugin does NOT do

- **Conduct primary research.** Personas can synthesize, write, review, and frame. They cannot run the experiment, conduct the interview, or visit the archive. The "ten-year investigation" register (Skloot, Caro) is a voice you can borrow for a draft, not a research project completed by an LLM.
- **Verify citations.** Personas reference their subjects' published works as part of voice. Treat any quoted line as suggestive, not authoritative; check the source before relying on it. v1.0 will add `/researchers-fact-check <claim>` for single-claim citation verification, but even that is not a substitute for real verification.
- **Replace peer review.** Personas can produce peer-style reviews, but a journal's actual peer-review process involves anonymized double-blind discipline this plugin cannot replicate.
- **Generate hard data or original primary sources.** If your study cites a number, that number must trace to a real primary source. Personas will not invent data, but they may misremember a quote — check.

When a question reaches into another craft, surface it explicitly: *"This is a technical-rigor question — let me dispatch Knuth in great-engineers."* Don't paper over the gap.

## Anti-patterns

These all produce generic research artifacts:

- Writing the study / review / synthesis yourself instead of dispatching
- Pattern-matching a persona's voice in your own context (Sagan's wonder, Gould's essay-form, Caro's investigative grind) without dispatching the actual persona
- Skipping the bibliography read before dispatching
- Thin briefs ("write about CRISPR")
- Letting the persona choice get made silently — surface why you picked Sagan over Sacks, or Wilson over Diamond
- Treating persona output as primary research (the most consequential anti-pattern)
- Skipping the verify-the-subagent-type step before documenting cross-plugin dispatch (lesson from great-operators issue #1)

The anti-pattern that catches most orchestrators is the first one. Watch for it.
