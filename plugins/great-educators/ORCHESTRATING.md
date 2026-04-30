# ORCHESTRATING great-educators

How this plugin fits in the Great Minds constellation pipeline.

## When to dispatch

Phil Jackson (great-minds:phil-jackson-orchestrator) routes here when:
- The project shape is educational content, curriculum, or instructional design
- The brief mentions "explain," "sequence," "scaffold," "tutorial," "lesson," "course"
- The user says "make this less top-down" or "rewrite as dialogue"

## Pipeline entry points

| Entry point | Delivers |
|---|---|
| `/educators-project-init` | `.great-educators/` project bible |
| `/educators-channel <persona>` | In-session collaboration with an educator voice |
| Direct `Agent({subagent_type: "great-educators:..."})` | Parallel Build phase output |

## Typical flows

**Curriculum from scratch:**
```
/educators-project-init     → bible scaffold
/educators-channel feynman → draft lessons (or parallel dispatch)
/educators-channel montessori → sequence review
/educators-channel freire → dialogic revision (optional)
```

**Technical tutorial:**
```
/educators-channel feynman → explain the concept
/great-authors:orwell-persona → tighten the prose
/great-minds:margaret-hamilton-qa → verify accuracy
```

## What this plugin does NOT do

- It does not write tests or code (great-engineers)
- It does not design covers or layout (great-publishers)
- It does not write marketing copy (great-marketers)
- It does not provide legal or policy advice (great-counsels)

## Cross-plugin dispatch matrix

| This plugin needs... | Dispatch to |
|---|---|
| Voice-driven narrative prose | great-authors |
| Fact-grounding / research | great-researchers |
| Strategic framing | great-minds |
| Design of learning materials | great-designers (visual) |
