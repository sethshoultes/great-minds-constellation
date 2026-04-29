---
name: constellation-status
description: "Single-glance project status for a constellation pipeline run. Reads the plan, checks which build artifacts have been produced, surfaces what phase the project is in, lists active background tasks, and computes elapsed time. Use during a parallel-dispatch run to answer the question 'what's the swarm doing right now?' without polling each background agent individually."
argument-hint: "<optional path to project root — defaults to current working directory>"
allowed-tools: [Read, Bash, Glob]
---

# /constellation-status — what's the swarm doing right now?

When the constellation runs in a parallel-dispatch pattern (4+ Build agents in flight, QA queued, Review pending), the operator loses visibility quickly. Each agent runs in isolation; each writes its output to a deterministic path; the operator has to poll each individually with `TaskOutput` or wait for notifications. This skill gives you a single board view.

## What this does

Reads the project's plan and filesystem, maps what's planned to what's been produced, lists in-flight work, and prints a structured status block.

This skill is **read-only** and **non-destructive**. It does not dispatch agents, does not modify files, does not change state. It is purely inspection.

## Usage

```
/constellation-status                    # status for current working directory
/constellation-status path/to/project    # status for a specific project
```

## Procedure

### Step 1 — Read the plan

Look for a planning artifact at the project root, in this priority order:

```bash
test -f plan.md && cat plan.md
test -f PLAN.md && cat PLAN.md
test -f team-brief.md && cat team-brief.md
test -f .great-minds.json && cat .great-minds.json
```

If none exists, report: *"No plan artifact found at project root. Run `/team-build` or `/agency-plan` to produce one, then retry."*

### Step 2 — Extract the workstream list

Parse the plan for dispatch entries. Phil Jackson's plan output uses the `### Dispatch Brief: <persona>` format with a deliverable path. Each dispatch is a workstream. Extract:

- **Persona** (e.g., `great-authors:mccarthy-persona`)
- **Phase** (Discovery, Debate, Plan, Build, QA, Review — usually inferable from the dispatch heading or the deliverable path)
- **Deliverable path** (e.g., `build/sample-chapter.md`)

If the plan format is non-standard, fall back to listing all `build/*.md` and `qa-*.md` and `review.md` paths mentioned in the plan.

### Step 3 — Check filesystem against plan

For each planned deliverable, check whether the file exists:

```bash
test -f <deliverable-path> && echo "DONE" || echo "PENDING"
```

For DONE files, read the modification time:

```bash
stat -f "%Sm" -t "%Y-%m-%d %H:%M" <path>     # macOS
stat -c "%y" <path>                            # linux
```

### Step 4 — Check active tasks via TaskList

Call the `TaskList` tool to get the operator-tracked task list. Filter for tasks with status `in_progress` or `pending` whose subject or description references a constellation persona name (look for `<plugin>:<persona>` patterns or persona-name fragments like `mccarthy`, `gottlieb`, `carmack`).

For each in_progress task, compute elapsed time from the task's creation timestamp.

### Step 5 — Identify the current phase

Phase is the latest phase that has at least one workstream in flight or pending:

| Phase | Signal |
|-------|--------|
| Discovery | discovery.md exists or planned |
| Debate | debate.md or `build/*-debate.md` exists or planned |
| Plan | plan.md exists; no build/* files yet |
| Build | any `build/*.md` exists or pending |
| Assembly | assembled artifact (e.g., `proposal.md`, `index.html`, `memo.md`) referenced in plan but not yet present |
| QA | `qa-report.md` planned or in flight |
| Review | `review.md` or `<persona>-review.md` planned or in flight |
| Ship | all phases above complete; ship verdict captured |

If multiple phases have activity (e.g., Build still running while QA queued), name the latest phase with active work.

### Step 6 — Produce the status block

Format the output exactly like this:

```
Constellation status — <project-name>

Phase: <current-phase>  (<n-active>/<n-total> workstreams in flight)

Active workstreams:
  <Persona> (<Phase>: <Deliverable>) — <elapsed> min elapsed
  ...
  (or "(none in flight)")

Completed workstreams:
  <Persona> → <path> (<timestamp>)
  ...
  (or "(none yet)")

Pending workstreams:
  <Persona> (<Phase>: <Deliverable>) — awaiting dispatch
  ...
  (or "(none queued)")

Plan source: <path-to-plan-artifact>
```

### Step 7 — Report missing data honestly

If the skill cannot determine something (plan format unfamiliar, persona names not parseable, task list empty during a known-active run), say so directly. Do not synthesize plausible-looking status. *"Plan format does not match Phil Jackson's dispatch brief schema; falling back to filesystem-only status (no persona attribution)."* — that's better than a confident-but-wrong report.

## Boundaries

This skill **does not**:

- Dispatch agents. To dispatch, use `/team-build` (for the team brief) or call the `Agent` tool directly.
- Modify state. No file writes, no task updates, no notifications.
- Predict completion times. *"45 minutes elapsed"* is observed; *"5 minutes remaining"* would be a guess.
- Resolve contradictions between plan and filesystem. If the plan says Carmack should have written to `build/engine.md` but the file is missing AND no task is in flight, the status reports both states honestly and lets the operator interpret.

## When this skill helps

- During a parallel Build phase with 3+ agents in flight; replaces polling each one
- After a long-running session resumes; tells you what's done vs. still queued
- Before Assembly; confirms all build inputs are present and named correctly
- Pre-QA gate; confirms the artifacts QA needs to review actually exist

## When this skill does not help

- Single-agent serial runs (just check the artifact path directly)
- Pre-Plan stage (no plan to read against; use `/team-build` instead)
- Post-ship (the project is done; brain entries matter more than live status)

## Related

- `/team-build` — pre-flight pick-the-team skill (produces `team-brief.md` and optionally `.claude/settings.json`)
- `phil-jackson-orchestrator` — the persona whose plan output this skill reads
- `margaret-hamilton-qa` — the persona whose QA reports this skill detects
