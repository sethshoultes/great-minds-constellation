# Orchestrating great-publishers

Notes for AI agents (or humans) running a publication project that uses this plugin's skills as sub-agents.

## The core distinction

The publisher personas in this plugin are *specialists at the publication threshold.* They bring craft for the form — the cover, the jacket, the magazine register, the book-as-object, the cultural-moment timing.

The orchestrator (you, when you are running a project) is *not a specialist.* The orchestrator coordinates: reads the manuscript, reads the film artifacts, reads the bible, briefs the right publisher persona for the question at hand, integrates the output, ships.

**The single most consequential mistake an orchestrator can make is to write the cover brief / jacket copy / positioning doc themselves.** When the orchestrator writes, it comes out generic — Chip Kidd doesn't think the same way Tina Brown does and Diana Vreeland doesn't think the same way Bob Silvers does. The fix is always to dispatch the right persona.

If you find yourself reaching for the Write tool to put a cover brief in `publishers/covers/`, stop. Ask: have I dispatched the right persona for this? If not, that's the next move.

## Who handles what

The personas split cleanly by publication form. Use this table when deciding who to dispatch.

| Question | Persona to dispatch |
|---|---|
| What does the cover *say*? Visual identity, jacket art, book-as-object. | `chip-kidd-designer` |
| Who is this for? How do we position this so it gets read instead of ignored? | `tina-brown-editor` |
| Does the manuscript hold up at the publication threshold? Final shaping before it ships. | `maxwell-perkins-editor` |
| Should we serialize? Multi-issue rollout? Time it to a cultural moment? | `jann-wenner-publisher` |
| Does the work hold the high register? Intellectual seriousness, scholarly weight. | `bob-silvers-editor` |
| The cover should provoke. Image-first. The visual is the argument. | `diana-vreeland-editor` |
| We're building a list, not just a book. What does the next title do? | `bennett-cerf-publisher` |
| The cover should make people stop on a newsstand. Headline-as-image. | `george-lois-designer` |

When two personas would honestly answer differently, that's a debate (filed for v1.0 as `/publishers-debate`).

## A typical orchestration flow

For a novel that needs to ship as a book site:

```
1. Read .great-authors/project.md — title, genre, premise, voice rules
2. Read manuscript/ — at minimum the first chapter, the table of contents, the closing chapter
3. Read film/ if it exists — the trailer storyboard, the keyframes
4. /publishers-project-init   (if publishers/ doesn't already exist)
5. /publishers-channel chip-kidd → cover concept brief saved to publishers/covers/<slug>.md
6. /publishers-channel tina-brown → positioning doc saved to publishers/positioning/<slug>.md
7. /publishers-channel maxwell-perkins → final read of the manuscript before it goes to layout
8. /publishers-build-book-site <slug>   (v0.2+ once Astro template ships)
9. Commit incrementally
```

For a magazine-style essay companion to a chapter:

```
1. Read the chapter and identify the standalone essay inside it
2. /publishers-channel tina-brown → positioning ("who is this for?")
3. /publishers-channel bob-silvers → register check ("does this hold the high seriousness?")
4. /publishers-build-blog-post <chapter>   (v1.0)
5. /publishers-publish-substack <draft>     (v1.0)
```

For a video trailer:

```
1. Confirm film/screenplay/<slug>.veo3.md (or .kling.md) exists
2. Pre-flight: check the project genre. If it's mystery / thriller / crime, plan for Veo's
   content-policy refusals on body/violence prompts. Kling is more permissive.
3. /publishers-build-trailer <slug>
4. Reviewed trailer lands at film/render/<slug>-trailer.mp4
5. Commit with a message that names the engine choice and any content-policy compromises
```

## Brief-writing as leverage

The single best investment you can make as an orchestrator is writing better briefs.

**A thin brief:**
> "Design a cover."

**A self-contained brief:**
> "Design a cover for [title], a [genre] novel of [length]. Read these files in order: `.great-authors/project.md`, `.great-authors/voice.md`, the first three chapters of the manuscript, the trailer storyboard at `film/screenplay/<slug>.veo3.md`. The cover must work at thumbnail (Amazon listing) and at full hardcover scale. Tone: [stark / warm / unsettling / etc.]. The cover should hold these architectural beats: [list]. What to avoid: [list — no body imagery, no figure with face, etc.]. Save the concept brief to `publishers/covers/<slug>.md`. Report under 300 words on the visual logic, materials, and three rejected alternatives."

The thin brief produces a generic cover. The self-contained brief produces a cover that can be argued and refined.

## When to write something yourself

Two narrow cases:

1. **Mechanical edits.** Surgical fixes — a typo in the jacket copy, a name continuity fix, an updated URL.
2. **The user explicitly asks you to.** *"Just write me one sentence of jacket copy here."* Honor that.

In all other cases: dispatch.

## Architecture as spine

`.great-authors/` is the spine. Every publisher dispatch should include the relevant bible files as part of the brief. When a cover concept drifts from the bible's voice — e.g., when the cover says "literary thriller" but `voice.md` says "elegiac mystery" — the cover concept is wrong, not the bible.

Update the bible deliberately, with the user, when the project's positioning genuinely changes. Don't let cover concepts silently overwrite the bible.

## Cross-plugin orchestration

The publishers plugin sits on top of the other three. You will routinely dispatch personas across plugin boundaries:

- `great-authors:gottlieb-persona` runs the manuscript stage. Don't duplicate.
- `great-authors:morrison-persona` may co-edit a magazine-register essay alongside `bob-silvers-editor`.
- `great-filmmakers:hitchcock-persona` may design a trailer's emotional architecture, then `chip-kidd-designer` translates that into a poster.
- `great-minds:rick-rubin-creative` may strip a launch concept down to its essence before `tina-brown-editor` positions it.

The dispatch syntax is `Agent({subagent_type: "<plugin>:<persona>-persona", ...})`. The orchestrator routes; the personas speak.

## What this plugin does NOT do (yet)

- **Marketing copy** — ad copy, demand generation, sales narrative. That's `great-marketers`, future plugin.
- **Software engineering** — code, systems, architecture craft. That's `great-engineers`, future plugin.
- **Product design** — UX, interaction patterns. That's `great-designers`, future plugin.
- **Operations** — finance, scaling, ops process. That's `great-operators`, future plugin.

When a question reaches into one of those territories, surface it explicitly: *"This is a marketing question; great-marketers doesn't exist yet, but here's what Tina Brown thinks about positioning since she's the closest publisher specialty."* Don't paper over the gap.

## Anti-patterns

These all produce generic publishing artifacts:

- Writing the cover brief / jacket copy yourself instead of dispatching
- Pattern-matching a publisher's voice in your own context (Tina Brown's wit, Chip Kidd's metaphor moves) without dispatching the actual persona
- Skipping the read of the bible and the manuscript before dispatching
- Thin briefs ("design a cover")
- Letting the trailer engine choice (Veo vs Kling vs Leonardo) get made silently — surface the trade-off
- Writing prose for any artifact whose deliverable is prose. That's `great-authors`'s job. Dispatch back to Gottlieb.

The anti-pattern that catches most orchestrators is the first one. Watch for it.
