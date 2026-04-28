---
name: researchers-write-study
description: Produce a study (essay / paper / literature review / case study / investigation) on a topic. Reads the project specification (README, CLAUDE.md, prior studies and reviews, bibliography, primary sources), then dispatches a researcher persona to draft the study in their register. Default persona auto-selected by signal — Sagan for science communication, Gould for essay-as-research, Roach for immersive, Sacks for clinical, Gawande for systems, Diamond for civilizational synthesis, Wilson for biology-anchored, Skloot for deep narrative, Caro for investigative biography. Override with --persona. Output saves to research/studies/<slug>.md. NOT ACADEMIC ADVICE — a craft register.
---

# /researchers-write-study <topic> [--persona <name>]

Produce a study on a research topic.

> ⚠️ **NOT ACADEMIC ADVICE.** This skill produces craft-register writing in the voice of canonical figures. It is a writing tool and a reasoning lens, not primary research.

## What this does

This skill is the research equivalent of `/engineers-write-spec` or `/counsels-write-memo` — the upstream artifact that downstream work obeys. The study establishes the question, the method, the primary sources, the analysis, the synthesis, the caveats, and the bibliography references.

The study serves three downstream consumers:

1. The reader (other researchers, the public, students) — they read the study and learn.
2. The peer reviewer (you, the orchestrator, or a peer) — they check the analysis.
3. The future reader — months or years later, they read the study to understand the framing.

## When to use

- A research topic is in front of the project and benefits from being captured in writing.
- A literature review needs synthesizing.
- A case study needs writing up.
- An investigative deep-dive needs framing.

Not for: actual primary research; line-level citation nits (use `/researchers-review`); craft conversations about a single concept (use `/researchers-channel <persona>`).

## Instructions for Claude

When this skill is invoked with a `<topic>` argument and optional `--persona`:

1. **Resolve the project root.** Verify `research/` exists; if not, recommend running `/researchers-project-init` first.

2. **Read the project specification:**
   - `CLAUDE.md` — orchestrator-mode notes, current study slug
   - `README.md` — what the project does
   - Prior studies at `research/studies/`
   - Prior reviews at `research/reviews/`
   - Bibliography at `research/bibliography/`
   - The actual primary sources the topic concerns
   - `.great-authors/project.md` if cross-craft

3. **Resolve the persona to dispatch.** Auto-select by signal:

   | Signal | Default persona |
   |---|---|
   | "communicate", "wonder", "skepticism", "Baloney Detection" | `carl-sagan-researcher` |
   | "essay form", "synthesize", "Natural History column style" | `stephen-jay-gould-researcher` |
   | "immersive", "field trip", "go visit the lab" | `mary-roach-researcher` |
   | "case study", "clinical", "patient as person" | `oliver-sacks-researcher` |
   | "medical system", "checklist", "healthcare ethics" | `atul-gawande-researcher` |
   | "civilizational synthesis", "geography + biology", "Yali's question" | `jared-diamond-researcher` |
   | "biology-anchored", "consilience", "myrmecology" | `edward-o-wilson-researcher` |
   | "deep research", "10-year investigation", "recover a person" | `rebecca-skloot-researcher` |
   | "investigative biography", "turn every page", "the archive" | `robert-caro-researcher` |
   | None of the above | `stephen-jay-gould-researcher` (essay-as-research default) |

4. **Dispatch the persona** via the Agent tool: `subagent_type: "great-researchers:<persona-slug>-researcher"`. Brief includes:
   - Topic, paths to bible files read, primary sources, prior studies/reviews
   - Output target: `research/studies/<slug>.md`
   - Required structure (below)
   - Length target: 800-2,000 words
   - **Mandatory disclaimer block at the top**

5. **Output structure:**

```markdown
---
title: <Study title>
slug: <slug>
persona: <persona-slug>
created: YYYY-MM-DD
status: draft | proposed | accepted | superseded
---

# Research study: <Title>

> ⚠️ **Craft register only — not academic advice.** This study is a reasoning exercise in the voice of <persona>, not primary research. For real research questions, consult living specialists and the primary literature.

## Question

<One paragraph. The specific research question.>

## Method

<One paragraph. How the persona approached the question — Sagan-style framing, Gould-essay structure, Roach-immersive, Sacks-clinical, Gawande-systems, Diamond-synthesis, etc.>

## Primary sources

<Bulleted list. The sources cited or referenced. Each links to or names a real primary source. Flag uncertain provenance honestly.>

## Analysis

<2-4 paragraphs. The actual reasoning. Show the work. Walk the reader through the chain of evidence.>

## Synthesis

<One paragraph. The persona's conclusion, with reasoning compressed.>

## Caveats and limits

<Bulleted list. What the study does NOT establish. What further research would require. **Always include: this is craft-register reasoning, not primary research.**>

## Bibliography references

<Bulleted list. The bibliography keys (from `research/bibliography/`) the study cites. If a source is not yet in the bibliography, flag it for inclusion.>
```

6. **Save** to `research/studies/<slug>.md`. Ask before overwriting.

7. **Report:**
   ```
   📝 Saved to research/studies/<slug>.md (<word-count> words, drafted by <persona>).
   ⚠️ Craft register only — not academic advice.

   Question:    <one-line summary>
   Synthesis:   <one-line summary>

   Next:
   - /researchers-review research/studies/<slug>.md to dispatch a peer-review panel
   - /researchers-channel <other-persona> to refine specific sections
   - For real research stakes: consult living specialists and the primary literature.
   ```

## What the skill does NOT do

- **Conduct primary research.** Personas can synthesize, write, frame. They cannot run the experiment, conduct the interview, or visit the archive.
- **Verify citations.** Personas reference subjects' published works; treat any quoted line as suggestive, not authoritative.
- **Generate hard data.** Personas will not invent data, but may misremember a quote — check.

## Notes

- The study is a reasoning instrument. If the synthesis conflicts with the user's intuition, the conflict is information.
- A project may have multiple studies over time. Each gets its own slug.
- For annotated-bibliography work, use `--persona caro` and request the bibliography format. Filed for v1.0 as `/researchers-write-bibliography`.
- **The disclaimer block is not optional.**
