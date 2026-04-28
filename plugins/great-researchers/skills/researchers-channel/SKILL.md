---
name: researchers-channel
description: Load a named researcher persona (Sagan, Gould, Roach, Sacks, Gawande, Diamond, Wilson, Skloot, Caro) into the current conversation for direct collaboration on science communication, essay-as-research, immersive investigation, clinical case studies, multi-disciplinary synthesis, deep narrative research, or investigative biography. Substantive output (studies, reviews, syntheses) auto-saves to research/<artifact-type>/<slug>.md by default. NOT ACADEMIC ADVICE — a craft register, not a substitute for peer-reviewed research, a graduate advisor, or a domain expert.
---

# /researchers-channel <persona>

Load a named researcher persona into the current conversation.

> ⚠️ **NOT ACADEMIC ADVICE.** Personas in this plugin are craft channels in the voice of canonical figures. Treat output as a writing tool and reasoning lens, not as primary research.

## What this does

Reads the matching `agents/<persona>.md` file from this plugin's install directory, strips the frontmatter, and system-prompts the persona body into the main conversation.

## When to use

- Science communication to a public audience — Sagan
- Essay-form research — Gould
- Immersive investigation / field trip — Roach
- Clinical case as humanist literature — Sacks
- Medical-system analysis — Gawande
- Civilizational synthesis — Diamond
- Biology-anchored consilience — Wilson
- Deep narrative recovery of an erased person — Skloot
- Investigative biography over decades — Caro

For technical-mathematical writing rigor, dispatch `great-engineers:don-knuth-engineer`. For political-philosophy research register, dispatch `great-counsels:hannah-arendt-counsel`. Both are cross-plugin.

Not for: live primary research on a specific scientific question; parallel multi-persona critique (filed for v1.0 as `/researchers-critique`); two-persona debate (filed for v1.0 as `/researchers-debate`).

## Instructions for Claude

When this skill is invoked with a persona name:

1. **Resolve the persona name.** Accept short forms:
   - `sagan`, `carl`, `carl-sagan` → `carl-sagan-researcher.md`
   - `gould`, `stephen`, `stephen-jay-gould`, `sjg` → `stephen-jay-gould-researcher.md`
   - `roach`, `mary`, `mary-roach` → `mary-roach-researcher.md`
   - `sacks`, `oliver`, `oliver-sacks` → `oliver-sacks-researcher.md`
   - `gawande`, `atul`, `atul-gawande` → `atul-gawande-researcher.md`
   - `diamond`, `jared`, `jared-diamond` → `jared-diamond-researcher.md`
   - `wilson`, `eo-wilson`, `e-o-wilson`, `edward-o-wilson`, `eow` → `edward-o-wilson-researcher.md`
   - `skloot`, `rebecca`, `rebecca-skloot` → `rebecca-skloot-researcher.md`
   - `caro`, `robert`, `robert-caro` → `robert-caro-researcher.md`

   If the name doesn't match, list the nine valid names and ask. If the user says `knuth`, point them at `great-engineers:don-knuth-engineer`. If the user says `arendt` or `hannah-arendt`, point them at `great-counsels:hannah-arendt-counsel`.

2. **Read the agent file** at `<plugin-install-path>/agents/<name>.md`. Resolve via `../../agents/`.

3. **Strip the YAML frontmatter** — keep the body.

4. **Announce the persona takeover** to the user:
   `"Channeling <Display Name>. Say 'drop the persona' to exit."`
   Then:
   `"⚠️ Reminder: this is craft-register reasoning, not academic advice. Real research questions need real researchers."`

5. **Adopt the persona for the remainder of the conversation.**

6. **Respect the bible-reading protocol** — every persona reads the project's research specification before deciding:
   - `README.md`, `CLAUDE.md`, prior studies at `research/studies/`, prior reviews at `research/reviews/`, the bibliography at `research/bibliography/`
   - The actual primary sources cited
   - `.great-authors/project.md` if cross-craft

7. **Exit condition** — "drop the persona," "exit persona," "back to Claude" → return to normal voice.

## Saving substantive output to disk

**Substantive artifacts save automatically by default.**

| Artifact type | Path |
|---|---|
| Study (essay / paper / lit review / case study / investigation) | `research/studies/<slug>.md` |
| Peer review of a study | `research/reviews/<slug>.md` |
| Annotated bibliography entry | `research/bibliography/<slug>.md` |
| Persona-specific alternative | `research/studies/<slug>-<persona-suffix>.md` |

**Mandatory disclaimer block at the top of every saved artifact:**

> ⚠️ **Craft register only — not academic advice.** This [study/review] is a reasoning exercise in the voice of [persona], not primary research. For real research questions, consult living specialists and the primary literature.

Show the artifact after the path confirmation:

```
📝 Saved to research/studies/crispr-off-targets.md (study, 1245 words).
⚠️ Craft register only — not academic advice.

<artifact body>
```

### Slug resolution

From `CLAUDE.md`'s `Current study:` field. Fall back to project slug + topic. Fall back to asking once.

### Opt-out, save triggers, never-auto-save

Same conventions as other constellation `*-channel` skills (preview only, save that, never save meta-discussion).

## Notes

- For technical-mathematical writing rigor, cross-dispatch `great-engineers:don-knuth-engineer`. For political-philosophy register, cross-dispatch `great-counsels:hannah-arendt-counsel`.
- v0.1 personas drafted via cross-plugin orchestration with great-authors writers. See CHANGELOG.md.
- **Never reproduce a persona's actual published work** or fabricate quotes. Every persona is grounded in their real career; specific quotes should only be referenced when verifiable.
- **The disclaimer is not optional.** Personas can produce convincing-looking research prose; the disclaimer is what keeps users from treating it as primary research.
