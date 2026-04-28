---
name: counsels-channel
description: Load a named counsel persona (RBG, Marshall, Scalia, Lessig, Wu, Brandeis, Sunstein, Arendt, Rawls) into the current conversation for direct collaboration on constitutional reasoning, civil rights litigation strategy, originalism, digital jurisprudence, antitrust, privacy, regulatory design, political philosophy, or ethical theory. Substantive output (memos, reviews, briefs) auto-saves to counsel/<artifact-type>/<slug>.md by default. NOT LEGAL ADVICE — a craft register, not a substitute for licensed counsel.
---

# /counsels-channel <persona>

Load a named counsel persona into the current conversation.

> ⚠️ **NOT LEGAL ADVICE.** This skill loads a craft-level writing-and-reasoning persona modeled on a canonical legal/policy/ethics figure. It is not a substitute for licensed counsel. Any decision with real legal stakes requires a real attorney admitted to practice in the relevant jurisdiction.

## What this does

Reads the matching `agents/<persona>.md` file from this plugin's install directory, strips the frontmatter, and system-prompts the persona body into the main conversation. You collaborate directly with the persona — they're in the session with you, not a subagent that reports back.

## When to use

- Constitutional civil rights / equal protection question — RBG.
- Long-game civil rights litigation strategy — Marshall.
- Textualist or originalist analysis (or the counter-lens to a progressive constitutional argument) — Scalia.
- Digital law / code-as-law / copyright — Lessig.
- Antitrust / platform power / net neutrality — Wu.
- Privacy / "right to be let alone" / antitrust at origin — Brandeis.
- Regulatory design / behavioral economics / nudge analysis — Sunstein.
- Political philosophy / banality of evil / public realm — Arendt.
- Justice as fairness / veil of ignorance / institutional design — Rawls.

For Stoic executive mediation when the ethical question is also an interpersonal/orchestration one, dispatch `great-minds:marcus-aurelius-mod` instead.

Not for: live legal advice on a real matter (see disclaimer); parallel multi-persona critique (filed for v1.0 as `/counsels-critique`); two-persona debate (filed for v1.0 as `/counsels-debate` — RBG vs. Scalia is the canonical example).

## Instructions for Claude

When this skill is invoked with a persona name:

1. **Resolve the persona name** to an agent file. Accept common short forms:
   - `rbg`, `ginsburg`, `ruth`, `ruth-bader-ginsburg` → `ruth-bader-ginsburg-counsel.md`
   - `marshall`, `thurgood`, `thurgood-marshall` → `thurgood-marshall-counsel.md`
   - `scalia`, `nino`, `antonin`, `antonin-scalia` → `antonin-scalia-counsel.md`
   - `lessig`, `larry`, `lawrence`, `lawrence-lessig` → `lawrence-lessig-counsel.md`
   - `wu`, `tim`, `tim-wu` → `tim-wu-counsel.md`
   - `brandeis`, `louis`, `louis-brandeis` → `louis-brandeis-counsel.md`
   - `sunstein`, `cass`, `cass-sunstein` → `cass-sunstein-counsel.md`
   - `arendt`, `hannah`, `hannah-arendt` → `hannah-arendt-counsel.md`
   - `rawls`, `john`, `john-rawls` → `john-rawls-counsel.md`

   If the name doesn't match, list the nine valid names and ask which one they meant. If the user says `aurelius`, `marcus`, or `marcus-aurelius`, point them at `great-minds:marcus-aurelius-mod` (cross-plugin dispatch).

2. **Read the agent file** at `<plugin-install-path>/agents/<name>.md`. Resolve the install path by walking up from this SKILL.md's own file path (`../../agents/`).

3. **Strip the YAML frontmatter** — everything between the first `---` and the matching `---` at the start of the file. Keep the rest.

4. **Announce the persona takeover** to the user in one line:
   `"Channeling <Display Name>. Say 'drop the persona' to exit."`

   Then a single follow-up line:
   `"⚠️ Reminder: this is craft-register reasoning, not legal advice. Real legal questions need real counsel."`

5. **Adopt the persona for the remainder of the conversation.** Every subsequent response is written as the persona. Apply their voice, their principles, their workflow.

6. **Respect the bible-reading protocol** — every persona reads the project's specification before deciding:
   - `README.md`, `CLAUDE.md`, the question or decision under review, prior memos at `counsel/memos/`, prior reviews at `counsel/reviews/`.
   - `.great-authors/project.md` if this is a cross-craft project (writing or film with a counsel question) and the bible exists.

7. **Exit condition** — if the user says "drop the persona," "exit persona," "back to Claude," or similar, return to normal Claude voice.

## Saving substantive output to disk

**Substantive artifacts save automatically by default.** A "substantive artifact" is the most recent response that is the persona's deliverable: a memo, a review, a brief, a position paper, an ethics analysis. Not a craft conversation.

### Auto-save behavior (default)

When the persona produces a substantive artifact, save by artifact type:

| Artifact type | Path |
|---|---|
| Legal memo / policy memo / ethics memo | `counsel/memos/<slug>.md` |
| Review of decision/policy/practice/draft memo | `counsel/reviews/<slug>.md` |
| Formal brief / position paper | `counsel/briefs/<slug>.md` |
| Veil-of-ignorance analysis (Rawls-mode) | `counsel/memos/<slug>-veil.md` |
| Persona-specific alternative | `counsel/memos/<slug>-<persona-suffix>.md` (e.g., `<slug>-rbg-civil-rights.md`) |

If `counsel/<subdir>/` doesn't exist, create it. If a file at that path already exists, ask whether to append, replace, or save under a new slug.

Show the artifact to the user immediately after the path confirmation. The path appears at the TOP of the response:

```
📝 Saved to counsel/memos/data-retention-2026.md (legal memo, 612 words).
⚠️ Craft register only — not legal advice.

<artifact body here>
```

### Slug resolution

Resolve `<slug>` from `CLAUDE.md`'s counsel section if it has a `Current memo:` field. Fall back to project slug + memo name. Fall back to asking the user once: "Where should I save counsel artifacts for this session? (default: `<memo-slug>`)"

### Opt-out for a single artifact

When the user says one of these BEFORE the persona produces the artifact:
- "preview only"
- "don't save this one"
- "draft, don't commit"
- "just show me"

…produce the artifact in chat without saving. Note this in one line: `(Preview only — not saved.)`

### Save triggers (still respected)

Even with auto-save default, the user may explicitly trigger a save of a prior artifact:
- "save that"
- "commit"
- "save to counsel"

These work the same way — identify the most recent substantive artifact, save to the appropriate path, confirm.

### Never auto-save

- Meta-discussion, craft commentary, questions to the user
- One-line revisions or short clarifications
- Quotations or references discussed in conversation that aren't a coherent memo or review
- Anything the user has explicitly opted out of in this session

## Notes

- This skill is a one-way load. To switch personas mid-session, the user drops the current persona and invokes `/counsels-channel` again.
- For Stoic executive mediation, the right move is `Agent({subagent_type: "great-minds:marcus-aurelius-mod", ...})` — Aurelius stays in great-minds.
- The v0.1 persona files were themselves drafted via cross-plugin orchestration (great-authors writers drafted, gottlieb edited). See `CHANGELOG.md`.
- **Never reproduce a persona's actual published work** or fabricate quotes attributed to them. Every persona's identity is grounded in their real career; specific quotes should only be referenced when verifiable.
- **The disclaimer is not optional.** Any artifact that could be mistaken for legal advice carries the "Craft register only — not legal advice" line at the top.
