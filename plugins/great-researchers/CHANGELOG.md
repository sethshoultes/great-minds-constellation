# Changelog

All notable changes to `great-researchers` are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/) with [SemVer](https://semver.org/) versioning.

## [0.1.0] — 2026-04-27 — MVP (constellation v0.1 complete)

The initial release. **Tenth and final v0.1 plugin in the Great Minds constellation.** Sibling to all nine other constellation plugins. Closes the academic/scientific register gap.

### Added

**Nine researcher personas** — `agents/`

- `carl-sagan-researcher` — astronomer; *Cosmos*, *The Demon-Haunted World*; science communication as discipline
- `stephen-jay-gould-researcher` — paleontologist; 27 years of monthly essays in *Natural History*; the essay as research
- `mary-roach-researcher` — *Stiff*, *Bonk*, *Packing for Mars*; immersive popular science
- `oliver-sacks-researcher` — *Awakenings*, *The Man Who Mistook His Wife for a Hat*; clinical case as humanist literature
- `atul-gawande-researcher` — *Being Mortal*, *The Checklist Manifesto*; medical research as systems thinking
- `jared-diamond-researcher` — *Guns, Germs, and Steel*, *Collapse*; multi-disciplinary synthesis at civilizational scale
- `edward-o-wilson-researcher` — *Sociobiology*, *Consilience*; unity-of-knowledge proposal
- `rebecca-skloot-researcher` — *The Immortal Life of Henrietta Lacks*; deep-research narrative
- `robert-caro-researcher` — *The Power Broker*, LBJ biographies; "turn every page"

**Drafted via cross-plugin orchestration.** Each persona file was drafted by a great-authors writer whose register fits the subject:

| Subject | Drafter | Lens |
|---|---|---|
| Sagan | King | Popular accessibility; science as story-craft |
| Gould | McPhee | Architecture of essays; patient observation; Natural History column as McPhee structure |
| Roach | Vonnegut | Warm curiosity + dry humor; the field trip as research method |
| Sacks | Morrison | Lyric attention to the specific person; refusal of clinical abstraction |
| Gawande | Didion | Cool clinical authority; the medical system observed |
| Diamond | Wallace | Multi-disciplinary lattice; *Guns Germs and Steel* as Wallace footnote tree at civilization scale |
| Wilson | Le Guin | Consilience as worldbuilding; the natural world as imagined ecology |
| Skloot | Baldwin | Moral urgency about who gets to be seen; Henrietta Lacks as Baldwin's actual subject in another mode |
| Caro | Hemingway | Iceberg theory in nonfiction; 1300 pages on Robert Moses with most of the research below the waterline |

Final pass: `great-authors:gottlieb-persona` did a threshold edit across the corpus.

**Cross-dispatch hints (two)** — research is broad enough that adjacent plugins carry registers this plugin doesn't replicate:

- `great-engineers:don-knuth-engineer` — formal mathematical/technical writing rigor (TAOCP, literate programming)
- `great-counsels:hannah-arendt-counsel` — political philosophy research register (*Origins of Totalitarianism*, *Eichmann in Jerusalem*)

The constellation pattern: compose, don't duplicate.

**Four operational skills** — `skills/`

- `/researchers-channel <persona>` — load a persona; substantive output auto-saves to `research/<artifact-type>/<slug>.md`
- `/researchers-project-init` — scaffold `research/{studies,reviews,bibliography}/` at the project root
- `/researchers-write-study <topic>` — produces a study (essay / paper / literature review / case study); persona-driven register
- `/researchers-review <path>` — dispatches persona(s) to peer-review an existing study; default panel Gould + Sagan + Wilson

**Distribution**

- Claude Code plugin (this repo)
- Claude Desktop DXT bundle at `distribution/dxt/`

### Architecture decisions

- **Two cross-dispatch hints, not one.** Earlier plugins had a single canonical cross-dispatch (Margaret to engineers, Ive to designers, Buffett to operators, Aurelius to counsels). Researchers needs two because the academic register lives in two adjacent plugins (engineers for technical rigor; counsels for political philosophy). Both are documented in README and channel skill.
- **Bibliography is a first-class subdir.** Citation discipline is the load-bearing differentiator for research vs. essay/policy/spec work. `research/bibliography/` is where citations live, separately from the studies that reference them.
- **Cross-plugin orchestration as build pattern, sixth and final v0.1 production use.** After marketers, engineers, designers, operators, counsels. The pattern works; it's now the default for any persona-set v0.1.
- **Constellation v0.1 is now complete.** Ten plugins shipped: minds, authors, filmmakers, publishers, marketers, engineers, designers, operators, counsels, researchers. Next phase per the brain roadmap: form the corporation, serve real customers.

### Deferred to v1.0

- `/researchers-write-bibliography <topic>` — annotated bibliography
- `/researchers-debate <topic> <a> <b>` — 2-round craft debate (Gould vs. Wilson on sociobiology is the canonical example)
- `/researchers-critique <path>` — fast 3-bullet verdict from N personas
- `/researchers-fact-check <claim>` — single-claim citation verification (still NOT a substitute for real verification)
- `/researchers-edit <file>` — multi-persona marked-up review
