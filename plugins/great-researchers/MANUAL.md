# Great Researchers — User Manual

Complete reference for the `great-researchers` Claude Code plugin. For the executive summary, see [README.md](./README.md). For orchestration patterns, see [ORCHESTRATING.md](./ORCHESTRATING.md).

> ⚠️ **NOT ACADEMIC ADVICE.** Personas are craft channels, not a substitute for peer-reviewed research, a graduate advisor, or a domain expert.

## 1. Install

```
/plugin marketplace add sethshoultes/great-researchers-plugin
/plugin install great-researchers@sethshoultes
```

For Claude Desktop:
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## 2. The nine personas

### Science communication

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `carl-sagan-researcher` | Cornell astronomer; *Cosmos*, *The Demon-Haunted World*, *Pale Blue Dot* | Science communication to a public audience; the wonder + the skepticism in the same sentence; baloney-detection-kit reasoning |

### Essay-as-research

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `stephen-jay-gould-researcher` | Harvard paleontologist; 27 years of monthly essays in *Natural History*; *Wonderful Life*, *The Mismeasure of Man* | Essay-form research; building the argument across the piece rather than fronting it as an abstract; punctuated-equilibrium-style synthesis |

### Immersive investigation

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `mary-roach-researcher` | *Stiff*, *Bonk*, *Packing for Mars*, *Gulp* | Immersive popular science; the field trip / lab visit / curious-but-irreverent investigation |

### Clinical / case-based

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `oliver-sacks-researcher` | Neurologist; *Awakenings*, *The Man Who Mistook His Wife for a Hat*, *Musicophilia* | Clinical case as humanist literature; the patient-as-person, not as chart |
| `atul-gawande-researcher` | Surgeon, public-health researcher; *Being Mortal*, *The Checklist Manifesto*, *Complications* | Medical research as systems thinking + ethics; checklists as design |

### Synthesis at scale

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `jared-diamond-researcher` | UCLA geographer/biologist; *Guns, Germs, and Steel*, *Collapse* | Multi-disciplinary synthesis at civilizational scale; geography-and-biology as historical engine |
| `edward-o-wilson-researcher` | Harvard biologist; *Sociobiology*, *Consilience*, *On Human Nature* | Biology-anchored synthesis; the unity-of-knowledge proposal |

### Deep-research narrative

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `rebecca-skloot-researcher` | *The Immortal Life of Henrietta Lacks* (10-year investigation) | Deep narrative research; recovering a human story from the medical/scientific archive; the patient long investigation |
| `robert-caro-researcher` | *The Power Broker*, the LBJ biographies | Investigative biography over decades; "turn every page" |

### Cross-dispatchable from sibling plugins

| Persona | Where | Dispatch when |
|---|---|---|
| `don-knuth-engineer` | `great-engineers` | Technical-mathematical writing rigor; TAOCP-style literate programming; formal proof presentation |
| `hannah-arendt-counsel` | `great-counsels` | Political philosophy research register; *Origins of Totalitarianism* / *Eichmann in Jerusalem* mode |

## 3. The four MVP skills

### `/researchers-channel <persona>`

Loads a researcher persona into the current conversation. Substantive output (studies, reviews, syntheses) auto-saves.

```
/researchers-channel sagan
/researchers-channel gould
/researchers-channel caro
```

Output paths by artifact type:

| Artifact type | Path |
|---|---|
| Study (essay / paper / lit review / case study / investigation) | `research/studies/<slug>.md` |
| Peer review of a study | `research/reviews/<slug>.md` |
| Annotated bibliography entry | `research/bibliography/<slug>.md` |
| Persona-specific alternative | `research/studies/<slug>-<persona-suffix>.md` |

### `/researchers-project-init`

Scaffolds a `research/` directory at the project root. Creates `studies/`, `reviews/`, `bibliography/`. Updates `CLAUDE.md` (or creates one) with a `## Research` section and a `Current study:` field.

### `/researchers-write-study <topic> [--persona <name>]`

Produces a study. Default persona by signal:

| Signal | Default persona |
|---|---|
| "communicate to public", "wonder", "skepticism", baloney-detection | `carl-sagan-researcher` |
| "essay form", "synthesize", "Natural History" | `stephen-jay-gould-researcher` |
| "immersive", "field trip", "go visit" | `mary-roach-researcher` |
| "case study", "clinical", "patient story" | `oliver-sacks-researcher` |
| "medical system", "checklist", "healthcare ethics" | `atul-gawande-researcher` |
| "civilizational synthesis", "why some societies", "geography + biology" | `jared-diamond-researcher` |
| "biology-anchored", "consilience", "unity of knowledge" | `edward-o-wilson-researcher` |
| "deep research", "recover a person", "10-year investigation" | `rebecca-skloot-researcher` |
| "investigative biography", "turn every page", "the archive" | `robert-caro-researcher` |
| Otherwise | `stephen-jay-gould-researcher` (the essay-as-research default) |

Output: `research/studies/<slug>.md`. Format: question → method → primary sources → analysis → synthesis → caveats and limits → bibliography references.

### `/researchers-review <path> [--personas <list>]`

Dispatches persona(s) to peer-review a study. Default panel (parallel review):

- `stephen-jay-gould-researcher` — synthesis, essay-form integrity
- `carl-sagan-researcher` — skeptical inquiry, baloney detection
- `edward-o-wilson-researcher` — consilience check, biology-anchored synthesis

Override with `--personas roach,gawande` etc. Output: `research/reviews/<slug>.md`. Format: per-persona verdict + marked passages, then consolidated highest-leverage change.

## 4. Project structure

```
.great-authors/                 # the bible (when project is cross-craft)
README.md                       # the spec for research-heavy projects
CLAUDE.md                       # orchestrator-mode notes
research/                       # great-researchers writes here (this plugin)
├── studies/                    #   essays, papers, lit reviews, case studies
├── reviews/                    #   peer-style reviews
└── bibliography/               #   citations, sources, annotated bibliographies
```

The `bibliography/` subdir is the load-bearing differentiator vs. essays in `manuscript/` (great-authors). Citation discipline lives separately from the studies that reference them.

## 5. Conventions

These are encoded across the constellation. Researchers inherits all of them.

1. **Orchestrator vs. specialist.** Personas are dispatched. The orchestrator never produces the artifact in-context.
2. **Default-save behavior.** Every generative skill saves to disk before showing in chat.
3. **Bible reading.** Every persona reads the project specification before deciding (README, CLAUDE.md, prior studies and reviews, bibliography; `.great-authors/` for cross-craft).
4. **Honest claim discipline.** No studies that promise certainty the evidence cannot deliver. No reviews that soften the verdict to be liked.
5. **Cross-plugin dispatch.** When a question reaches into another craft, dispatch the right plugin's persona. The constellation composes.
6. **Not a substitute for primary research.** This plugin produces craft-level studies, not original empirical findings.

## 6. Cross-plugin orchestration

The researchers plugin sits at the end of the constellation v0.1:

| Plugin | Role |
|---|---|
| `great-minds` | Strategy + constellation entry point |
| `great-authors` | Prose, manuscripts |
| `great-filmmakers` | Film, motion |
| `great-publishers` | Publication form |
| `great-marketers` | Marketing |
| `great-engineers` | Software-engineering craft (`don-knuth-engineer` cross-dispatched for technical rigor) |
| `great-designers` | Product, UX, visual-design craft |
| `great-operators` | Operations, management, execution craft |
| `great-counsels` | Legal, policy, ethics craft (`hannah-arendt-counsel` cross-dispatched for political-philosophy research register) |
| `great-researchers` | Academic / scientific register (this plugin) |

Dispatch syntax: `Agent({subagent_type: "<plugin>:<persona>-<suffix>", ...})`. **Always grep `<plugin>-plugin/agents/` for the actual file before writing dispatch examples** (lesson from great-operators issue #1).

The v0.1 personas of this plugin were drafted via cross-plugin orchestration — each researcher drafted by a great-authors writer whose register fits the subject. Sixth and final v0.1 production use of the constellation pattern.

## 7. What's deferred to v1.0

- `/researchers-write-bibliography <topic>` — annotated bibliography
- `/researchers-debate <topic> <a> <b>` — Gould vs. Wilson on sociobiology is the canonical example
- `/researchers-critique <path>` — fast 3-bullet verdict
- `/researchers-fact-check <claim>` — single-claim citation verification (still NOT a substitute for real verification)
- `/researchers-edit <file>` — multi-persona marked-up review

## 8. Smoke tests

Run before tagging a release:

```bash
bash tests/smoke.sh
```

Validates: SKILL.md frontmatter, persona frontmatter, persona-count alignment, version coherence, DXT tool/handler matching, Knuth + Arendt cross-dispatch redirect presence, NOT-ACADEMIC-ADVICE disclaimer presence in critical files.

## 9. The constellation context (now complete at v0.1)

| Plugin | Domain | Status |
|---|---|---|
| [great-minds](https://github.com/sethshoultes/great-minds-plugin) | Strategy + constellation entry point | v1.4+ |
| [great-authors](https://github.com/sethshoultes/great-authors-plugin) | Prose | v1.6+ |
| [great-filmmakers](https://github.com/sethshoultes/great-filmmakers-plugin) | Film | v1.10+ |
| [great-publishers](https://github.com/sethshoultes/great-publishers-plugin) | Publication form | v0.1 |
| [great-marketers](https://github.com/sethshoultes/great-marketers-plugin) | Marketing | v0.1 |
| [great-engineers](https://github.com/sethshoultes/great-engineers-plugin) | Software-engineering craft | v0.1 |
| [great-designers](https://github.com/sethshoultes/great-designers-plugin) | Product, UX, visual-design craft | v0.1 |
| [great-operators](https://github.com/sethshoultes/great-operators-plugin) | Operations, management, execution craft | v0.1 |
| [great-counsels](https://github.com/sethshoultes/great-counsels-plugin) | Legal, policy, ethics craft | v0.1 |
| **great-researchers** (this) | Academic / scientific register | **v0.1** |

The constellation v0.1 is complete. Next phase per the brain roadmap: form the corporation, serve real customers.
