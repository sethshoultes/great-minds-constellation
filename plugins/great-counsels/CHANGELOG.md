# Changelog

All notable changes to `great-counsels` are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/) with [SemVer](https://semver.org/) versioning.

## [0.1.0] — 2026-04-27 — MVP

The initial release. Ninth plugin in the Great Minds constellation. Sibling to `great-minds`, `great-authors`, `great-filmmakers`, `great-publishers`, `great-marketers`, `great-engineers`, `great-designers`, `great-operators`. Closes the constellation's "should we?" gap — the legal, policy, and ethics craft register distinct from the "can we?" execution plugins.

### Added

**Nine counsel personas** — `agents/`

- `ruth-bader-ginsburg-counsel` — Supreme Court Justice (1993-2020); constitutional civil rights, the long-game precedent strategy, the dissent as instruction
- `thurgood-marshall-counsel` — NAACP LDF chief, Solicitor General, Justice (1967-1991); litigation strategy that ended Plessy; Brown v. Board
- `antonin-scalia-counsel` — Justice (1986-2016); originalism, textualism, the dissent that mocks
- `lawrence-lessig-counsel` — Harvard Law; *Code and Other Laws of Cyberspace*; Creative Commons; code is law
- `tim-wu-counsel` — Columbia Law; *The Master Switch*, *The Curse of Bigness*; coined "net neutrality"
- `louis-brandeis-counsel` — Justice (1916-1939); *Other People's Money*; "the right to be let alone"
- `cass-sunstein-counsel` — Harvard Law, OIRA; *Nudge* (with Thaler); choice architecture
- `hannah-arendt-counsel` — Political theorist; *The Origins of Totalitarianism*, *Eichmann in Jerusalem*; banality of evil
- `john-rawls-counsel` — *A Theory of Justice*; veil of ignorance; justice as fairness

**Drafted via cross-plugin orchestration.** Each persona file was drafted by a great-authors writer whose register fits the subject:

| Subject | Drafter | Lens |
|---|---|---|
| RBG | Didion | Cool observational authority; the dissent as Didion's quiet sentence |
| Marshall | Baldwin | Moral urgency about civil rights; Baldwin's actual subject applied to litigation |
| Scalia | McCarthy | Brutal clarity, no consolation; originalism in McCarthy's cadence |
| Lessig | Le Guin | Worldbuilding the legal architecture of cyberspace |
| Wu | Vonnegut | Plainspoken, warm-direct about big tech; "so it goes" but in antitrust |
| Brandeis | Hemingway | Iceberg; the Brandeis brief as compressed surface, the facts below |
| Sunstein | Wallace | Footnoted regulatory lattice; *Nudge* IS Wallace footnote tree |
| Arendt | Morrison | The cultural object as load-bearing; the banality of evil as Morrison's moral attention |
| Rawls | McPhee | Patient architectural argument; *Theory of Justice* as McPhee structure |

Final pass: `great-authors:gottlieb-persona` did a threshold edit across the corpus. Cuts applied surgically. All nine personas land in the 70-110 line target range.

**Marcus Aurelius stays in great-minds** as `marcus-aurelius-mod` and is cross-dispatchable. great-counsels handles legal/policy/ethics craft at the working level (memos, reviews, briefs); great-minds handles Stoic executive mediation when the ethical question is also an interpersonal/orchestration one.

**Four operational skills** — `skills/`

- `/counsels-channel <persona>` — load a persona into the conversation; substantive output auto-saves to `counsel/<artifact-type>/<slug>.md`
- `/counsels-project-init` — scaffold `counsel/` at the project root (sibling to `manuscript/`, `film/`, `publishers/`, `marketing/`, `engineering/`, `design/`, `operations/`)
- `/counsels-write-memo <question>` — produces a legal/policy/ethics memo; persona-driven register
- `/counsels-review <path>` — dispatches persona(s) to review a decision, policy, practice, or draft memo; default panel RBG + Lessig + Rawls

**Distribution**

- Claude Code plugin (this repo)
- Claude Desktop DXT bundle at `distribution/dxt/`

### Architecture decisions

- **One Aurelius.** Marcus Aurelius stays in great-minds as the Stoic-moderator persona. great-counsels references him cross-dispatchably rather than duplicating. Same principle that kept Margaret Hamilton in great-minds for engineers, Jony Ive for designers, Warren Buffett for operators.
- **Bible reading is counsel-aware.** Counsel personas read README, CLAUDE.md, the decision under review, prior memos at `counsel/memos/`, prior reviews at `counsel/reviews/`. For cross-craft projects, they also read `.great-authors/project.md`.
- **Cross-plugin orchestration as build pattern.** Fifth production use after great-marketers, great-engineers, great-designers, and great-operators v0.1. Nine great-authors writers drafted nine counsel personas in parallel; Gottlieb edited; cuts applied. The constellation pattern is the build pattern.
- **Not legal advice — by design.** The plugin documents this disclaimer prominently in the README, in MANUAL.md, and in the channel skill's auto-save behavior. Personas are craft channels, not licensed counsel.

### Deferred to v1.0

- `/counsels-debate <topic> <a> <b>` — 2-round craft debate (RBG vs. Scalia is the canonical example)
- `/counsels-critique <path>` — fast 3-bullet verdict from N personas
- `/counsels-write-policy <topic>` — full policy document (longer than a memo)
- `/counsels-edit <file>` — multi-persona marked-up review
- `/counsels-veil-of-ignorance <decision>` — Rawls-mode ethical analysis as its own skill

Total at v1.0: ~12 skills. Matches the trilogy's breadth.
