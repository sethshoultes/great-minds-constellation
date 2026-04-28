---
name: researchers-project-init
description: Scaffold a research/ directory at the project root, sibling to manuscript/, film/, publishers/, marketing/, engineering/, design/, operations/, counsel/. Adds a Research section to CLAUDE.md (or creates one). Use when starting research work — citation discipline, literature review, deep-research narrative. NOT ACADEMIC ADVICE — a craft register.
---

# /researchers-project-init

Scaffold the `research/` directory and register it in the project's `CLAUDE.md`.

> ⚠️ **NOT ACADEMIC ADVICE.** Files in `research/` are craft-level writing in the voice of canonical figures. Treat them as drafts and reasoning tools.

## What this does

Creates a `research/` folder with three subdirectories:

```
research/
├── studies/        # Essays, papers, lit reviews, case studies, investigations
├── reviews/        # Peer-style reviews of studies
└── bibliography/   # Citations, sources, annotated bibliographies
```

The `bibliography/` subdir is the load-bearing differentiator vs. `manuscript/` (great-authors) — citation discipline lives separately from the studies that reference them.

Then adds (or creates) a `## Research` section in `CLAUDE.md`:

```markdown
## Research

> ⚠️ **Not academic advice.** Files in `research/` are craft-register writing in the voice of canonical figures. Real research questions require real researchers.

**Path:** `research/` (at project root)
**Current study:** `<user-chosen-slug>` (the study you're actively working on)

Commands that generate research artifacts (`/researchers-channel` save behavior, `/researchers-write-study`, `/researchers-review`) write to `research/<subdir>/<current-study>.md` by default.

The researcher personas read this file plus README.md, prior studies and reviews, the bibliography. For cross-craft projects, also `.great-authors/project.md`.

For technical-mathematical writing rigor, cross-dispatch `great-engineers:don-knuth-engineer`.
For political-philosophy research register, cross-dispatch `great-counsels:hannah-arendt-counsel`.
```

## When to use

- Starting a research project (literature review, science communication piece, deep-research narrative, investigative biography).
- Extending an existing constellation project with a research dimension (the citation discipline behind a book, the lit review behind a product spec, the deep-research piece behind an article).
- Before invoking `/researchers-channel` with save triggers.

## Instructions for Claude

When this skill is invoked:

1. **Resolve the project root.**

2. **Check for existing `research/` directory.** Default skip if exists.

3. **Read existing context.** README.md, CLAUDE.md, prior research docs, the manifest, `.great-authors/project.md` if cross-craft.

4. **Ask one question:** "What's the slug for the study or research question you're starting with? Default: based on what's in the project."

5. **Create the directory tree.** `studies/`, `reviews/`, `bibliography/`.

6. **Update `CLAUDE.md`** with the `## Research` section above. **Disclaimer line stays at the top — do not strip.**

7. **Report:**

   ```
   Created research/ with subdirs:
     studies/  reviews/  bibliography/

   Updated CLAUDE.md with ## Research section.
   Current study: <slug>

   ⚠️ Reminder: research/ holds craft-register writing, not primary research.

   Detected project context:
   - <Research question summary>
   - <Key files present: prior bibliographies, etc.>

   Next:
   - /researchers-channel <persona> for direct collaboration. Best fits for this question: <list 2-3 by signal>
   - /researchers-write-study <topic> to draft a study
   - /researchers-review <path> to peer-review a draft

   For technical rigor: Agent({subagent_type: "great-engineers:don-knuth-engineer", ...})
   For political-philosophy: Agent({subagent_type: "great-counsels:hannah-arendt-counsel", ...})
   ```

## Notes

- Doesn't commit to git.
- The `research/` directory is for ARTIFACTS, not the actual primary research itself (which is the user's).
- For software-only projects, no `.great-authors/` is needed.
- The `current-study` slug names "what we're working on right now."
- **The disclaimer is load-bearing.** Do not strip the "not academic advice" warning from the CLAUDE.md section or from artifact headers.
