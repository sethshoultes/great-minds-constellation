---
name: publishers-build-book-site
description: Generate an Astro book-site scaffold from manuscript/ chapters. v0.1 documents the contract and stubs the wiring — the working Astro template ships in templates/astro-book-site/ in a future release. Use when a manuscript is ready to ship as a public-facing book site (typography-first reading experience, dark/light mode, scroll-based, optional chapter illustrations and embedded clips). Output lands in a sibling repo to the project so the book site has its own deploy lifecycle.
---

# /publishers-build-book-site <project>

Scaffold an Astro book site from a project's `manuscript/` chapters.

## v0.1 status

This skill ships the **contract** for the book-site build in v0.1. The working Astro template — typography-first layout, IntersectionObserver fade-in `<Illustration>` and `<ClipLoop>` components, dark/light mode, scroll-based reading, GitHub Pages config, MDX integration — is being scaffolded in a separate repo and will land at `templates/astro-book-site/` of this plugin in a follow-up release.

In v0.1, the skill does the resolution work, generates the chapter manifest, and reports what would be built. In v0.2 it will copy the template into the target directory and wire in the chapter content.

## What this does

When the Astro template is in place (v0.2+), this skill will:

1. Read the project bible (`.great-authors/project.md`) for title, premise, voice rules.
2. Read all chapters in `manuscript/*.md` in lexicographic order.
3. Read available chapter illustrations in `film/render/kling/keyframes/*.png` and embedded clips in `film/render/<engine>/*.mp4`.
4. Copy the Astro template from `<plugin-install-path>/templates/astro-book-site/` into a sibling directory at `<project-parent>/<project-slug>-book/`.
5. Replace template variables in the copied site:
   - Site title, author, premise from `.great-authors/project.md`
   - Chapter MDX files from `manuscript/`
   - Illustration references for any keyframe whose filename matches a chapter slug
   - Clip references for any MP4 whose filename matches a chapter slug
6. Print the next-steps recipe: `cd <project-slug>-book && npm install && npm run dev` to preview locally; `git init && git push` to ship.

## When to use

- A novel, novella, or essay collection that has shipped through `great-authors` and is ready for a public-facing book site.
- A project that wants typography-first scroll-reading (not a marketing page, not a landing site — the actual book, hosted).
- A project where the book site is the product, distinct from a print release.

Not for: marketing pages (`great-marketers` — see `/marketers-write-launch-copy --channel web`), landing sites for a SaaS or service (out of scope), one-page essays (use a static blog template instead).

## Instructions for Claude

When this skill is invoked with a `<project>` argument (or no argument, meaning the current directory):

1. **Resolve the project root.** If `<project>` is provided, treat it as a directory path or a slug under the user's projects directory. If no argument, use the current working directory.

2. **Verify the project structure:**
   - `.great-authors/project.md` must exist
   - `manuscript/` must exist with at least one `.md` chapter
   - If either is missing, report the missing piece and stop. Do not proceed.

3. **Read the bible:**
   - `.great-authors/project.md` — title, genre, premise, voice rules
   - `.great-authors/voice.md` — voice rules in detail (informs site copy)

4. **Enumerate the manuscript:**
   - List `manuscript/*.md` in lexicographic order (chapter-01.md, chapter-02.md, ...)
   - Read chapter titles from each file's first heading or YAML frontmatter
   - Note word counts per chapter

5. **Enumerate the visual assets** (optional but auto-wired if present):
   - `film/render/kling/keyframes/*.png` — chapter illustrations
   - `film/render/kling/*.mp4` and `film/render/veo/*.mp4` — embedded clips
   - Match by slug: a keyframe named `chapter-01.png` becomes the illustration for `chapter-01.md`

6. **In v0.1, report the scaffold plan.** Print what would be built when the template lands:
   ```
   📚 Book site plan (v0.1 — template not yet in place):

   Project:       <title>
   Source:        <project-root>
   Target:        <project-parent>/<project-slug>-book/
   Chapters:      <N> (chapter-01.md ... chapter-NN.md)
   Total words:   <N>
   Illustrations: <N> matched, <N> chapters without
   Clips:         <N> matched, <N> chapters without

   When templates/astro-book-site/ lands (v0.2):
   - The template copies into <target>
   - Each chapter becomes <slug>.mdx with the chapter body
   - Matched illustrations and clips are wired into the chapter pages
   - npm run dev previews; git push deploys via GitHub Pages workflow

   For now, run /publishers-build-book-site again once v0.2 ships, or
   manually copy the Astro template when you have it and re-run this skill.
   ```

7. **In v0.2+ (post-template-landing), execute the scaffold:**
   - Create `<project-parent>/<project-slug>-book/`
   - Copy `templates/astro-book-site/*` into it
   - Replace template variables in `astro.config.mjs`, `src/site-config.ts`, `package.json`
   - For each chapter, generate `src/content/chapters/<slug>.mdx` with chapter body
   - For each matched illustration, copy into `public/illustrations/<slug>.png` and reference from the chapter MDX
   - For each matched clip, copy into `public/clips/<slug>.mp4` and reference from the chapter MDX
   - Print the next-steps recipe and the `npm install && npm run dev` command

## Backend awareness

Astro is the v0.1/v0.2 default. Hugo and Eleventy are filed as v1.x stubs — when those land, this skill takes a `--backend <name>` argument. For v0.1, the skill is Astro-only.

## What the skill does NOT do

- Does not deploy. Deployment is `/publishers-publish-github-pages` (filed for v1.0). The skill scaffolds the repo; the user pushes.
- Does not write copy. Site copy (about page, author bio, jacket copy on the landing page) comes from `tina-brown-editor` via `/publishers-channel tina` and saves to `publishers/jacket-copy/`. The book-site build reads those files if they exist.
- Does not generate the cover. The cover comes from `chip-kidd-designer`, `diana-vreeland-editor`, or `george-lois-designer` via `/publishers-channel`. The book-site build reads the cover concept brief from `publishers/covers/<slug>.md` if it exists and renders the cover image at the appropriate spot in the site (when the cover image PNG itself is present — image-gen integration is filed for v1.0's `/publishers-design-cover`).
- Does not write the chapters. Chapters come from `great-authors`. Don't pre-generate or paraphrase.

## Notes

- The book-site lives in a sibling repo by design. This separates the book's deploy lifecycle from the project's working directory and lets the book site be served from `<project>.example.com` or `username.github.io/<project>` without entangling the manuscript repo.
- Re-running the skill on an existing book-site directory should ask whether to overwrite chapters (which would lose any post-scaffold edits) or skip (leaving them alone). Default skip.
- The `<ClipLoop>` and `<Illustration>` components in the template are intentionally minimal: typography-first reading is the priority. Animation and motion are deferred to the chapter clip files themselves; the components just place them quietly in the page rhythm.
