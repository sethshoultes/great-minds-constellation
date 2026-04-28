# Great Counsels

Nine counsel personas (Ruth Bader Ginsburg, Thurgood Marshall, Antonin Scalia, Lawrence Lessig, Tim Wu, Louis Brandeis, Cass Sunstein, Hannah Arendt, John Rawls) and four operational skills for legal memos, policy memos, ethics reviews, and project initialization. A Claude Code plugin. Ninth in the Great Minds constellation:

- [`great-minds-plugin`](https://github.com/sethshoultes/great-minds-plugin) — strategic decision-makers
- [`great-authors-plugin`](https://github.com/sethshoultes/great-authors-plugin) — prose craft
- [`great-filmmakers-plugin`](https://github.com/sethshoultes/great-filmmakers-plugin) — film craft
- [`great-publishers-plugin`](https://github.com/sethshoultes/great-publishers-plugin) — publication form
- [`great-marketers-plugin`](https://github.com/sethshoultes/great-marketers-plugin) — marketing
- [`great-engineers-plugin`](https://github.com/sethshoultes/great-engineers-plugin) — software-engineering craft
- [`great-designers-plugin`](https://github.com/sethshoultes/great-designers-plugin) — product, UX, visual-design craft
- [`great-operators-plugin`](https://github.com/sethshoultes/great-operators-plugin) — operations, management, execution craft
- **`great-counsels-plugin`** (this repo) — legal, policy, ethics craft

> **New to the Great Minds constellation?** Start with [`/constellation-start`](https://github.com/sethshoultes/great-minds-plugin) in `great-minds` — it asks 2-3 questions about your project shape and routes to the right plugin.

> ⚠️ **NOT LEGAL ADVICE.** This plugin produces craft-level memos, reviews, and policy analysis in the voice of canonical legal/policy/ethics figures. It is a writing and reasoning tool, not a substitute for licensed counsel. Any decision with real legal stakes requires a real attorney admitted to practice in the relevant jurisdiction. The personas are channels for craft register; they are not your lawyer.

## Install

**Claude Code:**
```
/plugin marketplace add sethshoultes/great-counsels-plugin
/plugin install great-counsels@sethshoultes
```

**Claude Desktop** (DXT bundle):
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## What's in v0.1

### 9 Personas — at the counsel threshold

| Persona | Strength |
|---|---|
| `ruth-bader-ginsburg-counsel` | Supreme Court Justice (1993-2020), ACLU Women's Rights Project. Constitutional civil rights; the long-game strategy of incremental precedent; the dissent as instruction to a future court. |
| `thurgood-marshall-counsel` | NAACP LDF chief, Solicitor General, Supreme Court Justice (1967-1991). The litigation strategy that ended Plessy. Brown v. Board. The long game played from Howard Law to Marshall's chambers. |
| `antonin-scalia-counsel` | Supreme Court Justice (1986-2016). Originalism, textualism. The Constitution as a written document with a fixed meaning, and the dissent that mocks the majority for pretending otherwise. |
| `lawrence-lessig-counsel` | Harvard Law professor; *Code and Other Laws of Cyberspace*; Creative Commons co-founder. Code is law; the four modalities of regulation; cyberspace as a regulable place. |
| `tim-wu-counsel` | Columbia Law; *The Master Switch*, *The Curse of Bigness*. Net neutrality (he coined the term). Antitrust as a constitutional question; platform power as the central issue of the age. |
| `louis-brandeis-counsel` | Supreme Court Justice (1916-1939); *Other People's Money*. The right to be let alone (the original privacy doctrine). Bigness as a curse. The brief as a fact-laden argument. |
| `cass-sunstein-counsel` | Harvard Law; OIRA administrator under Obama; *Nudge* (with Thaler). Administrative law; behavioral economics applied to regulation; choice architecture as policy design. |
| `hannah-arendt-counsel` | Political theorist; *The Origins of Totalitarianism*, *Eichmann in Jerusalem*. The banality of evil. Action vs. behavior. The public realm as the precondition of politics. |
| `john-rawls-counsel` | *A Theory of Justice*. The veil of ignorance. Justice as fairness. The original position. The framework that organized the second half of 20th-century political philosophy. |

### How the personas were drafted

The v0.1 persona files were drafted via cross-plugin orchestration — each counsel written by a great-authors persona whose register fits the subject (Didion on RBG, Baldwin on Marshall, McCarthy on Scalia, Le Guin on Lessig, Vonnegut on Wu, Hemingway on Brandeis, Wallace on Sunstein, Morrison on Arendt, McPhee on Rawls). Then `great-authors:gottlieb-persona` did a threshold pass and named cuts. Fifth production use of the constellation pattern after great-marketers, great-engineers, great-designers, and great-operators v0.1.

### See also: `great-minds:marcus-aurelius-mod`

Marcus Aurelius — Stoic emperor and the *Meditations* — lives in [`great-minds`](https://github.com/sethshoultes/great-minds-plugin) as the moderator/orchestrator persona, used for conflict mediation, neutral facilitation, and the calm-under-pressure register. He's cross-dispatchable for any project where the ethical question is also an interpersonal/orchestration one:

```
Agent({
  subagent_type: "great-minds:marcus-aurelius-mod",
  prompt: "<self-contained Stoic-mediation brief>"
})
```

great-counsels handles legal/policy/ethics craft at the working level (memos, reviews, briefs); great-minds handles Stoic executive mediation when the question is "how do we sit calmly with this hard call."

### 4 MVP Skills

| Skill | What it does |
|---|---|
| `/counsels-channel <persona>` | Load a counsel persona into your current conversation. Substantive output (memos, reviews, briefs) auto-saves to `counsel/<artifact-type>/<slug>.md`. |
| `/counsels-project-init` | Scaffold a `counsel/` directory at the project root, sibling to `manuscript/`, `film/`, `publishers/`, `marketing/`, `engineering/`, `design/`, `operations/`. Subdirs: `memos/`, `reviews/`, `briefs/`. |
| `/counsels-write-memo <question>` | Produces a legal / policy / ethics memo, persona-driven register. Default RBG for civil rights, Marshall for litigation strategy, Scalia for textualist analysis, Lessig for digital law, Wu for antitrust/platform, Brandeis for privacy, Sunstein for regulatory, Arendt for political philosophy, Rawls for ethical reasoning. Override available. Output: `counsel/memos/<slug>.md`. |
| `/counsels-review <path>` | Dispatches persona(s) to review a decision, policy, practice, or draft memo. Default panel for parallel review: RBG (constitutional/civil-rights lens), Lessig (regulability/digital-context lens), Rawls (justice-as-fairness lens). Override available. Output: `counsel/reviews/<slug>.md`. |

## Why this plugin

The constellation could create artifacts (prose, film, publication, marketing copy), build software (engineers), design product surfaces (designers), and operate the business (operators) — but couldn't yet ask **whether the work was right to do**. The "can we?" question is downstream of the "should we?" question, and the constellation needed a craft register for the latter. Legal, policy, and ethics are different lenses on the same fundamental discipline: deciding whether an action is permissible, justifiable, and worthy.

great-counsels fills that gap. RBG and Marshall cover constitutional civil rights from progressive and litigation-strategy angles. Scalia covers originalism as the load-bearing counter-lens. Lessig and Wu cover the digital and platform questions. Brandeis covers privacy and antitrust at their American origin. Sunstein covers administrative law and behavioral regulation. Arendt and Rawls cover political philosophy and ethical reasoning. Different lenses, different vocabularies, different verdicts.

## ⚠️ Limits and disclaimers

- **Not legal advice.** This plugin produces craft-level memos in the voice of canonical figures. Any decision with real legal stakes requires a real attorney admitted to the relevant jurisdiction. Treat persona output as a writing tool and a reasoning lens, not as a representation by counsel.
- **Not a court filing.** Briefs produced by this plugin are persona-voice writing exercises. Filing them in a court is not a use case the plugin supports.
- **Not therapy or moral authority.** Arendt and Rawls personas can reason through ethical frameworks; they cannot tell you what the right thing to do is for your specific situation. The hard call is yours.
- **Quotes are not authoritative.** Personas reference their subjects' published works. Do not treat any quoted line as a verifiable citation without checking the source.

## Conventions inherited from the constellation

- **Orchestrator vs. specialist.** Personas are dispatched as sub-agents in clean contexts. The orchestrator never produces the artifact in-context.
- **Default-save.** Every generative skill saves output to disk before showing it in chat. Save to `counsel/<subdir>/<slug>.md`. Document the path in the response.
- **Bible reading.** Every persona reads the project's specification before deciding — `README.md`, `CLAUDE.md`, the question or decision under review, prior memos and reviews. For cross-craft projects, the persona also reads `.great-authors/project.md` if it exists.
- **Cross-plugin orchestration.** When a counsel question reaches into another craft (the engineering reality of a privacy claim, the design constraint behind a consent flow, the operations cost of a compliance regime), the orchestrator dispatches the right plugin's persona. The constellation composes; this plugin doesn't replicate.

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
counsel/                        # great-counsels writes here (this plugin)
├── memos/                      #   legal memos, policy memos, ethics memos
├── reviews/                    #   reviews of decisions, policies, practices
└── briefs/                     #   formal position papers, briefs
```

## Roadmap

- **v0.1** (this release) — nine personas, four MVP skills, DXT bundle.
- **v1.0** — Add `/counsels-debate <topic> <a> <b>` (mirror `/authors-debate` — RBG vs. Scalia would be the canonical example), `/counsels-critique <path>` (fast 3-bullet verdict from N personas), `/counsels-write-policy <topic>` (full policy document), `/counsels-edit <file>` (multi-persona marked-up review), `/counsels-veil-of-ignorance <decision>` (Rawls-mode ethical analysis). ~12 skills total. Matches the breadth of the trilogy.

## License

MIT. See [LICENSE](./LICENSE).
