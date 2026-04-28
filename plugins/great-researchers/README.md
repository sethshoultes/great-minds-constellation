# Great Researchers

Nine researcher personas (Carl Sagan, Stephen Jay Gould, Mary Roach, Oliver Sacks, Atul Gawande, Jared Diamond, E. O. Wilson, Rebecca Skloot, Robert Caro) and four operational skills for studies, peer reviews, and project initialization. A Claude Code plugin.

Part of the [Great Minds constellation](https://github.com/sethshoultes/great-minds-constellation) — 10 plugins for different craft domains.

> **New to the constellation?** Start with [`/constellation-start`](https://github.com/sethshoultes/great-minds-plugin) in `great-minds` — it asks 2-3 questions about your project shape and routes to the right plugin.

> ⚠️ **NOT ACADEMIC ADVICE.** Personas in this plugin are craft channels in the voice of canonical figures. The plugin is a writing and reasoning tool, not a substitute for peer-reviewed research, a graduate advisor, or a domain expert in your specific field.

## Install

```
/plugin marketplace add sethshoultes/great-minds-constellation
/plugin install great-researchers@great-minds-constellation
```

**Claude Desktop** (DXT bundle):
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## What's in v0.1

### 9 Personas — at the research threshold

| Persona | Strength |
|---|---|
| `carl-sagan-researcher` | Astronomer; *Cosmos*, *The Demon-Haunted World*, *Pale Blue Dot*. Science communication as discipline; the wonder + the skepticism in the same sentence. |
| `stephen-jay-gould-researcher` | Paleontologist; 27 years of monthly essays in *Natural History*; *Wonderful Life*, *The Mismeasure of Man*. The essay AS research. |
| `mary-roach-researcher` | Immersive popular science; *Stiff*, *Bonk*, *Packing for Mars*. Curious-but-irreverent investigation; the field trip as research method. |
| `oliver-sacks-researcher` | Neurologist; *Awakenings*, *The Man Who Mistook His Wife for a Hat*, *Musicophilia*. Clinical case as humanist literature. |
| `atul-gawande-researcher` | Surgeon; *Being Mortal*, *The Checklist Manifesto*, *Complications*. Medical research as systems thinking + ethics. |
| `jared-diamond-researcher` | Geographer/biologist; *Guns, Germs, and Steel*, *Collapse*, *The Third Chimpanzee*. Multi-disciplinary synthesis at civilizational scale. |
| `edward-o-wilson-researcher` | Biologist; *Sociobiology*, *Consilience*, *On Human Nature*. The unity-of-knowledge proposal; biology as the through-line. |
| `rebecca-skloot-researcher` | *The Immortal Life of Henrietta Lacks*. Deep-research narrative; the ten-year investigation that recovers a person erased from medical history. |
| `robert-caro-researcher` | *The Power Broker*, the LBJ biographies. Investigative obsession over decades; "turn every page." |

### How the personas were drafted

The v0.1 persona files were drafted via cross-plugin orchestration — each researcher written by a great-authors persona whose register fits the subject (King on Sagan, McPhee on Gould, Vonnegut on Roach, Morrison on Sacks, Didion on Gawande, Wallace on Diamond, Le Guin on Wilson, Baldwin on Skloot, Hemingway on Caro). Then `great-authors:gottlieb-persona` did a threshold pass and named cuts. **Sixth and final production use of the constellation pattern** at v0.1 (after marketers, engineers, designers, operators, counsels). The pattern works.

### Cross-dispatch hints (two)

For technical-mathematical writing rigor at the academic register, cross-dispatch:

```
Agent({
  subagent_type: "great-engineers:don-knuth-engineer",
  prompt: "<self-contained brief>"
})
```

For political-philosophy research register (the *Origins of Totalitarianism* / *Eichmann in Jerusalem* mode), cross-dispatch:

```
Agent({
  subagent_type: "great-counsels:hannah-arendt-counsel",
  prompt: "<self-contained brief>"
})
```

The pattern: research is broad enough that adjacent plugins (engineers for formal/mathematical rigor; counsels for political-philosophy register) carry registers this plugin doesn't replicate. **Compose; don't duplicate** — the constellation rule.

### 4 MVP Skills

| Skill | What it does |
|---|---|
| `/researchers-channel <persona>` | Load a researcher persona into your current conversation. Substantive output (studies, reviews, syntheses) auto-saves to `research/<artifact-type>/<slug>.md`. |
| `/researchers-project-init` | Scaffold a `research/` directory at the project root, sibling to the other constellation plugin directories. Subdirs: `studies/`, `reviews/`, `bibliography/`. |
| `/researchers-write-study <topic>` | Produces a study (essay / paper / literature review / case study / investigation), persona-driven register. Default Sagan for science communication, Gould for essay-as-research, Roach for immersive investigation, Sacks for clinical case, Gawande for medical-systems analysis, Diamond for synthesis, Wilson for biology-anchored synthesis, Skloot for deep-research narrative, Caro for investigative biography. Override available. Output: `research/studies/<slug>.md`. |
| `/researchers-review <path>` | Dispatches persona(s) to peer-review an existing study, claim, or research artifact. Default panel for parallel review: Gould (synthesis), Sagan (skeptical inquiry), Wilson (consilience). Override available. Output: `research/reviews/<slug>.md`. |

## Why this plugin

The constellation could create artifacts (prose, film, publication, marketing copy), build software, design product surfaces, operate the business, and counsel on legal/policy/ethics — but couldn't yet **conduct research with citation rigor**. Research is its own register: distinct from prose (great-authors), distinct from policy memos (great-counsels), distinct from technical specs (great-engineers). Closer to McPhee's nonfiction craft, but with the citation discipline of the academy and the synthesis instinct of the public-science writer.

great-researchers fills the final v0.1 gap. Sagan covers science communication, Gould covers essay-as-research, Roach covers immersive investigation, Sacks covers clinical humanism, Gawande covers medical systems, Diamond covers civilizational synthesis, Wilson covers biology-anchored consilience, Skloot covers deep narrative research, Caro covers investigative biographical obsession. Different registers, different methods, different reading-before-writing protocols.

## ⚠️ Limits and disclaimers

- **Not academic advice.** Personas produce craft-level studies in the voice of canonical figures. Do not submit persona output to a journal as your own; do not use it as a substitute for primary research with a domain expert.
- **Not citation-checked.** Personas reference their subjects' published works as part of voice. Treat any quoted line as suggestive, not authoritative; check the source before relying on it.
- **Not a substitute for the work.** The "ten-year investigation" register (Skloot, Caro) is a voice you can borrow for a draft, not a research project completed by an LLM.

## Conventions inherited from the constellation

- **Orchestrator vs. specialist.** Personas are dispatched as sub-agents in clean contexts.
- **Default-save.** Every generative skill saves output to disk before showing it in chat. Save to `research/<subdir>/<slug>.md`.
- **Bible reading.** Every persona reads the project's specification before deciding — `README.md`, `CLAUDE.md`, prior studies and reviews, the actual sources being cited. For cross-craft projects, also `.great-authors/project.md`.
- **Cross-plugin orchestration.** When a research question reaches into another craft, dispatch the right plugin's persona. Compose freely; don't duplicate.

## Project structure

```
.great-authors/                 # the bible — shared with great-authors (when present)
manuscript/                     # great-authors writes here (when present)
film/                           # great-filmmakers writes here (when present)
publishers/                     # great-publishers writes here (when present)
marketing/                      # great-marketers writes here (when present)
engineering/                    # great-engineers writes here (when present)
design/                         # great-designers writes here (when present)
operations/                     # great-operators writes here (when present)
counsel/                        # great-counsels writes here (when present)
research/                       # great-researchers writes here (this plugin)
├── studies/                    #   essays, papers, lit reviews, case studies, investigations
├── reviews/                    #   peer-style reviews of existing studies
└── bibliography/               #   citations, sources, annotated bibliographies
```

The `bibliography/` subdir is the load-bearing addition compared to other plugins — citation discipline lives separately from the studies that reference them.

## Roadmap

- **v0.1** (this release) — nine personas, four MVP skills, DXT bundle. **Completes the constellation v0.1.**
- **v1.0** — Add `/researchers-write-bibliography <topic>` (annotated bibliography), `/researchers-debate <topic> <a> <b>` (mirror `/authors-debate`), `/researchers-critique <path>` (fast 3-bullet verdict), `/researchers-fact-check <claim>` (single-claim citation verification — still NOT a substitute for real verification), `/researchers-edit <file>` (multi-persona marked-up review). ~12 skills total.

## License

MIT. See [LICENSE](./LICENSE).
