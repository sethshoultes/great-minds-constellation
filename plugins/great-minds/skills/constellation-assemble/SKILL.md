---
name: constellation-assemble
description: "Integrate the parallel Build phase outputs into a single assembled artifact, verifying the build-contract first and flagging any contradictions. Reads build-contract.md, every build/*.md file, and the plan's assembled-artifact template. Dispatches a functional implementer (not a named persona) to do the integration. Catches the rare case where a Build output slipped past QA. Use after every Build agent has reported done and before Margaret runs assembled-artifact QA."
argument-hint: "<optional path to project root — defaults to current working directory>"
allowed-tools: [Read, Write, Bash, Glob, Grep]
---

# /constellation-assemble — integrate Build outputs into the final artifact

After parallel Build agents finish, the operator faces a small but high-stakes integration step: read every `build/*.md` file, confirm each honors the contract, slot them into the final artifact's template, and write the result. Done by hand it's where contradictions slip through (recipe #2's opening-scene divergence) — the failure mode that motivated the build-contract pattern in the first place.

This skill mechanizes the integration *after* the contract has done its upstream job. Contract violations should already be impossible by this point, but the skill verifies one more time before writing — defense in depth.

## When to use

After the Build phase. All `build/*.md` files specified in the plan exist; each Build agent has reported done.

```
/constellation-status     # confirm Build is fully done
/constellation-assemble   # integrate
# Margaret runs QA on the assembled artifact (existing pipeline)
```

## When NOT to use

- **Mid-Build** — wait for all parallel agents. Partial assembly produces a partial artifact and burns the operator's trust in the skill.
- **Without a contract** — if Debate didn't emit `build-contract.md`, this skill cannot verify before writing. It will still assemble, but it will warn loudly that contradictions are not being checked. Better to run a Debate pass first.
- **For craft-style assembly** — if integration requires reconciling tone across writers, smoothing voice transitions, or other editorial judgment, dispatch `great-authors:gottlieb-persona` instead. This skill is for mechanical integration, not editorial work.

## Procedure

### Step 1 — Read the contract

```bash
test -f build-contract.md && cat build-contract.md
```

If the file does not exist, emit this warning and continue:

> ⚠️ No `build-contract.md` found at project root. Assembly will proceed without contract verification — contradictions between Build outputs will NOT be detected. If Debate produced testable invariants, save them as `build-contract.md` and re-run.

If the contract exists, parse it. Look for the `## Build contract — non-negotiable invariants` heading (Steve Jobs's persona format) or any structured invariant list. Extract:

- The numbered invariants ("locked decisions")
- The "Things that must NOT happen" list (anti-requirements)

### Step 2 — Inventory the Build outputs

```bash
ls build/*.md 2>/dev/null
```

For each file, read its contents. Build a mapping: `<file-path>` → `<contents>`. Note the persona that produced each (parseable from the file's frontmatter, or from the filename pattern `<deliverable>.md` cross-referenced against `plan.md`).

### Step 3 — Read the plan's assembled-artifact template

Look in `plan.md` (or `PLAN.md`) for a section describing the assembled artifact:

- Output path (e.g., `proposal.md`, `index.html`, `memo.md`)
- Section structure (e.g., "Section 1: Overview, Section 2: Author Bio, ...")
- Slot mapping (e.g., "Section 3 takes content from `build/sample-chapter.md`")

If the plan doesn't specify a template, infer one: take the build/*.md files in the order specified by the plan's dispatch list and concatenate under section headers derived from each file's first H1.

### Step 4 — Verify the contract before writing

For each invariant in the contract, scan every Build output for compliance:

- Positive invariants ("Book 2 opens on Bluff Street") → check that the relevant Build output (typically the sample chapter or opening scene) honors the rule.
- Anti-requirements ("No body in Chapter 1") → grep across all outputs for the forbidden pattern.

If any invariant is violated, **STOP**. Do not write the assembled artifact. Emit a structured violation report:

```
## Contract violation — Assembly blocked

Invariant: "Book 2 opens on Bluff Street with the stalker. No body in Chapter 1."
Violated by: build/sample-chapter.md
Evidence: Line 14 — "He found the body at sunrise on the Strip."

This contradicts the locked Debate decision. Resolve by either:
1. Rewriting build/sample-chapter.md to honor the invariant (re-dispatch the Build agent), OR
2. Amending build-contract.md (re-run a Debate pass — do NOT patch the contract here).

Assembly will not proceed until the violation is resolved.
```

The skill's response ends there. The operator (or orchestrator) handles the rewrite.

### Step 5 — Dispatch a functional implementer for integration

If contract verification passes (or no contract exists and the warning was emitted), dispatch a functional-role implementer to integrate. Use the Agent tool:

```
Agent({
  subagent_type: "documentation-writer",
  description: "Assemble final artifact from Build outputs",
  prompt: "<self-contained brief — see template below>"
})
```

Use a **functional implementer** (`documentation-writer` for prose, `code-reviewer` for technical artifacts, etc.), NOT a named persona like Gottlieb or Steve. Named personas reduce factual accuracy on knowledge-heavy tasks; assembly is fundamentally factual integration. Save named personas for craft work.

The dispatch brief MUST be self-contained:

```
You are integrating the Build phase outputs of a constellation pipeline run into a
single assembled artifact. This is mechanical integration, not editorial work — do
not rewrite the inputs, smooth tone, or impose your own judgment on register choices.

## Inputs

build-contract.md: <full text inline>

build/<file-1>.md: <full text inline>
build/<file-2>.md: <full text inline>
build/<file-3>.md: <full text inline>
build/<file-4>.md: <full text inline>

Assembled-artifact template (from plan.md):
<full template inline>

Output path: <path>

## Your job

1. Slot each Build output into the appropriate section of the template.
2. Preserve each Build output's prose verbatim — do not edit, smooth, or
   "improve" the language. Each section was written by a craft persona; your
   role is integration, not rewrite.
3. Add only the connective tissue the template requires (section headers,
   table of contents, frontmatter, page breaks). No new prose content.
4. If a Build output is missing or empty, leave that section with a clear
   placeholder: "[BUILD OUTPUT MISSING — <path>]". Do not invent content.
5. If you find a contradiction the contract verification missed (e.g., two
   Build outputs reference different opening scenes), STOP. Do not write the
   artifact. Report the contradiction with file paths and exact line numbers.

## Output

Write the assembled artifact to <output-path>. Report back:

- Output path written
- Section count
- Total word count
- Any anomalies you flagged (with file:line references)
```

### Step 6 — Report back to the operator

After the implementer returns, produce a structured summary:

```
## Assembly complete

Output: <path> (<word-count> words, <section-count> sections)

Contract verification: ✓ <n> invariants honored, ✓ <m> anti-requirements respected
(or: ⚠️ Contract not verified — no build-contract.md present)

Build outputs integrated:
  build/<file-1>.md (<lines> lines, persona: <name>)
  build/<file-2>.md (<lines> lines, persona: <name>)
  ...

Anomalies flagged by the implementer:
  <any reported contradictions or missing data>
  (or: "(none)")

Next step: Margaret QA on <output-path>.
```

## Failure modes the skill catches

- **Build output missing** — the file specified in the plan doesn't exist on disk. Skill names it explicitly and stops.
- **Contract violation** — any Build output contradicts a locked invariant or anti-requirement. Skill stops before writing; operator decides whether to re-dispatch the Build agent or amend the contract.
- **Contradiction across outputs** — two Build outputs disagree on a fact the contract didn't lock (the contract was incomplete). Implementer flags it; operator resolves before re-running.
- **Empty Build output** — a file exists but has zero or near-zero content. Skill flags the empty file and asks whether to proceed or stop.

## Failure modes the skill does NOT catch

- **Build outputs that honor the contract but are individually wrong** — e.g., a sample chapter that opens on Bluff Street as required but is poorly written. That's QA's job (Margaret + Gottlieb).
- **Tone drift across outputs** — e.g., one Build agent writes formally, another casually, but neither violated the contract. That's editorial work for Gottlieb, not assembly.
- **Strategic mistakes in the plan** — if the plan's section template is wrong for the project shape, the assembled artifact will be wrong. That's an upstream Plan-phase problem.

## Boundaries

This skill is read-mostly:

- It reads `build-contract.md`, `plan.md`, all `build/*.md` files
- It writes ONE file: the assembled artifact at the path specified by the plan
- It does NOT modify Build outputs, the contract, or the plan
- It does NOT dispatch Build agents (it is downstream of Build, never restarts it)
- It does NOT mark tasks complete (operator's call)

## Related

- `phil-jackson-orchestrator` — produces the plan with the assembled-artifact template
- `steve-jobs-visionary` — produces `build-contract.md` in the Debate phase
- `margaret-hamilton-qa` — runs QA on the assembled artifact post-assembly
- `/constellation-status` — pre-flight check that all Build outputs are present before assembly
