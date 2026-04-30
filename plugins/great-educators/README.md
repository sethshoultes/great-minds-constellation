# great-educators

Three educator personas for the Great Minds constellation — instructional content, curriculum sequencing, and learning design.

## Personas

| Persona | Register | Best For |
|---|---|---|
| **Richard Feynman** | Explainer | Making complex concepts feel reachable through physical analogy and first principles |
| **Maria Montessori** | Scaffold designer | Sequencing learning material, isolating difficulty, building self-correcting instruction |
| **Paulo Freire** | Dialogic educator | Co-investigative writing, problem-posing, critical consciousness |

## Skills

- `/educators-project-init` — scaffold a new education project with learner profile, objectives, sequence map, and assessment plan
- `/educators-channel <persona>` — load an educator persona into the current conversation

## Install

Via the Great Minds constellation marketplace:

```
/marketplace install great-educators
```

Or standalone (not recommended — the constellation provides cross-plugin routing):

```
/plugin add github:sethshoultes/great-minds-constellation/plugins/great-educators
```

## Usage

Start a curriculum project:

```
/educators-project-init python-for-beginners
```

Then channel a persona to write a lesson:

```
/educators-channel feynman
> Write a lesson explaining recursion to someone who has never met the concept.
```

Or dispatch for parallel build:

```
Agent({
  subagent_type: "great-educators:maria-montessori-scaffold",
  description: "Sequence a 5-module Python course",
  prompt: "..."
})
```

## What this is NOT

- **Not an academic advising tool.** These are craft registers for writing instructional content, not pedagogical theory.
- **Not a substitute for instructional designers.** The personas write content; they don't replace the human judgment about what learners actually need.
- **Not aligned to specific standards** (Common Core, ISTE, etc.) unless you provide those constraints in the brief.

## Cross-plugin routing

The constellation routinely routes educational work here:
- `great-minds:phil-jackson-orchestrator` → identifies education projects and routes to this plugin
- `great-authors` personas → may be called in parallel for voice-driven narrative within educational content
- `great-researchers` → may be called for fact-grounding in educational material

## Version

1.0.0 — initial release with three personas and two skills.

## License

MIT
