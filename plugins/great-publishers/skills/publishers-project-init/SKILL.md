---
name: publishers-project-init
description: Scaffold a publishers/ directory at the project root, sibling to manuscript/ and film/. Adds a ## Publishing section to .great-authors/project.md so publishing-stage commands know where to write. Use when starting the publication-form work for a project — the moment after great-authors and great-filmmakers have produced their artifacts and you're ready to ship.
---

# /publishers-project-init

Scaffold the `publishers/` directory and register it in the project bible.

## What this does

Creates a `publishers/` folder at the current working directory's project root with six empty subdirectories:

```
publishers/
├── covers/         # Cover concept briefs (and PNGs in v1.0 with image-gen)
├── jacket-copy/    # Blurbs, cover lines, positioning copy
├── positioning/    # Audience and pitch docs, threshold reads, list strategies
├── trailer/        # Trailer scripts and render manifests
├── blog-posts/     # Chapter extractions reformatted (v1.0)
└── social-copy/    # Launch copy (v1.0)
```

Then adds a `## Publishing` section to `.great-authors/project.md`:

```markdown
## Publishing

**Path:** `publishers/` (at project root, sibling to `.great-authors/`, `manuscript/`, and `film/`)
**Current artifact:** `<user-chosen-slug>`

Commands that generate publishing artifacts (`/publishers-channel` save triggers, `/publishers-build-book-site`, `/publishers-build-trailer`) write to `publishers/<subdir>/<current-artifact>.md` by default. Update `Current artifact` when moving to a different launch or a different work in a series.
```

## When to use

- Starting the publication-form stage on a project that has manuscripts and/or film artifacts.
- Extending an existing great-authors / great-filmmakers project with publishing-stage work.
- Before invoking `/publishers-channel` with save triggers (which need to know where to write).

## Instructions for Claude

When this skill is invoked:

1. **Verify `.great-authors/` exists** in the current working directory. If not, tell the user: "This skill assumes a project bible at `.great-authors/`. Run `/authors-project-init` (from great-authors-plugin) first to scaffold the bible, then re-run this skill."

2. **Check for existing `publishers/` directory.** If it exists, ask: "A `publishers/` directory already exists. Overwrite the scaffold (destroys existing content) or skip (leaves it alone)? (overwrite/skip)" — default skip.

3. **Read `.great-authors/project.md`** to import title and genre. If a `## Film` section exists (from `/filmmakers-project-init`), note the current scene; the publishing artifact slug often aligns with it.

4. **Ask the artifact-slug question.** One question:
   - "What's the slug for the publishing artifact you're starting with? Default: same as the project slug. Accept any kebab-case identifier (e.g., `arizona-strip-launch`, `chapter-01-blog`, `season-rollout`)."

5. **Create the directory tree.** Six empty subdirectories under `publishers/`:
   - `covers/`
   - `jacket-copy/`
   - `positioning/`
   - `trailer/`
   - `blog-posts/`
   - `social-copy/`

6. **Update `.great-authors/project.md`.** Read the existing file. If it already has a `## Publishing` section, ask whether to overwrite it. If not, append the `## Publishing` block documented above, substituting the user's chosen slug into `Current artifact`.

7. **Report:**
   ```
   Created publishers/ with subdirs:
     covers/  jacket-copy/  positioning/  trailer/  blog-posts/  social-copy/

   Updated .great-authors/project.md with ## Publishing section.
   Current artifact: <slug>

   Next:
   - /publishers-channel <persona> to channel a publisher (chip, tina, perkins, jann, silvers, diana, bennett, george).
     Substantive output (cover briefs, jacket copy, positioning docs) saves to publishers/<subdir>/<slug>.md by default.
   - /publishers-build-book-site to scaffold an Astro book site from manuscript/ chapters (v0.2+).
   - /publishers-build-trailer to compose the great-filmmakers render scripts into a trailer.
   ```

## Notes

- This skill does not commit to git. The user owns their repository.
- The `publishers/` directory is for ARTIFACTS at the publication-form stage. The bible stays at `.great-authors/`; the manuscript stays at `manuscript/`; film artifacts stay at `film/`. Each has its own owner.
- If the user's project has no `.great-authors/` directory at all (working on a one-off cover for a standalone piece), the skill still creates `publishers/` but emits a warning that personas won't have bible context to read before working.
- The `current-artifact` slug is editorial: a single project might have multiple publishing artifacts (a cover, a jacket, a launch rollout, a magazine extraction) and the slug is just the convention for "what we're working on right now." Update it when you move to a different artifact within the same project.
