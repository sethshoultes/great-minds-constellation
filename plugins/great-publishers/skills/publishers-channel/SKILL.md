---
name: publishers-channel
description: Load a named publisher persona (Chip Kidd, Tina Brown, Maxwell Perkins, Jann Wenner, Bob Silvers, Diana Vreeland, Bennett Cerf, George Lois) into the current conversation for direct collaboration. Substantive output (cover briefs, jacket copy, positioning docs, threshold reads, list strategies, visual briefs) auto-saves to publishers/<artifact-type>/<slug>.md by default. Use when the user wants to work *with* a specific publisher persona on the publication form — e.g., "channel Chip on the cover," "let me work with Tina on the jacket copy," "put George Lois at the keyboard for this Esquire-style cover."
---

# /publishers-channel <persona>

Load a named publisher persona into the current conversation.

## What this does

Reads the matching `agents/<persona>.md` file from this plugin's install directory, strips the frontmatter, and system-prompts the persona body into the main conversation. You then collaborate directly with the persona — they're in the session with you, not a subagent that reports back.

## When to use

- Working a cover concept and you want Chip Kidd or George Lois in the room.
- Drafting jacket copy collaboratively with Tina Brown.
- Doing the threshold read on a manuscript with Maxwell Perkins.
- Planning a multi-issue rollout with Jann Wenner.
- Editing a long-form essay with Bob Silvers.
- Specifying the visual rhythm of a launch with Diana Vreeland.
- Sketching the multi-title list strategy with Bennett Cerf.

Not for: parallel multi-persona critique (filed for v1.0 as `/publishers-critique`); two-persona debate (filed for v1.0 as `/publishers-debate`).

## Instructions for Claude

When this skill is invoked with a persona name:

1. **Resolve the persona name** to an agent file. Accept common short forms:
   - `chip`, `kidd`, `chip-kidd` → `chip-kidd-designer.md`
   - `tina`, `brown`, `tina-brown` → `tina-brown-editor.md`
   - `perkins`, `max`, `maxwell-perkins` → `maxwell-perkins-editor.md`
   - `jann`, `wenner`, `jann-wenner` → `jann-wenner-publisher.md`
   - `silvers`, `bob`, `bob-silvers` → `bob-silvers-editor.md`
   - `diana`, `vreeland`, `dv` → `diana-vreeland-editor.md`
   - `bennett`, `cerf`, `bennett-cerf` → `bennett-cerf-publisher.md`
   - `george`, `lois`, `george-lois` → `george-lois-designer.md`

   If the name doesn't match, list the eight valid names and ask which one they meant.

2. **Read the agent file** at `<plugin-install-path>/agents/<name>.md`. Resolve the install path by walking up from this SKILL.md's own file path (`../../agents/`).

3. **Strip the YAML frontmatter** — everything between the first `---` and the matching `---` at the start of the file. Keep the rest.

4. **Announce the persona takeover** to the user in one line:
   `"Channeling <Display Name>. Say 'drop the persona' to exit."`

5. **Adopt the persona for the remainder of the conversation.** Every subsequent response is written as the persona. Apply their voice, their principles, their workflow.

6. **Respect the `## Before you decide / How you ... ` protocol.** If `.great-authors/` exists in the user's current working directory, read the relevant bible files before producing any substantive artifact.

7. **Exit condition** — if the user says "drop the persona," "exit persona," "back to Claude," or similar, return to normal Claude voice and acknowledge the handoff.

## Saving substantive output to disk

**Substantive artifacts save automatically by default.** The user should never have to remember an incantation to keep their own work — the default is the right behavior; opt-out is for the rare case.

A "substantive artifact" is the most recent response that is the persona's deliverable: a cover brief, a jacket copy block, a positioning doc, a threshold read letter, a rollout plan, a list strategy, a visual brief. Not a craft conversation, not a meta-discussion, not a one-line revision.

### Auto-save behavior (default)

When the persona produces a substantive artifact, save by artifact type:

| Artifact type | Path |
|---|---|
| Cover concept brief (Chip Kidd) | `publishers/covers/<slug>.md` |
| Cover provocation brief (George Lois) | `publishers/covers/<slug>-provocation.md` |
| Visual brief / opening spread (Diana Vreeland) | `publishers/covers/<slug>-visual-brief.md` |
| Jacket copy / blurb / cover line (Tina Brown) | `publishers/jacket-copy/<slug>.md` |
| Positioning doc / pitch (Tina Brown) | `publishers/positioning/<slug>.md` |
| Threshold read letter (Maxwell Perkins) | `publishers/positioning/<slug>-threshold-read.md` |
| Editorial letter (Bob Silvers) | `publishers/positioning/<slug>-editorial-letter.md` |
| Rollout plan (Jann Wenner) | `publishers/positioning/<slug>-rollout.md` |
| List strategy (Bennett Cerf) | `publishers/positioning/<slug>-list-strategy.md` |
| Trailer concept | `publishers/trailer/<slug>.md` |

If `publishers/<subdir>/` doesn't exist, create it. If a file at that path already exists, ask whether to append, replace, or save under a new slug.

Show the artifact to the user immediately after the path confirmation. The path appears at the TOP of the response so the user knows where the work landed before they read it:

```
📝 Saved to publishers/covers/arizona-strip.md (cover concept brief, 412 words).

<artifact body here>
```

### Slug resolution

Resolve `<slug>` from `.great-authors/project.md`'s `## Publishing` section if it has a `Current artifact:` field. Fall back to project slug. Fall back to asking the user once at session start: "Where should I save publishing artifacts for this session? (default: `<project-slug>`)"

### Opt-out for a single artifact

When the user says one of these BEFORE the persona produces the artifact:
- "preview only"
- "don't save this one"
- "draft, don't commit"
- "just show me"

…produce the artifact in chat without saving. Note this in one line: `(Preview only — not saved.)`

### Opt-out persistently for the session

If the user says "channel mode: chat-only" or similar, honor it for the rest of the session and require explicit save triggers (`save that`, `commit`, etc.) to write to disk.

### Save triggers (still respected)

Even with auto-save default, the user may explicitly trigger a save of a prior artifact:

- "save that"
- "commit"
- "save to publishers"

These work the same way — identify the most recent substantive artifact, save to the appropriate path, confirm.

### Never auto-save

- Meta-discussion, craft commentary, questions to the user
- One-line revisions or short clarifications
- Lists or outlines that are mid-process, not deliverables
- Anything the user has explicitly opted out of in this session

If unsure, default toward saving (it's easier to delete a saved file than to recover work lost to context).

## Notes

- This skill is a one-way load. To switch personas mid-session, the user drops the current persona and invokes `/publishers-channel` again with a different name.
- If the user asks a question genuinely outside the persona's domain (e.g., Chip Kidd asked about software architecture), answer in the persona's voice but acknowledge the boundary honestly. See each persona's `## Staying in character` footer.
- Never reproduce a persona's actual published work or attribute fabricated quotes to them. Every persona's identity opening is grounded in their real career; specific covers, manuscripts, or quotes should only be referenced when verifiable.
- Auto-save is the default. The plugin previously required users to remember a trigger phrase; this was changed to match the trilogy convention. See great-authors `learnings/generative-skills-must-persist-by-default.md` in the brain vault for the underlying principle.
