---
name: counsels-project-init
description: Scaffold a counsel/ directory at the project root, sibling to manuscript/, film/, publishers/, marketing/, engineering/, design/, operations/. Adds a Counsel section to CLAUDE.md (or creates one if absent) so counsel-stage commands know where to write. Use when starting the legal/policy/ethics work for a project. NOT LEGAL ADVICE — a craft register.
---

# /counsels-project-init

Scaffold the `counsel/` directory and register it in the project's `CLAUDE.md`.

> ⚠️ **NOT LEGAL ADVICE.** Files in the `counsel/` directory are craft-level writing in the voice of canonical figures. Treat them as reasoning tools, not as representation by counsel.

## What this does

Creates a `counsel/` folder at the current working directory's project root with three subdirectories:

```
counsel/
├── memos/        # Legal memos, policy memos, ethics memos
├── reviews/      # Reviews of decisions, policies, practices, prior memos
└── briefs/       # Formal position papers, briefs
```

Then adds (or creates) a `## Counsel` section in `CLAUDE.md`:

```markdown
## Counsel

> ⚠️ **Not legal advice.** Files in `counsel/` are craft-register writing in the voice of canonical legal/policy/ethics figures. Real legal matters require licensed counsel.

**Path:** `counsel/` (at project root)
**Current memo:** `<user-chosen-slug>` (the memo or question you're actively working on)

Commands that generate counsel artifacts (`/counsels-channel` save behavior, `/counsels-write-memo`, `/counsels-review`) write to `counsel/<subdir>/<current-memo>.md` by default. Update `Current memo` when moving to a different question.

The counsel personas read this file plus the project's `README.md`, the question or decision under review, prior memos at `counsel/memos/`, prior reviews at `counsel/reviews/`. For cross-craft projects, they also read `.great-authors/project.md`.

For Stoic executive mediation when the ethical question is also an interpersonal/orchestration one, cross-dispatch `great-minds:marcus-aurelius-mod` — he stays in great-minds.
```

## When to use

- Starting legal, policy, or ethics analysis on a project (privacy policy review, antitrust analysis, ethical review of a decision, policy memo on a regulatory question).
- Extending an existing project (great-authors / great-filmmakers / great-publishers / great-marketers / great-engineers / great-designers / great-operators) with a counsel question — for example, the publishing-rights question for a book, the fair-use question for a film, the privacy question for a SaaS.
- Before invoking `/counsels-channel` with save triggers (which need to know where to write).

## Instructions for Claude

When this skill is invoked:

1. **Resolve the project root.** Use the current working directory unless the user specifies otherwise.

2. **Check for existing `counsel/` directory.** If it exists, ask: "A `counsel/` directory already exists. Overwrite the scaffold (destroys existing content) or skip (leaves it alone)? (overwrite/skip)" — default skip.

3. **Read existing project context.** Look for these files and read them if present:
   - `README.md`
   - `CLAUDE.md`
   - Prior counsel docs (memos, reviews, briefs)
   - The decision, policy, or practice under review
   - The manifest (`package.json`, etc.) — informs which counsel personas might be most relevant
   - `.great-authors/project.md` (if cross-craft project)

   This context informs which counsel personas the user should know about (e.g., if the project has a privacy concern, mention Brandeis; if it's a platform/antitrust question, mention Wu; if it's a regulatory design question, mention Sunstein).

4. **Ask the memo-slug question.** One question:
   - "What's the slug for the memo or question you're starting with? Default: based on what's in the project. Accept any kebab-case identifier (e.g., `data-retention-2026`, `platform-antitrust-analysis`, `consent-flow-ethics`)."

5. **Create the directory tree.** Three empty subdirectories under `counsel/`:
   - `memos/`
   - `reviews/`
   - `briefs/`

6. **Update `CLAUDE.md`.** Read the existing file. If it doesn't exist, create one with a minimal header plus the `## Counsel` section. If it exists and has a `## Counsel` section, ask whether to overwrite. If it exists without one, append the `## Counsel` block documented above. **The disclaimer line stays at the top of the section — do not strip it.**

7. **Report:**
   ```
   Created counsel/ with subdirs:
     memos/  reviews/  briefs/

   Updated CLAUDE.md with ## Counsel section.
   Current memo: <slug>

   ⚠️ Reminder: counsel/ holds craft-register writing, not legal advice.

   Detected project context:
   - <Counsel question summary: privacy / antitrust / ethics / etc.>
   - <Key files present: prior memos, decisions, etc.>

   Next:
   - /counsels-channel <persona> for direct collaboration. The personas best-fit to this question are <list 2-3 based on signals>.
   - /counsels-write-memo <question> to draft a memo.
   - /counsels-review <path> to review a decision, policy, or draft.

   For Stoic executive mediation, dispatch marcus-aurelius-mod from great-minds:
     Agent({subagent_type: "great-minds:marcus-aurelius-mod", ...})
   ```

## Notes

- This skill does not commit to git. The user owns their repository.
- The `counsel/` directory is for ARTIFACTS at the counsel stage. The actual legal representation, if required, is the user's to obtain. Counsel artifacts describe, analyze, and reason; they do not represent.
- For software-only projects, no `.great-authors/` is needed. For cross-craft projects, `.great-authors/` is the bible spine and counsel personas read it alongside the counsel context.
- The `current-memo` slug names "what we're working on right now." Update it when you move to a different question.
- **The disclaimer is load-bearing.** Do not strip the "not legal advice" warning from the CLAUDE.md section or from artifact headers.
