---
name: counsels-write-memo
description: Produce a legal / policy / ethics memo on a specific question. Reads the project specification (README, CLAUDE.md, prior memos and reviews, the question context), then dispatches a counsel persona to draft the memo in their register. Default persona auto-selected by signal — RBG for civil rights, Marshall for litigation strategy, Scalia for textualism, Lessig for digital law, Wu for antitrust/platform, Brandeis for privacy, Sunstein for regulatory design, Arendt for political philosophy, Rawls for ethical reasoning. Override with --persona. Output saves to counsel/memos/<slug>.md. NOT LEGAL ADVICE — a craft register.
---

# /counsels-write-memo <question> [--persona <name>]

Produce a legal, policy, or ethics memo on a specific question.

> ⚠️ **NOT LEGAL ADVICE.** This skill produces craft-register writing in the voice of canonical figures. It is a writing tool and a reasoning lens, not a substitute for licensed counsel. Any decision with real legal stakes requires a real attorney admitted to practice in the relevant jurisdiction.

## What this does

This skill is the counsel equivalent of `/engineers-write-spec` — the upstream artifact that downstream work obeys. Before a decision is made, the memo establishes the question, the relevant facts, the applicable framework, the analysis, the conclusion, and the caveats and limits.

The memo serves three downstream consumers:

1. The decision-maker — they read the memo and decide.
2. The reviewer (a peer, a senior, a real lawyer if one is being engaged) — they check the analysis.
3. The future reader — months or years later, they read the memo to understand why a given choice was made.

## When to use

- A specific legal, policy, or ethics question is in front of the project and the analysis benefits from being captured in writing.
- Multiple framings are possible and the team needs to commit to one (or to capture the disagreement).
- An ethical question requires reasoning through a framework rather than gut feel.
- A regulatory question needs choice-architecture or behavioral analysis.

Not for: live legal advice on a real matter (the disclaimer is not optional); line-level policy nits (use `/counsels-review`); craft conversations about a single concept (use `/counsels-channel <persona>`); auto-generated documentation.

## Instructions for Claude

When this skill is invoked with a `<question>` argument and optional `--persona`:

1. **Resolve the project root** the same way `/counsels-project-init` does. Verify `CLAUDE.md` (or `README.md`) exists and the `counsel/` directory is present. If `counsel/` is missing, recommend running `/counsels-project-init` first; do not auto-create.

2. **Read the project specification:**
   - `CLAUDE.md` — orchestrator-mode notes, current memo slug, conventions
   - `README.md` — what the project does, who it serves
   - Prior memos at `counsel/memos/`
   - Prior reviews at `counsel/reviews/`
   - The actual decision, policy, or practice the question concerns
   - `.great-authors/project.md` if cross-craft project

3. **Resolve the persona to dispatch.** If `--persona` is given, use it. Otherwise auto-select by signal:

   | Signal in the question / project | Default persona |
   |---|---|
   | "civil rights", "equal protection", "discrimination", constitutional | `ruth-bader-ginsburg-counsel` |
   | "litigation strategy", "long game", "precedent", "test case" | `thurgood-marshall-counsel` |
   | "textualist", "originalist", "what does the text say", statutory | `antonin-scalia-counsel` |
   | "code as law", "regulation by code", digital, "four modalities" | `lawrence-lessig-counsel` |
   | "antitrust", "monopoly", "platform power", "net neutrality" | `tim-wu-counsel` |
   | "privacy", "right to be let alone", surveillance, data | `louis-brandeis-counsel` |
   | "regulation", "rule-making", "nudge", "choice architecture" | `cass-sunstein-counsel` |
   | "totalitarianism", "banality of evil", "action vs behavior" | `hannah-arendt-counsel` |
   | "veil of ignorance", "justice as fairness", "original position" | `john-rawls-counsel` |
   | None of the above | `john-rawls-counsel` (the ethical-reasoning default) |

   Document the choice in the memo's frontmatter.

4. **Dispatch the persona** via the Agent tool with `subagent_type: "great-counsels:<persona-slug>-counsel"`. The brief must include:
   - The question (the user's `<question>` argument, plus any context)
   - All bible files read above (paths only — the persona reads them)
   - The actual decision, policy, or practice the question concerns
   - Prior memos and reviews that inform this question
   - The output target: `counsel/memos/<slug>.md`
   - The required structure (below)
   - Length target: 600-1,200 words
   - **Mandatory disclaimer block at the top of the artifact**

5. **The output structure:**

```markdown
---
title: <Memo title>
slug: <slug>
persona: <persona-slug>
created: YYYY-MM-DD
status: draft | proposed | accepted | superseded
---

# Counsel memo: <Title>

> ⚠️ **Craft register only — not legal advice.** This memo is a reasoning exercise in the voice of <persona display name>, not a representation by counsel. For matters with real legal stakes, retain an attorney admitted in the relevant jurisdiction.

## Question

<One paragraph. The specific question the memo answers.>

## Relevant facts

<Bulleted list. The facts that bear on the question. Honest about what is known and what is assumed.>

## Applicable framework

<2-3 paragraphs. The legal/policy/ethical framework the persona is applying. RBG would apply equal-protection doctrine; Lessig would apply the four modalities; Rawls would apply justice as fairness. Name the framework and its load-bearing parts.>

## Analysis

<2-4 paragraphs. The actual reasoning — apply the framework to the facts. Show the work. Cite the cases, principles, or arguments the persona would cite.>

## Conclusion

<One paragraph. The persona's verdict, with the reasoning compressed.>

## Caveats and limits

<Bulleted list. What the memo does NOT decide. What further analysis would require. **Always include: this is craft-register reasoning, not legal advice.** What facts, if different, would change the conclusion.>
```

6. **Save the doc** to `counsel/memos/<slug>.md`. If the file exists, ask the user whether to overwrite, save as `<slug>-v2.md`, or skip.

7. **Report:**
   ```
   📝 Saved to counsel/memos/<slug>.md (<word-count> words, drafted by <persona>).
   ⚠️ Craft register only — not legal advice.

   Question:    <one-line summary>
   Conclusion:  <one-line summary>

   Next:
   - /counsels-review counsel/memos/<slug>.md to dispatch a panel for review
   - /counsels-channel <other-persona> to refine specific sections (e.g., Scalia on the textualist counter-reading, Wu on the platform-power dimension)
   - For real legal stakes: retain a licensed attorney in the relevant jurisdiction.
   ```

## What the skill does NOT do

- **Give legal advice.** The persona is a craft channel; the memo is a reasoning exercise. It does not represent the user.
- **File a brief.** Memos produced by this skill are persona-voice writing exercises. Filing them in a court is not a use case the plugin supports.
- **Resolve hard moral calls.** Arendt and Rawls personas can reason through ethical frameworks; they cannot tell the user what the right thing to do is for the user's specific situation.
- **Verify quotations or citations.** Personas reference their subjects' published works as part of voice. Treat any quoted line as suggestive, not authoritative; check the source before relying on it.

## Notes

- The memo is a contract on reasoning, not on action. If the conclusion conflicts with the user's intuition, the conflict is information; the next move is a real lawyer or ethicist, not a redraft.
- A project may have multiple memos over time (one per question). Each gets its own slug — set via `CLAUDE.md`'s `Current memo:` field.
- For veil-of-ignorance analysis specifically, use `--persona rawls` and request the veil format explicitly. Filed for v1.0 as `/counsels-veil-of-ignorance`.
- **The disclaimer block is not optional.** Do not strip the "Craft register only — not legal advice" line from any memo.
