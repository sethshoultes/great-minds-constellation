# Great Counsels — User Manual

Complete reference for the `great-counsels` Claude Code plugin. For the executive summary, see [README.md](./README.md). For orchestration patterns, see [ORCHESTRATING.md](./ORCHESTRATING.md).

> ⚠️ **NOT LEGAL ADVICE.** This plugin produces craft-level legal/policy/ethics writing in the voice of canonical figures. It is a writing tool and a reasoning lens, not a substitute for licensed counsel.

## 1. Install

```
/plugin marketplace add sethshoultes/great-counsels-plugin
/plugin install great-counsels@sethshoultes
```

For Claude Desktop, build the DXT bundle:
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## 2. The nine personas

Personas are dispatched as sub-agents in clean contexts. Each persona file at `agents/<slug>.md` carries its own identity, voice, principles, before-decision protocol, and what it never does.

### Constitutional civil rights and litigation strategy

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `ruth-bader-ginsburg-counsel` | Supreme Court Justice (1993-2020); ACLU Women's Rights Project | Constitutional civil rights, equal protection, the dissent as instruction to a future court |
| `thurgood-marshall-counsel` | NAACP LDF chief; Solicitor General; Justice (1967-1991) | Long-game litigation strategy; civil rights as a sequence of precedents over decades |

### Originalism and textualism

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `antonin-scalia-counsel` | Justice (1986-2016) | Originalist constitutional analysis; textualist statutory analysis; the counter-lens to progressive constitutional reasoning |

### Digital and platform jurisprudence

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `lawrence-lessig-counsel` | Harvard Law; Creative Commons co-founder; *Code and Other Laws of Cyberspace* | Digital law; the four modalities of regulation (law, market, norms, code); cyberspace as a regulable place |
| `tim-wu-counsel` | Columbia Law; *The Master Switch*, *The Curse of Bigness* | Antitrust as constitutional question; platform power; net neutrality (he coined the term) |

### Privacy and antitrust at origin

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `louis-brandeis-counsel` | Supreme Court Justice (1916-1939); *Other People's Money* | Privacy ("the right to be let alone"); bigness as a curse; the Brandeis brief as fact-laden argument |

### Administrative and behavioral regulation

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `cass-sunstein-counsel` | Harvard Law; OIRA administrator (Obama); *Nudge* (with Thaler) | Administrative law; choice architecture; behavioral economics applied to regulation |

### Political philosophy and ethics

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `hannah-arendt-counsel` | Political theorist; *The Origins of Totalitarianism*, *Eichmann in Jerusalem* | Political theory; the banality of evil; action vs. behavior; the public realm |
| `john-rawls-counsel` | Harvard philosopher; *A Theory of Justice* | Justice as fairness; the veil of ignorance; the original position; ethical reasoning |

### Cross-dispatchable from `great-minds`

| Persona | Where | Dispatch when |
|---|---|---|
| `marcus-aurelius-mod` | `great-minds` | Stoic executive mediation; calm under pressure; conflict resolution. Use `Agent({subagent_type: "great-minds:marcus-aurelius-mod", ...})`. |

## 3. The four MVP skills

### `/counsels-channel <persona>`

Loads a counsel persona into the current conversation for direct collaboration. Substantive output (memos, reviews, briefs) auto-saves.

```
/counsels-channel rbg
/counsels-channel scalia
/counsels-channel rawls
```

Output paths by artifact type:

| Artifact type | Path |
|---|---|
| Legal memo / policy memo / ethics memo | `counsel/memos/<slug>.md` |
| Review of decision/policy/practice | `counsel/reviews/<slug>.md` |
| Formal brief / position paper | `counsel/briefs/<slug>.md` |
| Persona-specific alternative | `counsel/memos/<slug>-<persona-suffix>.md` (e.g., `<slug>-rbg-civil-rights.md`) |

Save triggers (explicit) and opt-out flags work the same as `/authors-channel`. See `skills/counsels-channel/SKILL.md`.

### `/counsels-project-init`

Scaffolds a `counsel/` directory at the project root. Reads the project's specification (`README.md`, `CLAUDE.md`, prior memos and reviews) to import context. Creates the subdirs `memos/`, `reviews/`, `briefs/`. Updates `CLAUDE.md` (or creates one if absent) to note the counsel directory's existence and the `Current memo:` field.

### `/counsels-write-memo <question> [--persona <name>]`

Produces a legal / policy / ethics memo. Default persona by signal:

| Signal | Default persona |
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
| Otherwise | `john-rawls-counsel` (the ethical-reasoning default) |

Output: `counsel/memos/<slug>.md`. Format: question → relevant facts → applicable framework → analysis → conclusion → caveats and limits.

### `/counsels-review <path> [--personas <list>]`

Dispatches persona(s) to review an existing decision, policy, practice, or draft memo.

Default panel (parallel review):
- `ruth-bader-ginsburg-counsel` — constitutional / civil-rights lens
- `lawrence-lessig-counsel` — regulability / digital-context lens
- `john-rawls-counsel` — justice-as-fairness lens

Override with `--personas scalia,sunstein` (etc.). Output: `counsel/reviews/<slug>.md`. Format: per-persona verdict + marked passages, then consolidated highest-leverage change.

## 4. Project structure

```
.great-authors/                 # the bible (when project is cross-craft)
README.md                       # the spec for counsel-heavy projects
CLAUDE.md                       # orchestrator-mode notes
counsel/                        # great-counsels writes here (this plugin)
├── memos/                      #   legal memos, policy memos, ethics memos
├── reviews/                    #   reviews of decisions, policies, practices
└── briefs/                     #   formal position papers, briefs
```

For cross-craft projects, `.great-authors/` is the shared spine across all nine plugins.

## 5. Conventions

These are encoded across the constellation. Counsels inherits all of them.

1. **Orchestrator vs. specialist.** Personas are dispatched. The orchestrator never produces the artifact in-context.
2. **Default-save behavior.** Every generative skill saves to disk before showing in chat.
3. **Bible reading.** Every persona reads the project specification before deciding (README, CLAUDE.md, prior memos, prior reviews; `.great-authors/` for cross-craft).
4. **Honest claim discipline.** No memos that promise certainty the framework cannot deliver. No reviews that soften the verdict to be liked.
5. **Cross-plugin dispatch.** When a question reaches into another craft, dispatch the right plugin's persona. The constellation composes.
6. **Not legal advice.** This plugin produces writing and reasoning, not representation. Hard legal questions require licensed counsel.

## 6. Cross-plugin orchestration

The counsels plugin sits within the full constellation:

| Plugin | Role |
|---|---|
| `great-minds` | Strategy, board reviews, `marcus-aurelius-mod` for Stoic mediation cross-dispatched |
| `great-authors` | Prose, copy, voice |
| `great-filmmakers` | Motion, storyboards, sequence design |
| `great-publishers` | Publication form |
| `great-marketers` | Marketing — positioning, ad copy, demand generation |
| `great-engineers` | Software-engineering craft |
| `great-designers` | Product, UX, visual-design craft |
| `great-operators` | Operations, management, execution craft |
| `great-counsels` | Legal, policy, ethics craft (this plugin) |

Dispatch syntax: `Agent({subagent_type: "<plugin>:<persona>-persona", ...})`.

The v0.1 personas of this plugin were drafted via cross-plugin orchestration — each counsel drafted by a great-authors writer whose register fits the subject. The constellation pattern is the build pattern.

## 7. What's deferred to v1.0

- `/counsels-debate <topic> <persona-A> <persona-B>` — 2-round craft debate (RBG vs. Scalia is canonical)
- `/counsels-critique <path>` — fast 3-bullet verdict from N personas in parallel
- `/counsels-write-policy <topic>` — full policy document (longer than a memo)
- `/counsels-edit <file>` — multi-persona marked-up review
- `/counsels-veil-of-ignorance <decision>` — Rawls-mode ethical analysis as its own skill

## 8. Smoke tests

Run before tagging a release:

```bash
bash tests/smoke.sh
```

Validates: SKILL.md frontmatter, persona frontmatter, persona-count alignment between `agents/` and `distribution/dxt/server/personas/`, version coherence, DXT tool definitions matched by handlers, Aurelius cross-dispatch redirect presence.

## 9. The constellation context

| Plugin | Domain | Status |
|---|---|---|
| [great-minds](https://github.com/sethshoultes/great-minds-plugin) | Strategy + constellation entry point | v1.4 |
| [great-authors](https://github.com/sethshoultes/great-authors-plugin) | Prose | v1.6 |
| [great-filmmakers](https://github.com/sethshoultes/great-filmmakers-plugin) | Film | v1.10 |
| [great-publishers](https://github.com/sethshoultes/great-publishers-plugin) | Publication form | v0.1 |
| [great-marketers](https://github.com/sethshoultes/great-marketers-plugin) | Marketing | v0.1 |
| [great-engineers](https://github.com/sethshoultes/great-engineers-plugin) | Software-engineering craft | v0.1 |
| [great-designers](https://github.com/sethshoultes/great-designers-plugin) | Product, UX, visual-design craft | v0.1 |
| [great-operators](https://github.com/sethshoultes/great-operators-plugin) | Operations, management, execution craft | v0.1 |
| **great-counsels** (this) | Legal, policy, ethics craft | **v0.1** |

The constellation roadmap lives in the brain vault at `projects/great-minds-ai-company-constellation.md`. Each plugin owns one craft. Cross-plugin dispatch composes them.
