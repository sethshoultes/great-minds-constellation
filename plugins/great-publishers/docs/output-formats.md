# Output formats

Backend-aware specs for the artifacts `great-publishers` produces. Every artifact a skill writes follows one of these contracts so downstream tooling (book-site builder, trailer pipeline, future cross-promote skill) can rely on the structure.

## Cover concept brief

**Owner:** `chip-kidd-designer` via `/publishers-channel chip-kidd`
**Path:** `publishers/covers/<slug>.md`

```markdown
---
title: <Book title>
slug: <slug>
designer: chip-kidd
created: YYYY-MM-DD
---

# Cover Concept: <Title>

## What the book is about (one paragraph)
<The book's core argument, stated as Chip would state it.>

## The chosen concept (2-3 sentences)
<What the cover argues, not what it looks like.>

## Visual logic
- **Image:** <subject, framing, materials>
- **Type:** <typeface, weight, tracking, treatment>
- **Color:** <palette with reference colors>
- **Materials:** <paper, finish, binding, embossing>
- **Negative space:** <where the type lives within the image>

## Three rejected alternatives
1. <One sentence concept + one sentence reason for rejection>
2. ...
3. ...

## Position relative to comps
<2-3 comp covers and why this concept argues differently>
```

## Cover provocation brief

**Owner:** `george-lois-designer` via `/publishers-channel george-lois`
**Path:** `publishers/covers/<slug>-provocation.md`

```markdown
---
title: <Book title or magazine cover>
slug: <slug>
designer: george-lois
created: YYYY-MM-DD
---

# Cover Provocation: <Title>

## The argument the cover makes (one sentence)
<What the cover is asserting; the reader cannot remain neutral.>

## The headline
<The headline that lives inside the image. Three to seven words.>

## The image, staged
- **Subject:** <who/what is in it>
- **Setting:** <studio, location, available light>
- **Posture / gesture / metaphor:** <exactly>
- **Photographic register:** <documentary, deadpan, painterly, saturated>

## Type
<Typeface, weight, tracking, where it sits in the image>

## Defense
<One paragraph naming what each element is doing for the argument and why each substitution would weaken it. The defense is part of the brief.>
```

## Visual brief / opening spread

**Owner:** `diana-vreeland-editor` via `/publishers-channel diana-vreeland`
**Path:** `publishers/covers/<slug>-visual-brief.md`

```markdown
---
title: <Title>
slug: <slug>
director: diana-vreeland
created: YYYY-MM-DD
---

# Visual Brief: <Title>

## The visual argument (one paragraph)
<What the cover or opening spread is saying, in plain terms.>

## The image, specified
- **Frame:** <tight on the face, wide on the room, cropped detail>
- **Subject:** <person, object, arrangement, tableau>
- **Posture/gesture:** <exactly>
- **Palette:** <named colors with references>
- **Photographic register:** <flash, daylight, soft B&W, saturated, painterly>
- **Negative space:** <where the type lives within the image>

## Supporting visual rhythm (3-5 frames after the cover)
1. <Frame description, palette, role in the rhythm>
2. ...

## What the cover is NOT
<The obvious move, the literal interpretation, the category cliché. What the cover is arguing against.>
```

## Jacket copy / cover line

**Owner:** `tina-brown-editor` via `/publishers-channel tina-brown`
**Path:** `publishers/jacket-copy/<slug>.md`

```markdown
---
title: <Title>
slug: <slug>
editor: tina-brown
created: YYYY-MM-DD
---

# Jacket Copy: <Title>

## Audience (one specific person)
<Not a demographic; a person. Who they are, what they read, why they pick this up.>

## Cover line / subtitle (8-15 words)
<The promise the title turns into.>

## Back-cover blurb (150-200 words)
<The blurb that promises what the book delivers and only what it delivers.>

## What this promises that the book delivers
- <Promise>
- <Promise>

## What this does NOT promise
<Things the genre or market would like the blurb to say but the book cannot keep.>
```

## Positioning doc

**Owner:** `tina-brown-editor` via `/publishers-channel tina-brown`
**Path:** `publishers/positioning/<slug>.md`

```markdown
---
title: <Title>
slug: <slug>
editor: tina-brown
created: YYYY-MM-DD
---

# Positioning: <Title>

## Audience (one specific person)
<Named in real terms, not demographic.>

## Angle (why this lands now)
<The bridge from the work to the cultural moment.>

## Comparable titles
- <Title> — <how this is like it>
- <Title> — <how this differs>

## The cultural moment
<What's coming, what's already visible, why the work meets it.>

## Risks
<What could undercut the position. Counter-positioning the work would have to overcome.>
```

## Threshold read letter

**Owner:** `maxwell-perkins-editor` via `/publishers-channel maxwell-perkins`
**Path:** `publishers/positioning/<slug>-threshold-read.md`

```markdown
---
title: <Title>
slug: <slug>
editor: maxwell-perkins
created: YYYY-MM-DD
---

# Threshold Read: <Title>

Dear <writer>,

<One paragraph naming what the manuscript is doing at its best, with specifics.>

<Three or four moments that gave you pause, framed as questions, not instructions. Page references where applicable.>

<Three or four moments that are working at the highest level — the passages the writer must protect through any revision.>

<The threshold question: is this the book you set out to write?>

— Max
```

## Editorial letter (long-form essay)

**Owner:** `bob-silvers-editor` via `/publishers-channel bob-silvers`
**Path:** `publishers/positioning/<slug>-editorial-letter.md`

```markdown
---
title: <Essay title>
slug: <slug>
editor: bob-silvers
created: YYYY-MM-DD
---

# Editorial Letter: <Title>

## The argument as I read it (one sentence)
<State the essay's argument back to the writer for confirmation.>

## Where density slackens
- Page <N>: <quoted phrase>. <One-sentence note on what's happening.>
- ...

## Citations to verify or add
- <Claim that needs sourcing>
- <Quotation to verify>

## Where the prose is doing more work than the argument
<The sentences that are good prose but are not yet doing the essay's work.>

— Bob
```

## Rollout plan

**Owner:** `jann-wenner-publisher` via `/publishers-channel jann-wenner`
**Path:** `publishers/positioning/<slug>-rollout.md`

```markdown
---
title: <Title>
slug: <slug>
publisher: jann-wenner
created: YYYY-MM-DD
---

# Rollout Plan: <Title>

## Editorial argument across the run (one paragraph)

## Schedule
| Issue / Installment | Date | Cover-or-body | Cultural moment | Supporting pieces |
|---|---|---|---|---|
| Opener: <piece> | <date> | Cover | <hook> | <list> |
| Midpoint: <piece> | <date> | Cover | <hook> | <list> |
| Closer: <piece> | <date> | Cover | <hook> | <list> |
| Body: <piece> | <date> | Body | — | — |

## Risks and open questions
<What could disrupt the rollout.>

## The defining argument
<If we publish this run in this order with these covers, what cultural argument will we have made by the end?>
```

## List strategy

**Owner:** `bennett-cerf-publisher` via `/publishers-channel bennett-cerf`
**Path:** `publishers/positioning/<slug>-list-strategy.md`

```markdown
---
title: <Imprint or project>
slug: <slug>
publisher: bennett-cerf
created: YYYY-MM-DD
---

# List Strategy: <Imprint>

## The argument the imprint is making (one paragraph)

## Publication order
| # | Title | Why it leads/follows |
|---|---|---|
| 1 | <Title> | <Why this title establishes the imprint> |
| 2 | <Title> | <How it compounds the lead's argument> |
| 3 | <Title> | <Either becomes itself or thins out> |

## Backlist plan
- **Front-list-only:** <titles>
- **Backlist-foundational:** <titles>

## Acquisition profile (next two years)
- **Acquire:** <kinds of books that advance the list>
- **Refuse:** <kinds of books that would dilute>

## Risks
<Where the list might thin out; where a single misstep would compromise the imprint.>
```

## Trailer concept

**Owner:** any director persona from great-filmmakers via cross-plugin dispatch
**Path:** `publishers/trailer/<slug>.md`

This artifact is generated in `great-filmmakers` (e.g., by Hitchcock-persona) and copied or referenced from publishers. The publication-form decisions (engine choice, content-policy compromises, embed location in book site) are documented here.

```markdown
---
title: <Title>
slug: <slug>
director: <hitchcock | scorsese | etc.>
engine: <kling | veo | mixed>
created: YYYY-MM-DD
---

# Trailer Concept: <Title>

## Production doc
<Path to the .veo3.md or .kling.md production doc this trailer will render from.>

## Engine choice rationale
<Why Kling vs Veo. Content-policy implications. Cost.>

## Embed plan
<Where this trailer lands publicly. Book site? Social rollout? Both?>

## Render output
<Will land at: film/render/<slug>-trailer.mp4>
```

## Backend awareness for the book-site build

`/publishers-build-book-site` is **Astro**-only in v0.1 / v0.2. The argument for Astro: typography-first defaults, MDX integration that lets chapter content live as Markdown with embedded components, IntersectionObserver-friendly component patterns, GitHub Pages deploy via Actions.

Filed for v1.x:

| Backend | Status | Why it might land |
|---|---|---|
| Hugo | Stub | Faster build for very long manuscripts; some users prefer Go templating |
| Eleventy | Stub | Lighter dependency footprint; more flexible templating for unusual page layouts |
| Next.js | Not currently planned | Heavier than the book-site use case warrants |

When a non-Astro backend lands, the skill takes `--backend <name>` and copies from `templates/<backend>-book-site/`. The artifact paths in `publishers/` stay the same; the build target changes.
