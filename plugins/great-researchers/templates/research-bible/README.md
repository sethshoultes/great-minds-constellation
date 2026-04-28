# research/ — research-stage artifacts

This is the directory `/researchers-project-init` copies into a project's root, sibling to all other constellation plugin directories.

> ⚠️ **NOT ACADEMIC ADVICE.** Files in this directory are craft-level writing in the voice of canonical figures. Treat them as drafts and reasoning tools, not as peer-reviewed primary research.

## Subdirectories

| Subdir | Owner | Contents |
|---|---|---|
| `studies/` | Sagan, Gould, Roach, Sacks, Gawande, Diamond, Wilson, Skloot, Caro (persona-driven by signal) | Essays, papers, lit reviews, case studies, investigations |
| `reviews/` | Default panel: Gould + Sagan + Wilson (override available) | Peer-style reviews of studies, claims, or research artifacts |
| `bibliography/` | Citation discipline (any persona that cites a source touches this) | Annotated bibliographies, primary sources, citation keys |

## The bibliography is load-bearing

Unlike other constellation plugins, `research/bibliography/` is **not** an output subdir for finished artifacts. It's the project's citation foundation. Every study should cite from it; every review should verify against it. When a study cites a source not in the bibliography, the bibliography gets updated as part of that study's commit.

A research project without a bibliography is an essay. With a bibliography, it's a study.

## Slug convention

Each study saves as `<slug>.md` under the appropriate subdir. The slug is set in `CLAUDE.md`'s `## Research` section's `Current study:` field.

## Filename suffixes

- `<slug>.md` — primary study (auto-selected persona; default Gould for essay-as-research)
- `<slug>-sagan-public.md`, `<slug>-gawande-systems.md` — when the same topic gets multiple persona drafts for comparison
- `<slug>-litreview.md` — literature-review format
- `<slug>-case.md` — case-study format

## When to seek a domain expert

If any of the following are true, the project needs a real researcher beyond what this directory can provide:

- The work is being submitted to a peer-reviewed journal
- The conclusions will be cited by other researchers
- The work involves human subjects, IRB approval, or any ethics review
- The work claims a primary finding that hasn't been independently verified
- The persona output is being relied on as research rather than as draft

In all of these, dispatch a real researcher. The personas are channels for craft register; they are not your advisor.
