# Great Designers — User Manual

Complete reference for the `great-designers` Claude Code plugin. For the executive summary, see [README.md](./README.md). For orchestration patterns, see [ORCHESTRATING.md](./ORCHESTRATING.md).

## 1. Install

```
/plugin marketplace add sethshoultes/great-minds-constellation
/plugin install great-designers@great-minds-constellation
```

For Claude Desktop, build the DXT bundle:
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## 2. The nine personas

Personas are dispatched as sub-agents in clean contexts. Each persona file at `agents/<slug>.md` carries its own identity, voice, principles, before-decision protocol (read the project's design specification), and what it never does.

### Cognitive design and research

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `don-norman-designer` | *The Design of Everyday Things*, Nielsen Norman Group | Mental models, affordances, signifiers; "the user is never wrong" |
| `jared-spool-designer` | UIE founder; usability research methodologist | Usability research, user-test design, observed-vs-stated behavior; "what do they actually do" |

### Design management and discovery

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `julie-zhuo-designer` | First product designer at Facebook; *The Making of a Manager* | Building taste in teams, design management craft, the maker who learned to scale |
| `marty-cagan-designer` | Silicon Valley Product Group; *Inspired*, *Empowered* | Product discovery, risk reduction, outcome over output, the four risks of product work |

### Visual restraint and form

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `dieter-rams-designer` | Braun, Vitsoe; ten principles for good design | Industrial restraint, less but better, longevity over novelty; "good design is as little design as possible" |
| `susan-kare-designer` | Original Mac icons, NeXT, Facebook | Pixel-level craft, icon systems, metaphor as economy; "the dog should look like a dog" |

### Typography, identity, and physical product

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `paula-scher-designer` | Pentagram partner; Citi, Tiffany, MoMA, Public Theater | Typography as voice, identity systems, environmental design, type at scale |
| `tinker-hatfield-designer` | Air Jordan III–XV, Nike Mag, Air Max 1 | Physical product as story, athlete-as-user, narrative product design |

### Information design

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `edward-tufte-designer` | *The Visual Display of Quantitative Information*, *Envisioning Information* | Charts, dashboards, data UI, information density; "above all else, show the data" |

### Cross-dispatchable from `great-minds`

| Persona | Where | Dispatch when |
|---|---|---|
| `jony-ive-designer` | `great-minds` | Strategic visual taste at the executive register; product direction; "is this product worth building, visually". Use `Agent({subagent_type: "great-minds:jony-ive-designer", ...})`. |

## 3. The four MVP skills

### `/designers-channel <persona>`

Loads a designer persona into the current conversation for direct collaboration. Substantive output (specs, audits, system docs) auto-saves.

```
/designers-channel norman
/designers-channel rams
/designers-channel tufte
```

Output paths by artifact type:

| Artifact type | Path |
|---|---|
| Design spec / IA / interaction spec | `design/specs/<slug>.md` |
| Design audit / heuristic eval | `design/audits/<slug>.md` |
| Design system doc / component / token spec | `design/systems/<slug>.md` |

Save triggers (explicit) and opt-out flags work the same as `/authors-channel`. See `skills/designers-channel/SKILL.md`.

### `/designers-project-init`

Scaffolds a `design/` directory at the project root. Reads the project's specification (`README.md`, `CLAUDE.md`, brand brief, existing design system docs) to import context. Creates the subdirs `specs/`, `audits/`, `systems/`. Updates `CLAUDE.md` (or creates one if absent) to note the design directory's existence and the `Current spec:` field.

### `/designers-write-spec <feature> [--persona <name>]`

Produces a design spec / IA doc / interaction spec. Default persona by signal:

| Signal | Default persona |
|---|---|
| Cognitive flow / mental model question | `don-norman-designer` |
| Usability / observed-behavior question | `jared-spool-designer` |
| Visual restraint / industrial form | `dieter-rams-designer` |
| Icon, glyph, pixel-grid work | `susan-kare-designer` |
| Product discovery / risk question | `marty-cagan-designer` |
| Typography / identity / brand expression | `paula-scher-designer` |
| Physical product / hardware narrative | `tinker-hatfield-designer` |
| Data UI / chart / dashboard | `edward-tufte-designer` |
| Otherwise | `don-norman-designer` (the cognitive default) |

Output: `design/specs/<slug>.md`. Format: problem → user → constraints → proposal → alternatives → trade-offs → decision → open questions.

### `/designers-design-review <path> [--personas <list>]`

Dispatches persona(s) to review a UI, design system, or visual artifact.

Default panel (parallel review):
- `don-norman-designer` — cognitive load, mental models, error states
- `jared-spool-designer` — evidence-first; what would a user test reveal
- `dieter-rams-designer` — what can be removed; visual restraint

Override with `--personas tufte,scher` (etc.). Output: `design/audits/<slug>.md`. Format: per-persona verdict + marked passages, then consolidated highest-leverage change.

## 4. Project structure

```
.great-authors/                 # the bible (when project is cross-craft)
README.md                       # the spec for design-only projects
CLAUDE.md                       # orchestrator-mode notes
design/                         # great-designers writes here (this plugin)
├── specs/                      #   design specs, IA docs, interaction specs
├── audits/                     #   design reviews, accessibility audits
└── systems/                    #   design system docs, component libraries
```

For software-heavy projects, `design/` lives alongside `engineering/`. For cross-craft projects, `.great-authors/` is the shared spine across all seven plugins.

## 5. Conventions

These are encoded across the constellation. Designers inherits all of them.

1. **Orchestrator vs. specialist.** Personas are dispatched. The orchestrator never produces the artifact in-context.
2. **Default-save behavior.** Every generative skill saves to disk before showing in chat.
3. **Bible reading.** Every persona reads the project specification before deciding (README, CLAUDE.md, brand brief, design system docs; `.great-authors/` for cross-craft).
4. **Honest claim discipline.** No specs that promise what the implementation cannot deliver. No audits that soften the verdict to be liked.
5. **Cross-plugin dispatch.** When a question reaches into another craft, dispatch the right plugin's persona. The constellation composes.
6. **No implementation.** This plugin produces specs, audits, system docs — not running code. (For UI implementation, dispatch `great-engineers`.)

## 6. Cross-plugin orchestration

The designers plugin sits within the full constellation:

| Plugin | Role |
|---|---|
| `great-minds` | Strategy, board reviews, `jony-ive-designer` for strategic visual taste cross-dispatched |
| `great-authors` | Prose, copy, voice |
| `great-filmmakers` | Motion, storyboards, sequence design |
| `great-publishers` | Publication form (book covers, jacket copy, magazine register) |
| `great-marketers` | Marketing — positioning, ad copy, demand generation |
| `great-engineers` | Software-engineering craft (UI implementation) |
| `great-designers` | Product, UX, visual-design craft (this plugin) |

Dispatch syntax: `Agent({subagent_type: "<plugin>:<persona>-persona", ...})`.

The v0.1 personas of this plugin were drafted via cross-plugin orchestration — each designer drafted by a great-authors writer whose register fits the subject. The constellation pattern is the build pattern.

## 7. What's deferred to v1.0

- `/designers-write-system <name>` — design system documentation (components, tokens, type scale)
- `/designers-debate <topic> <persona-A> <persona-B>` — 2-round craft debate (mirror `/authors-debate`)
- `/designers-critique <path>` — fast 3-bullet verdict from N personas in parallel
- `/designers-accessibility-audit <path>` — WCAG-aware review
- `/designers-edit <file>` — multi-persona marked-up review

## 8. Smoke tests

Run before tagging a release:

```bash
bash tests/smoke.sh
```

Validates: SKILL.md frontmatter, persona frontmatter, persona-count alignment between `agents/` and `distribution/dxt/server/personas/`, version coherence across `package.json` / `plugin.json` / DXT manifest, DXT tool definitions matched by handlers, Ive cross-dispatch redirect presence.

## 9. The constellation context

| Plugin | Domain | Status |
|---|---|---|
| [great-minds](https://github.com/sethshoultes/great-minds-plugin) | Strategy + constellation entry point | v1.4 |
| [great-authors](https://github.com/sethshoultes/great-authors-plugin) | Prose | v1.6 |
| [great-filmmakers](https://github.com/sethshoultes/great-filmmakers-plugin) | Film | v1.10 |
| [great-publishers](https://github.com/sethshoultes/great-publishers-plugin) | Publication form | v0.1 |
| [great-marketers](https://github.com/sethshoultes/great-marketers-plugin) | Marketing | v0.1 |
| [great-engineers](https://github.com/sethshoultes/great-engineers-plugin) | Software-engineering craft | v0.1 |
| **great-designers** (this) | Product, UX, visual-design craft | **v0.1** |
| great-operators | Finance, ops | Future |

The constellation roadmap lives in the brain vault at `projects/great-minds-ai-company-constellation.md`. Each plugin owns one craft. Cross-plugin dispatch composes them.
