# Changelog

All notable changes to `great-publishers` are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/) with [SemVer](https://semver.org/) versioning.

## [0.1.0] — 2026-04-26 — MVP

The initial release. Fourth plugin in the Great Minds constellation, sibling to `great-minds`, `great-authors`, `great-filmmakers`.

### Added

**Eight publisher/editor/designer personas** — agents/

- `chip-kidd-designer` — Knopf book-cover designer; visual identity at the publication threshold
- `tina-brown-editor` — Vanity Fair / New Yorker editor; positioning, audience, jacket copy
- `maxwell-perkins-editor` — Scribner editor; final-pass developmental shaping at the publication threshold
- `jann-wenner-publisher` — Rolling Stone founder; long-form serialized publication, cultural moment
- `bob-silvers-editor` — NYRB editor; intellectual seriousness, scholarly long-form
- `diana-vreeland-editor` — Vogue / Harper's Bazaar; visual-driven editorial provocation
- `bennett-cerf-publisher` — Random House founder; building a publishing list, brand strategy across releases
- `george-lois-designer` — Esquire cover designer; cover provocation, headline-as-image

**Four operational skills** — skills/

- `/publishers-channel <persona>` — load a publisher persona into the current conversation; substantive output (cover briefs, positioning docs, jacket copy) auto-saves to `publishers/<artifact-type>/<slug>.md`
- `/publishers-project-init` — scaffold a `publishers/` directory at the project root (sibling to `manuscript/`, `film/`); reads `.great-authors/project.md` to import title, genre, premise
- `/publishers-build-book-site <project>` — generates an Astro book-site scaffold from `manuscript/` chapters (template ships separately; v0.1 documents the contract and stubs the wiring)
- `/publishers-build-trailer <project>` — composes great-filmmakers' render scripts into a single trailer pipeline; surfaces engine choice and content-policy implications when the project genre signals it

**Distribution**

- Claude Code plugin (this repo)
- Claude Desktop DXT bundle at `distribution/dxt/`

### Architecture decisions

- **Constellation, not mega-plugin.** Publishers owns publication form. Marketing/ad copy, software engineering, product design, operations are deferred to future sibling plugins (great-marketers, great-engineers, great-designers, great-operators). Constellation roadmap lives in the brain vault at `projects/great-minds-ai-company-constellation.md`.
- **No Gottlieb-equivalent.** The orchestrator role is already covered by Gottlieb in great-authors; this plugin does not duplicate it. Publishers personas are dispatched specialists.
- **Default-save.** Every generative skill saves to disk before showing in chat. See `ORCHESTRATING.md`.
- **Bible at `.great-authors/`** is the shared spine across all four plugins. Publishers personas read it before deciding.

### Deferred to v1.0

- `/publishers-design-cover` — Chip Kidd dispatch with optional image-gen
- `/publishers-build-blog-post` — chapter → serial blog post extraction
- `/publishers-publish-github-pages` — Astro book-site → GitHub Pages deploy
- `/publishers-publish-substack` — draft → Substack via API
- `/publishers-cross-promote` — social copy, jacket copy, query letter
- `/publishers-orchestrate-launch` — seven-phase autonomous ship pipeline (mirror `/authors-orchestrate-novel`)
- `/publishers-debate`, `/publishers-critique`, `/publishers-edit` — composite editorial commands

### Real-world test project

The Murder on the Arizona Strip novel at `~/writing/murder-on-the-arizona-strip/` is the v0.1 test project. The book site, trailer, and cover brief generated from it will inform v0.2 of every skill.
