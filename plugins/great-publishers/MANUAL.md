# Great Publishers — User Manual

Complete reference for the `great-publishers` Claude Code plugin. For the executive summary, see [README.md](./README.md). For orchestration patterns, see [ORCHESTRATING.md](./ORCHESTRATING.md).

## 1. Install

```
/plugin marketplace add sethshoultes/great-minds-constellation
/plugin install great-publishers@great-minds-constellation
```

For Claude Desktop, build the DXT bundle:
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```

## 2. The eight personas

Personas are dispatched as sub-agents in clean contexts. The orchestrator does not channel them inline — see [ORCHESTRATING.md](./ORCHESTRATING.md). Each persona file at `agents/<slug>.md` carries its own identity, voice, principles, before-decision protocol (read the bible), and what it never does.

### Books register

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `chip-kidd-designer` | Knopf book-cover designer | Cover and jacket art; visual identity at the publication threshold |
| `maxwell-perkins-editor` | Scribner editor (Hemingway, Fitzgerald, Wolfe) | Final-pass developmental shaping before the manuscript ships — distinct from manuscript-stage editing (Gottlieb in great-authors) |
| `bennett-cerf-publisher` | Random House founder | Multi-title strategy; building a publishing list and brand identity across releases |

### Magazine / long-form register

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `tina-brown-editor` | Vanity Fair / New Yorker editor | Positioning, audience, jacket copy; "who is this for" |
| `bob-silvers-editor` | NYRB editor | Long-form essays, scholarly nonfiction, intellectual register |
| `jann-wenner-publisher` | Rolling Stone founder | Long-form serialized publication; multi-issue rollouts; cultural-moment timing |

### Visual editorial register

| Persona | Real-world identity | Dispatch when |
|---|---|---|
| `diana-vreeland-editor` | Vogue / Harper's Bazaar | Visual-led publications; high-concept covers; image-first stories |
| `george-lois-designer` | Esquire cover designer | Cover concepts that need to provoke; satirical or culturally sharp covers |

## 3. The four MVP skills

### `/publishers-channel <persona>`

Loads a publisher persona into the current conversation for direct collaboration. Substantive output (cover briefs, positioning docs, jacket copy) auto-saves to `publishers/<artifact-type>/<slug>.md`.

```
/publishers-channel chip-kidd
```

Output paths by artifact type:

| Artifact type | Path |
|---|---|
| Cover brief | `publishers/covers/<slug>.md` |
| Jacket copy / blurb | `publishers/jacket-copy/<slug>.md` |
| Positioning doc | `publishers/positioning/<slug>.md` |
| Trailer concept | `publishers/trailer/<slug>.md` |

Save triggers (explicit) and opt-out flags work the same as `/authors-channel`. See `skills/publishers-channel/SKILL.md`.

### `/publishers-project-init`

Scaffolds a `publishers/` directory at the project root. Reads `.great-authors/project.md` to import title and genre. Asks for the current artifact slug (defaults to project slug).

Created tree:

```
publishers/
├── covers/
├── jacket-copy/
├── positioning/
├── trailer/
├── blog-posts/
└── social-copy/
```

Updates `.great-authors/project.md` with a `## Publishing` section noting the current artifact and pointing publishing-stage commands at it.

### `/publishers-build-book-site <project>`

Generates an Astro book-site scaffold from `manuscript/` chapters. v0.1 documents the contract; the working Astro template lands in `templates/astro-book-site/` next pass.

```
/publishers-build-book-site arizona-strip
```

Inputs:
- `manuscript/*.md` chapters
- `.great-authors/project.md` for title, genre, voice
- `film/render/kling/keyframes/*.png` for chapter illustrations (optional)
- `film/render/<engine>/*.mp4` for embedded clips (optional)

Output:
- A sibling repo at `<project>-book/` with the scaffolded Astro project. MDX-based, IntersectionObserver fade-in `<Illustration>` components, dark/light mode, scroll-based reading, GitHub Pages config.

### `/publishers-build-trailer <project>`

Composes great-filmmakers' render pipeline into a single trailer skill.

```
/publishers-build-trailer arizona-strip
```

Steps:
1. Verify `film/screenplay/<slug>.veo3.md` (or `.kling.md`) exists.
2. Pre-flight: read `.great-authors/project.md` for genre. If mystery / thriller / crime, surface Veo's content-policy refusals before render time.
3. Render keyframes (`scripts/render_keyframes.py`).
4. Render video shots (`scripts/render_kling.py` or `scripts/render_veo.py` per the doc).
5. Assemble with ffmpeg.
6. Output `film/render/<slug>-trailer.mp4`.

The skill does not invent the engine choice — it reads the production doc that's already there. If you need a different engine, run `/filmmakers-crew --backend <name>` first.

## 4. Project structure

```
.great-authors/                 # the bible — shared with great-authors
├── project.md                  # title, genre, premise, voice rules
├── voice.md                    # voice rules in detail
└── ...                         # see great-authors MANUAL

manuscript/                     # great-authors writes here
film/                           # great-filmmakers writes here
├── screenplay/                 #   .heygen.md, .veo3.md, .kling.md
├── shot-lists/
├── score-notes/
├── storyboards/
├── edit-notes/
└── render/                     #   PNGs, MP4s
    ├── kling/
    └── veo/
publishers/                     # great-publishers writes here (this plugin)
├── covers/
├── jacket-copy/
├── positioning/
├── trailer/
├── blog-posts/
└── social-copy/
```

## 5. Conventions

These are encoded in the constellation and the publishers plugin must inherit them.

1. **Orchestrator vs. specialist.** Personas are dispatched. The orchestrator never produces the artifact in-context.
2. **Default-save behavior.** Every generative skill saves to disk before showing in chat.
3. **Project-bible reading.** Every persona reads `.great-authors/` before deciding.
4. **Backend awareness.** Astro is the default book-site backend; Hugo and Eleventy are stubs for v1.x.
5. **Content-policy awareness.** The trailer-build skill must surface engine refusals when the genre signals it.
6. **Voice-lint discipline.** Generated copy respects `voice.md` and `voice-lints.md`.
7. **No prose drafting.** When the deliverable is prose (e.g., a blog post), dispatch back to great-authors. Publishers shapes form, not voice.

## 6. Cross-plugin orchestration

The publishers plugin sits on top of the rest of the constellation:

| Plugin | Role |
|---|---|
| `great-minds` | Strategic decisions, brand voice, positioning at the company level |
| `great-authors` | Prose, manuscript editing (Gottlieb is the orchestrator at the writing stage) |
| `great-filmmakers` | Storyboards, shot lists, render manifests, the trailer's cinematic logic |
| `great-publishers` | Publication form — the book as object, the cover, the jacket, the magazine register |

Dispatch syntax for cross-plugin work: `Agent({subagent_type: "<plugin>:<persona>-persona", ...})`. See great-authors `ORCHESTRATING.md` for the deeper composition patterns.

## 7. What's deferred to v1.0

- `/publishers-design-cover` — Chip Kidd dispatch with optional gpt-image-1 integration
- `/publishers-build-blog-post` — chapter → serial blog post extraction
- `/publishers-publish-github-pages` — Astro book-site → GitHub Pages deploy
- `/publishers-publish-substack` — draft → Substack via API
- `/publishers-cross-promote` — social copy, jacket copy, query letter
- `/publishers-orchestrate-launch` — seven-phase autonomous ship pipeline
- `/publishers-debate`, `/publishers-critique`, `/publishers-edit` — composite editorial commands
- `/publishers-build-epub` — EPUB generation (filed for v1.1)
- Audiobook generation via ElevenLabs (filed for v1.x)

## 8. Smoke tests

Run before tagging a release:

```bash
bash tests/smoke.sh
```

Validates: SKILL.md frontmatter, persona frontmatter, persona-count alignment between `agents/` and `distribution/dxt/server/personas/`, version coherence across `package.json` / `plugin.json` / DXT manifest, DXT tool definitions matched by handlers.

## 9. The constellation context

The Great Minds constellation, in order:

| Plugin | Domain | Status |
|---|---|---|
| [great-minds](https://github.com/sethshoultes/great-minds-plugin) | Strategy | Live |
| [great-authors](https://github.com/sethshoultes/great-authors-plugin) | Prose | Live |
| [great-filmmakers](https://github.com/sethshoultes/great-filmmakers-plugin) | Film | Live |
| **great-publishers** (this) | Publication form | v0.1 |
| great-marketers | Positioning, ad copy, demand | Future |
| great-engineers | Software craft | Future |
| great-designers | Product, UX | Future |
| great-operators | Finance, ops | Future |

The roadmap lives in the brain vault at `projects/great-minds-ai-company-constellation.md`. Each plugin owns one craft. Cross-plugin dispatch composes them.
