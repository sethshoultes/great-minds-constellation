---
name: designers-project-init
description: Scaffold a design/ directory at the project root, sibling to manuscript/, film/, publishers/, marketing/, engineering/. Adds a Design section to CLAUDE.md (or creates one if absent) so design-stage commands know where to write. Use when starting the design work for a project — UI/UX, brand, design system, or cross-craft.
---

# /designers-project-init

Scaffold the `design/` directory and register it in the project's `CLAUDE.md`.

## What this does

Creates a `design/` folder at the current working directory's project root with three subdirectories:

```
design/
├── specs/         # Design specs, IA docs, interaction specs, flow diagrams
├── audits/        # Design reviews, accessibility audits, heuristic evals
└── systems/       # Design system docs, component libraries, type scales, color tokens
```

Then adds (or creates) a `## Design` section in `CLAUDE.md`:

```markdown
## Design

**Path:** `design/` (at project root)
**Current spec:** `<user-chosen-slug>` (the spec or surface you're actively working on)

Commands that generate design artifacts (`/designers-channel` save behavior, `/designers-write-spec`, `/designers-design-review`) write to `design/<subdir>/<current-spec>.md` by default. Update `Current spec` when moving to a different surface or subsystem.

The designer personas read this file plus the project's `README.md`, brand brief at `design/systems/brand.md` (if present), existing component libraries, and any user research artifacts. For cross-craft projects (writing or film with a UI surface), they also read `.great-authors/project.md`.

For strategic visual taste at the executive register, cross-dispatch `great-minds:jony-ive-designer` — he stays in great-minds.
```

## When to use

- Starting design work on a product (UI/UX, brand, design system).
- Extending an existing project (great-authors / great-filmmakers / great-publishers / great-marketers / great-engineers) with a design surface — for example, the companion app for a book, the website for a film, the dashboard for an engineering tool.
- Before invoking `/designers-channel` with save triggers (which need to know where to write).

## Instructions for Claude

When this skill is invoked:

1. **Resolve the project root.** Use the current working directory unless the user specifies otherwise.

2. **Check for existing `design/` directory.** If it exists, ask: "A `design/` directory already exists. Overwrite the scaffold (destroys existing content) or skip (leaves it alone)? (overwrite/skip)" — default skip.

3. **Read existing project context.** Look for these files and read them if present:
   - `README.md`
   - `CLAUDE.md`
   - Brand brief (`design/systems/brand.md`, `BRAND.md`, `brand-guide.md`)
   - Existing design system docs
   - User research artifacts (transcripts, usability reports)
   - The manifest (`package.json`, `pyproject.toml`, etc.) — informs which designers might be most relevant
   - `.great-authors/project.md` (if cross-craft project)

   This context informs which design personas the user should know about (e.g., if there's a data-heavy dashboard, mention Tufte; if there's a brand identity question, mention Scher; if there's an icon system, mention Kare).

4. **Ask the spec-slug question.** One question:
   - "What's the slug for the spec or surface you're starting with? Default: based on what's in the project. Accept any kebab-case identifier (e.g., `onboarding-v3`, `dashboard-redesign`, `brand-identity-2026`)."

5. **Create the directory tree.** Three empty subdirectories under `design/`:
   - `specs/`
   - `audits/`
   - `systems/`

6. **Update `CLAUDE.md`.** Read the existing file. If it doesn't exist, create one with a minimal header plus the `## Design` section. If it exists and has a `## Design` section, ask whether to overwrite. If it exists without one, append the `## Design` block documented above.

7. **Report:**
   ```
   Created design/ with subdirs:
     specs/  audits/  systems/

   Updated CLAUDE.md with ## Design section.
   Current spec: <slug>

   Detected project context:
   - <Surface summary: dashboard / brand / app / book site / etc.>
   - <Key files present: brand.md, research/, etc.>

   Next:
   - /designers-channel <persona> for direct collaboration. The personas best-fit to this surface are <list 2-3 based on project signals>.
   - /designers-write-spec <feature> to draft a design spec.
   - /designers-design-review <path> to audit existing UI or design system.

   For strategic visual taste at the executive register, dispatch jony-ive-designer from great-minds:
     Agent({subagent_type: "great-minds:jony-ive-designer", ...})
   ```

## Notes

- This skill does not commit to git. The user owns their repository.
- The `design/` directory is for ARTIFACTS at the design stage. The actual UI implementation lives in `src/` (or wherever the project conventions place it). Design artifacts describe, audit, and decide; they don't replace the code.
- For software-only projects, no `.great-authors/` is needed. For cross-craft projects, `.great-authors/` is the bible spine and designer personas read it alongside the design context.
- The `current-spec` slug names "what we're working on right now." Update it when you move to a different surface within the same project.
