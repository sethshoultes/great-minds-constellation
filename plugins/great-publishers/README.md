# Great Publishers

Eight publisher/editor/designer personas (Chip Kidd, Tina Brown, Maxwell Perkins, Jann Wenner, Bob Silvers, Diana Vreeland, Bennett Cerf, George Lois) and four operational skills that take what `great-authors` and `great-filmmakers` produce and ship it as book sites, trailers, blog posts, and cover briefs. A Claude Code plugin. Fourth in the Great Minds constellation:

- [`great-minds-plugin`](https://github.com/sethshoultes/great-minds-plugin) — strategic decision-makers
- [`great-authors-plugin`](https://github.com/sethshoultes/great-authors-plugin) — prose craft
- [`great-filmmakers-plugin`](https://github.com/sethshoultes/great-filmmakers-plugin) — film craft
- **`great-publishers-plugin`** (this repo) — publication form

> **New to the Great Minds constellation?** Start with [`/constellation-start`](https://github.com/sethshoultes/great-minds-plugin) in `great-minds` — it asks 2-3 questions about your project shape and routes to the right plugin.

## Install

**Claude Code:**
```
/plugin marketplace add sethshoultes/great-publishers-plugin
/plugin install great-publishers@sethshoultes
```

**Claude Desktop** (DXT bundle):
```bash
cd distribution/dxt && npm install && npx @anthropic-ai/dxt pack
```
Share the generated `great-publishers.dxt` — teammates double-click to install.

## What's in v0.1

### 8 Personas — at the publication threshold

| Persona | Strength |
|---|---|
| `chip-kidd-designer` | Book covers and visual identity. The most influential book-cover designer of the last 30 years (Knopf). Where the book becomes an object. |
| `tina-brown-editor` | Positioning, audience, jacket copy. The Vanity Fair / New Yorker register — the editor who decides which piece gets read and which gets ignored. |
| `maxwell-perkins-editor` | Final-pass developmental editing at the publication threshold. Scribner editor for Hemingway, Fitzgerald, Wolfe. Distinct from Gottlieb (manuscript-stage editor in great-authors). |
| `jann-wenner-publisher` | Long-form serialized publication. Rolling Stone. The magazine as cultural object — multi-issue rollouts, cultural-moment timing. |
| `bob-silvers-editor` | Intellectual seriousness, scholarly long-form. NYRB. When the work needs to land at the high register without showmanship. |
| `diana-vreeland-editor` | Visual-driven editorial provocation. Vogue, Harper's Bazaar. Image-first stories; the cover as event. |
| `bennett-cerf-publisher` | Building a publishing list, brand strategy across releases. Random House. Multi-title strategy. |
| `george-lois-designer` | Cover provocation, headline-as-image. Esquire. Satirical, culturally sharp covers that argue. |

### 4 MVP Skills

| Skill | What it does |
|---|---|
| `/publishers-channel <persona>` | Load a publisher persona into your current conversation. Cover briefs, positioning docs, jacket copy, and other substantive output auto-save to `publishers/<artifact-type>/<slug>.md` by default. Mirrors `/authors-channel`. |
| `/publishers-project-init` | Scaffold a `publishers/` directory at the project root, sibling to `manuscript/` and `film/`. Subdirs: `covers/`, `jacket-copy/`, `positioning/`, `trailer/`, `blog-posts/`, `social-copy/`. Reads `.great-authors/project.md` for title and genre. |
| `/publishers-build-book-site <project>` | Generates an Astro book-site scaffold from `manuscript/` chapters. v0.1 ships the contract; the working Astro template lands in `templates/astro-book-site/` next pass. |
| `/publishers-build-trailer <project>` | Composes the existing render pipeline (great-filmmakers' `render_keyframes.py`, `render_kling.py`, `render_veo.py`) into a single trailer-build skill. Surfaces engine choice and content-policy implications. |

## Why this plugin

The Great Minds trilogy as it stood produced **artifacts** but not **shippable, public-facing deliverables**. Manuscripts. Storyboards. Render manifests. Strategic decisions. None of them was a book site, a video trailer, a launch jacket, a cover. That's the gap great-publishers fills: it consumes what the other plugins produce and ships it.

The plugin is consciously scoped to **publication form**. Marketing and ad copy live in [`great-marketers`](https://github.com/sethshoultes/great-marketers-plugin) (shipped). Software engineering, product design, and operations are filed for future sibling plugins (`great-engineers`, `great-designers`, `great-operators`) — see [`projects/great-minds-ai-company-constellation`](https://github.com/sethshoultes) for the roadmap.

## Conventions inherited from the trilogy

- **Orchestrator vs. specialist.** Personas are dispatched as sub-agents in clean contexts. The orchestrator never produces the published artifact in-context. See `ORCHESTRATING.md`. The Gottlieb persona in great-authors covers the orchestrator role; great-publishers does NOT add a parallel persona.
- **Default-save.** Every generative skill saves output to disk before showing it in chat. Save to `publishers/<subdir>/<slug>.md`. Document the path in the response. Never strand work in chat.
- **Project-bible reading.** Every persona reads `.great-authors/` at the project root before deciding. The bible is the shared spine across all four plugins.
- **Backend awareness.** Like great-filmmakers' three-backend output formats, the book-site skill is backend-aware — Astro is the v0.1 default; Hugo and Eleventy are stubs.
- **Content-policy awareness.** Veo refuses crime-fiction prompts that name bodies or violence. Kling is more permissive. The trailer-build skill must surface this when the genre signals it.

## Project structure

```
.great-authors/                 # the bible — shared with great-authors
manuscript/                     # great-authors writes here
film/                           # great-filmmakers writes here
publishers/                     # great-publishers writes here (v0.1)
├── covers/                     #   cover briefs (and PNGs in v1.0)
├── jacket-copy/                #   blurbs, positioning copy
├── positioning/                #   audience and pitch docs
├── trailer/                    #   trailer scripts and render manifests
├── blog-posts/                 #   chapter extractions reformatted
└── social-copy/                #   launch copy (v1.0)
```

## Roadmap

- **v0.1** (this release) — eight personas, four MVP skills, DXT bundle, real test against Murder on the Arizona Strip.
- **v0.2** — Astro book-site template lands in `templates/astro-book-site/`; `/publishers-build-book-site` becomes fully functional.
- **v1.0** — Adds `/publishers-design-cover`, `/publishers-build-blog-post`, `/publishers-publish-github-pages`, `/publishers-publish-substack`, `/publishers-cross-promote`, `/publishers-orchestrate-launch`, `/publishers-debate`, `/publishers-critique`, `/publishers-edit`. ~12 skills total. Matches the breadth of great-authors and great-filmmakers.

## License

MIT. See [LICENSE](./LICENSE).
