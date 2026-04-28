# Astro Book Site Template

This directory will hold the working Astro book-site template that `/publishers-build-book-site` copies into a project. **In v0.1, the template ships separately and lands here in v0.2.**

## What v0.2 will provide

A minimal, typography-first Astro project with:

- **MDX-based chapters.** One file per chapter, content authored in Markdown with embedded React/Astro components for illustrations and clip loops.
- **Typography-first reading.** Long-form serif body, careful measure, generous line height. The site reads like a book, not like a web page.
- **`<Illustration>` component.** IntersectionObserver fade-in for chapter PNGs from `film/render/kling/keyframes/`. Quiet, deferred-load, accessibility-friendly.
- **`<ClipLoop>` component.** IntersectionObserver-triggered short MP4 loops for embedded scenes. `preload="metadata"`, autoplay-on-visible, looped, muted by default.
- **Dark/light mode.** System preference by default; user-toggleable. Both modes carefully designed for reading at length.
- **Scroll-based reading.** No pagination by default; the chapter scrolls. Optional reading-progress bar.
- **GitHub Pages deploy config.** A `.github/workflows/deploy.yml` that builds the site on push and deploys to GitHub Pages. Custom domain configurable.
- **Site config.** `src/site-config.ts` carries title, author, premise, color palette, and reading preferences. The book-site build skill replaces these from `.great-authors/project.md`.

## How `/publishers-build-book-site` will use it

```
1. /publishers-build-book-site is invoked with a <project> argument
2. The skill verifies .great-authors/ and manuscript/ exist
3. The skill copies templates/astro-book-site/ into <project-parent>/<project-slug>-book/
4. The skill replaces template variables in src/site-config.ts and astro.config.mjs
5. The skill generates src/content/chapters/<slug>.mdx for each manuscript chapter
6. The skill wires in matched illustrations and clips by slug
7. The user runs: cd <project-slug>-book && npm install && npm run dev
```

## Why the template ships separately

The Astro template is itself a substantial deliverable — its own components, build config, deploy workflow, and design decisions. It's being scaffolded in a separate repo so the design can iterate independently of the plugin manifest. When the template stabilizes, it copies into this directory and the v0.2 release cuts.

For now, `/publishers-build-book-site` reports its build plan but does not copy a template. Re-run the skill once v0.2 ships.
