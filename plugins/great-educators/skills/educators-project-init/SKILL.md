---
name: educators-project-init
description: "Scaffold a new education project in the Great Minds constellation. Creates a project bible with learner profile, sequence map, and assessment plan. Use when starting curriculum, instructional content, or learning design from scratch."
argument-hint: "<project-name>"
allowed-tools: [Read, Write, Bash]
---

# /educators-project-init — scaffold an education project

Creates the minimal bible structure for an education project: learner profile, learning objectives, sequence map, and assessment plan.

## Usage

```
/educators-project-init my-course
```

## Output structure

```
my-course/
  .great-educators/
    project.md          # title, audience, duration, modality
    learner.md          # prior knowledge, misconceptions, sensitive periods
    objectives.md       # what the learner will be able to do
    sequence.md         # module-by-module progression
    assessments.md      # self-checks, checkpoints, summative
    materials/          # lesson scripts, handouts, slides
```

## Procedure

1. Read the project name from the argument (or prompt if missing)
2. Create `.great-educators/` directory
3. Write `project.md` with:
   - Title, target audience, modality (async/ sync/ blended)
   - Estimated duration and session structure
   - Prerequisites the learner must bring
4. Write `learner.md` with:
   - Prior knowledge profile
   - Common misconceptions (loaded with the material)
   - Sensitive periods or readiness windows
5. Write `objectives.md` with:
   - 3-5 learning objectives in "the learner will be able to..." format
   - One objective per line, each testable
6. Write `sequence.md` with:
   - Module-level progression
   - For each module: concept, concrete encounter, abstraction, checkpoint
7. Write `assessments.md` with:
   - Self-checks (learner verifies their own understanding)
   - Formative checkpoints (after each module)
   - Summative assessment (end of sequence)

Do NOT write full lesson content — this is scaffolding only. The Build phase produces the actual materials.

## Related

- `educators-channel` — dispatch educator personas for content writing
- `great-minds:phil-jackson-orchestrator` — for full constellation pipeline planning
