---
name: educators-channel
description: "Dispatch an educator persona for a specific instructional task. Routes to Feynman (explainer), Montessori (scaffold designer), or Freire (dialogic educator) based on the task shape. Use when you need educational content written by a specific pedagogical voice."
argument-hint: "<persona-name> <task-description>"
allowed-tools: [Agent]
---

# /educators-channel — dispatch an educator persona

Routes instructional tasks to the right educator persona.

## Usage

```
/educators-channel feynman "explain recursion to a 12-year-old"
/educators-channel montessori "sequence this Python tutorial for absolute beginners"
/educators-channel freire "rewrite this corporate training as problem-posing dialogue"
```

## Routing

| Persona | Best for | Trigger |
|---|---|---|
| **feynman** | Explaining complex concepts simply, finding the right analogy, stripping jargon | "explain," "analogy," "jargon," "first principles" |
| **montessori** | Sequencing learning material, isolating difficulty, self-correcting instruction | "sequence," "scaffold," "beginner," "tutorial," "self-check" |
| **freire** | Dialogic education, critical consciousness, problem-posing | "dialogue," "problem-posing," "critical," "co-investigate," "praxis" |

## Dispatch brief template

```
Agent({
  subagent_type: "great-educators:<persona-name>",
  description: "<brief task description>",
  prompt: "<self-contained brief with all context>"
})
```

## Notes

- If the persona is not installed, the operator will get a clear error. Install via `/install great-educators`.
- For multi-persona education projects (e.g., Feynman explains + Montessori sequences + Freire dialogues), use the constellation pipeline: `/educators-project-init` first, then parallel dispatch.
