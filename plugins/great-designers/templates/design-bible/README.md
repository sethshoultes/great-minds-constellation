# design/ — design-stage artifacts

This is the directory `/designers-project-init` copies into a project's root, sibling to `manuscript/` (great-authors), `film/` (great-filmmakers), `publishers/` (great-publishers), `marketing/` (great-marketers), `engineering/` (great-engineers).

## Subdirectories

| Subdir | Owner | Contents |
|---|---|---|
| `specs/` | Norman, Spool, Rams, Cagan, etc. (persona-driven by signal) | Design specs, IA docs, interaction specs, flow diagrams |
| `audits/` | Default panel: Norman + Spool + Rams (override available) | Design reviews, accessibility audits, heuristic evals |
| `systems/` | Rams, Scher, Kare, Tufte (depending on layer) | Design system docs, component libraries, type scales, color tokens |

## Slug convention

Each artifact saves as `<slug>.md` under the appropriate subdir. The slug is set in `CLAUDE.md`'s `## Design` section's `Current spec` field. A single project may have multiple design specs in flight (different surfaces, different subsystems) — update the current-spec field as you move between them.

## Filename suffixes

Persona-specific alternatives use suffixes:

- `<slug>.md` — primary spec (auto-selected persona, default Norman for cognitive flows)
- `<slug>-rams-restraint.md`, `<slug>-tufte-density.md`, etc. — when the same surface gets multiple persona drafts for comparison
- `<slug>-a11y.md` — accessibility-focused audit
- `<slug>-system.md` — design-system-level (vs feature-level) review

The orchestrator picks which to ship; the alternatives stay on disk as the design conversation.

## Why this lives at the project level

Design artifacts depend on the project's brand, voice, user research, and existing surface. They cannot be project-agnostic. So they live at the project root, owned by the project, committed to the project's repo. The plugin scaffolds the directory; the project owns it from then on.
