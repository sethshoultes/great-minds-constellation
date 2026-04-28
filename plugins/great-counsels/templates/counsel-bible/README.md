# counsel/ — counsel-stage artifacts

This is the directory `/counsels-project-init` copies into a project's root, sibling to `manuscript/` (great-authors), `film/` (great-filmmakers), `publishers/` (great-publishers), `marketing/` (great-marketers), `engineering/` (great-engineers), `design/` (great-designers), `operations/` (great-operators).

> ⚠️ **NOT LEGAL ADVICE.** Files in this directory are craft-level writing in the voice of canonical legal/policy/ethics figures. Treat them as reasoning tools, not as representation by counsel.

## Subdirectories

| Subdir | Owner | Contents |
|---|---|---|
| `memos/` | RBG, Marshall, Scalia, Lessig, Wu, Brandeis, Sunstein, Arendt, Rawls (persona-driven by signal) | Legal memos, policy memos, ethics memos |
| `reviews/` | Default panel: RBG + Lessig + Rawls (override available) | Reviews of decisions, policies, practices, prior memos |
| `briefs/` | Persona-driven (typically the most-litigation-shaped: RBG, Marshall, Brandeis) | Formal position papers, briefs |

## Slug convention

Each artifact saves as `<slug>.md` under the appropriate subdir. The slug is set in `CLAUDE.md`'s `## Counsel` section's `Current memo` field. A single project may have multiple memos in flight (different questions, different jurisdictions, different ethical frames) — update the current-memo field as you move between them.

## Filename suffixes

Persona-specific alternatives use suffixes:

- `<slug>.md` — primary memo (auto-selected persona, default Rawls for ethical-reasoning questions)
- `<slug>-rbg-civil-rights.md`, `<slug>-scalia-textualist.md` — when the same question gets multiple persona drafts for comparison (debates)
- `<slug>-veil.md` — Rawls-mode ethical analysis
- `<slug>-policy.md` — full policy document (vs. shorter memo)

The orchestrator picks which to ship; the alternatives stay on disk as the counsel conversation.

## Why this lives at the project level

Counsel artifacts depend on the project's specific decisions, jurisdictions, and ethical context. They cannot be project-agnostic. So they live at the project root, owned by the project, committed to the project's repo. The plugin scaffolds the directory; the project owns it from then on.

## ⚠️ When to seek a real lawyer

If any of the following are true, the project needs licensed counsel beyond what this directory can provide:
- The decision will be challenged in a court
- The decision affects regulated obligations (privacy law, employment law, securities, IP)
- The decision involves crossing jurisdictions
- The decision exposes the user (or others) to civil or criminal liability
- The persona output is being relied on as advice rather than as reasoning

In all of these, dispatch a real attorney. The personas are channels for craft register; they are not your lawyer.
