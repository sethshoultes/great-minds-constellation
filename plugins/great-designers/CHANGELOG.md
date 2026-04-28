# Changelog

All notable changes to `great-designers` are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/) with [SemVer](https://semver.org/) versioning.

## [0.1.0] — 2026-04-26 — MVP

The initial release. Seventh plugin in the Great Minds constellation. Sibling to `great-minds`, `great-authors`, `great-filmmakers`, `great-publishers`, `great-marketers`, `great-engineers`. Closes the gap between Ive's strategic-design role in great-minds and the hands-on craft of product, UX, and visual design.

### Added

**Nine design personas** — `agents/`

- `don-norman-designer` — *The Design of Everyday Things*; cognitive ergonomics, mental models, affordances
- `julie-zhuo-designer` — Facebook's first product designer; *The Making of a Manager*; design management craft
- `jared-spool-designer` — UIE founder; usability research as discipline
- `dieter-rams-designer` — Braun, Vitsoe; ten principles for good design; less but better
- `susan-kare-designer` — original Mac icons; pixel-level craft; metaphor as economy
- `marty-cagan-designer` — Silicon Valley Product Group; *Inspired*; product discovery as risk reduction
- `paula-scher-designer` — Pentagram partner; identity, posters, environmental design; typography as voice
- `tinker-hatfield-designer` — Air Jordan III–XV, Nike Mag; product as story; athlete-as-user
- `edward-tufte-designer` — *The Visual Display of Quantitative Information*; information density without clutter

**Drafted via cross-plugin orchestration.** Each persona file was drafted by a great-authors writer whose register fits the subject:

| Subject | Drafter | Lens |
|---|---|---|
| Norman | McPhee | Architecture and explanation; *The Design of Everyday Things* is in McPhee's mode — patient observation that becomes argument |
| Zhuo | Vonnegut | Warm direct prose; the maker who learned to manage and writes about it without armor |
| Spool | Orwell | Plain language and evidence-first; usability research as Orwell-clear writing about what people actually do |
| Rams | Hemingway | Iceberg theory in a kitchen radio; less is what shows, the principles are below the waterline |
| Kare | Le Guin | Worldbuilding inside a 32×32 grid; the imagined world conjured in tiny strokes |
| Cagan | King | Popular voice; risk-and-discovery as story-craft; the framework that reads like field reports |
| Scher | Wallace | Self-aware density; typographic decisions made and footnoted; the system aware of itself |
| Hatfield | Morrison | Lyric register; product as cultural object; the shoe as the story of who wears it |
| Tufte | Didion | Cool observational authority; the chart as the page, the page as the argument |

Final pass: `great-authors:gottlieb-persona` did a threshold edit across the corpus. Cuts applied surgically. All nine personas land in the 70-110 line target range.

**Jony Ive stays in great-minds** as `jony-ive-designer` and is cross-dispatchable. great-designers handles hands-on craft (Rams for industrial restraint, Scher for typography, Kare for icons, Hatfield for physical product); great-minds handles strategic visual taste (Ive). One Jony, two registers depending on the question.

**Four operational skills** — `skills/`

- `/designers-channel <persona>` — load a persona into the conversation; substantive output auto-saves to `design/<artifact-type>/<slug>.md`
- `/designers-project-init` — scaffold `design/` at the project root (sibling to `manuscript/`, `film/`, `publishers/`, `marketing/`, `engineering/`)
- `/designers-write-spec <feature>` — produces a design spec / IA doc / interaction spec; persona-driven register
- `/designers-design-review <path>` — dispatches persona(s) to review a UI, design system, or visual artifact; default panel Norman + Spool + Rams

**Distribution**

- Claude Code plugin (this repo)
- Claude Desktop DXT bundle at `distribution/dxt/`

### Architecture decisions

- **One Jony.** Jony Ive stays in great-minds as the strategic-design persona. great-designers references him cross-dispatchably rather than duplicating. Same principle that kept Margaret Hamilton in great-minds for engineers, Sara Blakely in great-minds for marketers — the constellation composes; plugins don't replicate.
- **Bible reading is design-aware.** Designer personas read README, CLAUDE.md, brand briefs, the design system docs (if any), and any existing `design/` artifacts. For cross-craft projects with a fiction bible, they also read `.great-authors/project.md`.
- **Cross-plugin orchestration as build pattern.** Third production use after great-marketers v0.1 and great-engineers v0.1. Nine great-authors writers drafted nine designer personas in parallel; Gottlieb edited; cuts applied. The constellation pattern is the build pattern.

### Deferred to v1.0

- `/designers-write-system` — design system documentation (component library, type scale, color tokens)
- `/designers-debate <topic> <a> <b>` — 2-round craft debate (mirror `/authors-debate`)
- `/designers-critique <path>` — fast 3-bullet verdict from N personas
- `/designers-accessibility-audit <path>` — WCAG-aware review
- `/designers-edit <file>` — multi-persona marked-up review

Total at v1.0: ~12 skills. Matches the trilogy's breadth.
