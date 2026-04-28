# publishers/ — publication-form artifacts

This is the directory `/publishers-project-init` copies into a project's root, sibling to `manuscript/` (great-authors) and `film/` (great-filmmakers).

## Subdirectories

| Subdir | Owner | Contents |
|---|---|---|
| `covers/` | Chip Kidd, Diana Vreeland, George Lois | Cover concept briefs, visual briefs, provocation briefs. PNGs in v1.0 once image-gen is wired in. |
| `jacket-copy/` | Tina Brown | Jacket blurbs, cover lines, back-cover copy. |
| `positioning/` | Tina Brown, Bob Silvers, Maxwell Perkins, Jann Wenner, Bennett Cerf | Positioning docs, threshold reads, editorial letters, rollout plans, list strategies. |
| `trailer/` | (composed by `/publishers-build-trailer`) | Trailer concept docs, render manifests. The actual MP4 lives in `film/render/`. |
| `blog-posts/` | (filed for v1.0) | Chapter extractions reformatted as serial blog posts. |
| `social-copy/` | (filed for v1.0) | Launch copy, social posts, query letters. |

## Slug convention

Each artifact saves as `<slug>.md` under the appropriate subdir. The slug is set in `.great-authors/project.md`'s `## Publishing` section's `Current artifact` field. A single project may have multiple artifacts in flight (a cover, a jacket, a launch rollout) — update the current-artifact field as you move between them.

## Filename suffixes

Some personas produce variants of the same artifact type. Suffixes keep them distinct without forcing a single voice:

- `<slug>.md` — primary artifact (Chip Kidd cover, Tina Brown jacket copy)
- `<slug>-provocation.md` — George Lois alternative
- `<slug>-visual-brief.md` — Diana Vreeland alternative
- `<slug>-threshold-read.md` — Maxwell Perkins
- `<slug>-editorial-letter.md` — Bob Silvers
- `<slug>-rollout.md` — Jann Wenner
- `<slug>-list-strategy.md` — Bennett Cerf

The orchestrator picks which to ship; the alternatives stay on disk as the editorial conversation.

## Why this lives at the project level

Publishing artifacts depend on the project's manuscript, film artifacts, and bible. They cannot be project-agnostic. So they live at the project root, owned by the project, committed to the project's repo. The plugin scaffolds the directory; the project owns it from then on.
