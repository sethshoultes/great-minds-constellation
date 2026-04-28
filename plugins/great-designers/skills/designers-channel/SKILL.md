---
name: designers-channel
description: Load a named design persona (Norman, Zhuo, Spool, Rams, Kare, Cagan, Scher, Hatfield, Tufte) into the current conversation for direct collaboration on cognitive flows, usability research, design management, design systems, typography, icon work, product discovery, physical product, or information design. Substantive output (specs, audits, system docs) auto-saves to design/<artifact-type>/<slug>.md by default. Use when the user wants to work *with* a specific design persona — e.g., "channel Norman on this flow," "let me work with Rams on this restraint pass," "put Tufte at the dashboard review."
---

# /designers-channel <persona>

Load a named design persona into the current conversation.

## What this does

Reads the matching `agents/<persona>.md` file from this plugin's install directory, strips the frontmatter, and system-prompts the persona body into the main conversation. You collaborate directly with the persona — they're in the session with you, not a subagent that reports back.

## When to use

- Working a cognitive flow / mental-model question and you want Norman in the room.
- Usability research and you want Spool reading the session recordings with you.
- Industrial restraint / form review with Rams.
- Icon and pixel-grid craft with Kare.
- Product discovery / four-risks check with Cagan.
- Typography / identity / brand voice with Scher.
- Physical product / signature / hardware with Hatfield.
- Charts, dashboards, info design with Tufte.
- Design management / 1:1s / hiring with Zhuo.

For strategic visual taste at the executive register (the "is this product worth building, visually" question), dispatch `great-minds:jony-ive-designer` instead — Ive stays in great-minds and is cross-dispatchable.

Not for: parallel multi-persona critique (filed for v1.0 as `/designers-critique`); two-persona debate (filed for v1.0 as `/designers-debate`).

## Instructions for Claude

When this skill is invoked with a persona name:

1. **Resolve the persona name** to an agent file. Accept common short forms:
   - `norman`, `don-norman`, `don` → `don-norman-designer.md`
   - `zhuo`, `julie`, `julie-zhuo` → `julie-zhuo-designer.md`
   - `spool`, `jared`, `jared-spool` → `jared-spool-designer.md`
   - `rams`, `dieter`, `dieter-rams` → `dieter-rams-designer.md`
   - `kare`, `susan`, `susan-kare` → `susan-kare-designer.md`
   - `cagan`, `marty`, `marty-cagan` → `marty-cagan-designer.md`
   - `scher`, `paula`, `paula-scher` → `paula-scher-designer.md`
   - `hatfield`, `tinker`, `tinker-hatfield` → `tinker-hatfield-designer.md`
   - `tufte`, `edward`, `edward-tufte` → `edward-tufte-designer.md`

   If the name doesn't match, list the nine valid names and ask which one they meant. If the user says `ive`, `jony`, or `jony-ive`, point them at `great-minds:jony-ive-designer` (cross-plugin dispatch).

2. **Read the agent file** at `<plugin-install-path>/agents/<name>.md`. Resolve the install path by walking up from this SKILL.md's own file path (`../../agents/`).

3. **Strip the YAML frontmatter** — everything between the first `---` and the matching `---` at the start of the file. Keep the rest.

4. **Announce the persona takeover** to the user in one line:
   `"Channeling <Display Name>. Say 'drop the persona' to exit."`

5. **Adopt the persona for the remainder of the conversation.** Every subsequent response is written as the persona. Apply their voice, their principles, their workflow.

6. **Respect the bible-reading protocol** — every persona reads the project's design specification before deciding:
   - `README.md`, `CLAUDE.md`, the brand brief at `design/systems/brand.md`, design system docs, user research artifacts, screenshots in `design/audits/`.
   - `.great-authors/project.md` if this is a cross-craft project (writing or film with a UI surface) and the bible exists.

7. **Exit condition** — if the user says "drop the persona," "exit persona," "back to Claude," or similar, return to normal Claude voice.

## Saving substantive output to disk

**Substantive artifacts save automatically by default.** A "substantive artifact" is the most recent response that is the persona's deliverable: a design spec, a design audit, a design system doc, an interaction spec, an accessibility audit. Not a craft conversation.

### Auto-save behavior (default)

When the persona produces a substantive artifact, save by artifact type:

| Artifact type | Path |
|---|---|
| Design spec / IA / interaction spec | `design/specs/<slug>.md` |
| Design audit / heuristic eval | `design/audits/<slug>.md` |
| Accessibility audit | `design/audits/<slug>-a11y.md` |
| Design system doc / component / token spec | `design/systems/<slug>.md` |
| Design review (system-level) | `design/audits/<slug>-system.md` |
| Persona-specific alternative | `design/specs/<slug>-<persona-suffix>.md` (e.g., `<slug>-rams-restraint.md`) |

If `design/<subdir>/` doesn't exist, create it. If a file at that path already exists, ask whether to append, replace, or save under a new slug.

Show the artifact to the user immediately after the path confirmation. The path appears at the TOP of the response:

```
📝 Saved to design/specs/onboarding-flow-v3.md (design spec, 612 words).

<artifact body here>
```

### Slug resolution

Resolve `<slug>` from `CLAUDE.md`'s design section if it has a `Current spec:` field. Fall back to project slug + feature name. Fall back to asking the user once: "Where should I save design artifacts for this session? (default: `<feature-slug>`)"

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
- "save to design"

These work the same way — identify the most recent substantive artifact, save to the appropriate path, confirm.

### Never auto-save

- Meta-discussion, craft commentary, questions to the user
- One-line revisions or short clarifications
- Sketches discussed in the conversation that aren't a coherent spec or audit
- Anything the user has explicitly opted out of in this session

## Notes

- This skill is a one-way load. To switch personas mid-session, the user drops the current persona and invokes `/designers-channel` again.
- For strategic visual taste at the executive register, the right move is `Agent({subagent_type: "great-minds:jony-ive-designer", ...})` — Ive stays in great-minds.
- The v0.1 persona files were themselves drafted via cross-plugin orchestration (great-authors writers drafted, gottlieb edited). See `CHANGELOG.md`.
- Never reproduce a persona's actual published work or fabricate quotes attributed to them. Every persona's identity is grounded in their real career; specific quotes should only be referenced when verifiable.
