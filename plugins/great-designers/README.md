# Great Designers

Nine design personas (Don Norman, Julie Zhuo, Jared Spool, Dieter Rams, Susan Kare, Marty Cagan, Paula Scher, Tinker Hatfield, Edward Tufte) and four operational skills for design specs, design audits, and project initialization. A Claude Code plugin.

Part of the [Great Minds constellation](https://github.com/sethshoultes/great-minds-constellation) — 10 plugins for different craft domains.

> **New to the constellation?** Start with [`/constellation-start`](https://github.com/sethshoultes/great-minds-plugin) in `great-minds` — it asks 2-3 questions about your project shape and routes to the right plugin.

## Install

```
/plugin marketplace add sethshoultes/great-minds-constellation
/plugin install great-designers@great-minds-constellation
```

**Claude Desktop (DXT bundle)** — DXT distribution lives in the standalone repo, not the constellation copy:
```bash
git clone https://github.com/sethshoultes/great-designers-plugin
cd great-designers-plugin/distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## What's in v0.1

### 9 Personas — at the design threshold

| Persona | Strength |
|---|---|
| `don-norman-designer` | Author of *The Design of Everyday Things*. Cognitive ergonomics, mental models, affordances; the user is never wrong. |
| `julie-zhuo-designer` | First product designer at Facebook; *The Making of a Manager*. Design-management craft; building taste in teams; the maker who scaled. |
| `jared-spool-designer` | UIE founder; usability research as discipline. Evidence over opinion; observe before deciding; user-test the assumption. |
| `dieter-rams-designer` | Braun, Vitsoe; *Ten Principles for Good Design*. Less but better; longevity over novelty; the object that disappears into use. |
| `susan-kare-designer` | Original Mac icons; pixel-level craft. Restraint inside a tiny grid; metaphor as economy; warmth without sentimentality. |
| `marty-cagan-designer` | Silicon Valley Product Group; *Inspired*. Product discovery as discipline; outcome over output; risks tested before commitment. |
| `paula-scher-designer` | Pentagram partner; identity, posters, environmental design. Typography as voice; bold over polite; the system that performs at scale. |
| `tinker-hatfield-designer` | Air Jordan III–XV, Nike Mag. Athlete-as-user; product as story; performance and meaning in the same object. |
| `edward-tufte-designer` | *The Visual Display of Quantitative Information*. Information density without clutter; the small multiple; the chart that argues. |

### How the personas were drafted

The v0.1 persona files were drafted via cross-plugin orchestration — each designer written by a great-authors persona whose register fits the subject (McPhee on Norman, Vonnegut on Zhuo, Orwell on Spool, Hemingway on Rams, Le Guin on Kare, King on Cagan, Wallace on Scher, Morrison on Hatfield, Didion on Tufte). Then `great-authors:gottlieb-persona` did a threshold pass and named cuts. Third production use of the constellation pattern after great-marketers v0.1 and great-engineers v0.1.

### See also: `great-minds:jony-ive-designer`

Jony Ive — Apple SVP of Design, deep collaborator with Steve Jobs — lives in [`great-minds`](https://github.com/sethshoultes/great-minds-plugin) as the strategic design persona. He's cross-dispatchable for executive-level visual taste and product direction:

```
Agent({
  subagent_type: "great-minds:jony-ive-designer",
  prompt: "<self-contained design-direction brief>"
})
```

We didn't duplicate him here. One Jony. great-designers handles hands-on craft (Rams, Kare, Scher, Hatfield); great-minds handles strategic taste (Ive).

### 4 MVP Skills

| Skill | What it does |
|---|---|
| `/designers-channel <persona>` | Load a design persona into your current conversation. Substantive output (specs, audits, system docs) auto-saves to `design/<artifact-type>/<slug>.md`. Mirrors `/authors-channel` and `/engineers-channel`. |
| `/designers-project-init` | Scaffold a `design/` directory at the project root, sibling to `manuscript/`, `film/`, `publishers/`, `marketing/`, `engineering/`. Subdirs: `specs/`, `audits/`, `systems/`. |
| `/designers-write-spec <feature>` | Produces a design spec / IA doc / interaction spec, persona-driven register. Default Norman for cognitive flows; Spool for usability; Rams for visual restraint; Tufte for data UI; override available. Output: `design/specs/<slug>.md`. |
| `/designers-design-review <path>` | Dispatches persona(s) to review a UI, design system, or visual artifact. Default panel for parallel review: Norman (cognitive load), Spool (evidence), Rams (visual restraint). Override available. Output: `design/audits/<slug>.md`. |

## Why this plugin

The constellation could write the book, film the trailer, publish the artifacts, market the launch, and engineer the software — but couldn't yet **design the surface** the user actually touches. Wireframes, design systems, accessibility audits, typography choices, micro-interaction reviews: all of these need their own persona register, distinct from Ive's strategic design role in great-minds.

great-designers fills that gap with hands-on craft. Norman covers cognitive ergonomics, Zhuo covers design management, Spool covers usability research, Rams covers industrial restraint, Kare covers icon and pixel craft, Cagan covers product discovery, Scher covers typography and identity, Hatfield covers physical product narratives, Tufte covers information design. Different lenses, different vocabularies, different decisions.

## Conventions inherited from the constellation

- **Orchestrator vs. specialist.** Personas are dispatched as sub-agents in clean contexts. The orchestrator never produces the artifact in-context.
- **Default-save.** Every generative skill saves output to disk before showing it in chat. Save to `design/<subdir>/<slug>.md`. Document the path in the response.
- **Bible reading.** Every persona reads the project's specification before deciding — `README.md`, `CLAUDE.md`, the brand brief, the design system docs if any, screenshots in `design/audits/`. For cross-craft projects (writing or film with a UI surface), the persona also reads `.great-authors/project.md` if it exists.
- **Cross-plugin orchestration.** When a design question reaches into another craft (the engineering constraints behind the spec, the launch copy that the UI must accommodate, the brand voice that typography expresses), the orchestrator dispatches the right plugin's persona. The constellation composes; this plugin doesn't replicate.

## Project structure

```
.great-authors/                 # the bible — shared with great-authors (when present)
manuscript/                     # great-authors writes here (when present)
film/                           # great-filmmakers writes here (when present)
publishers/                     # great-publishers writes here (when present)
marketing/                      # great-marketers writes here (when present)
engineering/                    # great-engineers writes here (when present)
design/                         # great-designers writes here (this plugin)
├── specs/                      #   design specs, IA docs, interaction specs
├── audits/                     #   design reviews, accessibility audits, heuristic evals
└── systems/                    #   design system docs, component libraries, type scales
```

For software-only projects, `design/` lives alongside `engineering/`. For brand or print work without software, `design/` is the primary surface.

## Roadmap

- **v0.1** (this release) — nine personas, four MVP skills, DXT bundle.
- **v1.0** — Add `/designers-write-system` (design-system documentation), `/designers-debate <topic> <a> <b>` (two-round craft debate; mirror `/authors-debate`), `/designers-critique <path>` (fast 3-bullet verdict from N personas), `/designers-accessibility-audit <path>` (WCAG-aware review), `/designers-edit <file>` (multi-persona marked-up review). ~12 skills total. Matches the breadth of the constellation's mature plugins.

## License

MIT. See [LICENSE](./LICENSE).
