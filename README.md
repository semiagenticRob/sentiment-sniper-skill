# Sentiment Sniper

**A Claude skill for precise emotional sentiment analysis and emotionally aligned writing, built on an extended Feeling Wheel and a companion index of ~1,900 English idioms.**

A [100 Reps](https://github.com/semiagenticRob) project rep.

---

## What it does

Sentiment Sniper packages the **`emotional-alignment`** skill: a model-agnostic
toolkit that lets an AI assistant do two things with unusual precision —

- **Assess** — read any text and say what it *feels like*: which emotions it
  conveys, how strongly, and how the feeling shifts from start to finish.
- **Produce** — take a target feeling (stated or implied) and return words,
  idioms, or a full rewrite that land on exactly that emotional note.

The trick that makes it more than free-association is a shared spine. Both of the
skill's reference documents classify emotion using the **same path notation**:

```
Core → Secondary → Tertiary
```

Because a single-word lookup and an idiom lookup resolve to the *same* path, you
can move between them losslessly — get the precise diction from the wheel, get
the idiomatic phrasing from the index, and know both are consistent with the same
underlying feeling.

---

## Repository structure

```
sentiment-sniper/
├── README.md
├── references/
│   └── feeling-wheel-disaggregated-extended.md   # standalone copy of the wheel
└── skills/
    └── emotional-alignment/                       # the installable skill
        ├── SKILL.md                               # skill instructions + frontmatter
        └── references/
            ├── feeling-wheel.md                   # 4-ring taxonomy, 500 terms
            └── phrase-sentiment-index.md          # 1,871 idioms on the same paths
```

The installable unit is **`skills/emotional-alignment/`**. The top-level
`references/feeling-wheel-disaggregated-extended.md` is a standalone copy of the
wheel for readers who just want the taxonomy without the skill wrapper.

---

## The two references

### 1. The Feeling Wheel (`feeling-wheel.md`)

A strict four-ring hierarchy, read from the inside out — each ring is reachable
only through its parent on the ring inside it:

| Ring | Count | Example |
|---|---|---|
| **Core emotion** | 7 | Happy, Surprised, Bad, Fearful, Angry, Disgusted, Sad |
| **Secondary feeling** | 41 | Happy → **Optimistic** |
| **Tertiary feeling** | 82 | Happy → Optimistic → **Hopeful** |
| **Bounded synonyms** (quaternary) | 500 | …→ Hopeful → *encouraged, expectant, heartened, uplifted, buoyant* |

A **bounded synonym** is a synonym filtered against the *entire path* above it.
Synonyms of *Aroused* under Happy → Playful exclude clinical or negative senses
("agitated," "provoked") — leaving only language consistent with the full path.
Because bounding is path-dependent, a word that appears on two paths gets two
different synonym sets: *Overwhelmed* under Bad → Stressed yields "swamped,
snowed under, maxed out," while *Overwhelmed* under Fearful → Anxious yields
"flooded, panicky, drowning in worry."

The wheel is bidirectional: **Sections 1–7** are forward production tables (one
per core emotion), and **Section 8** is an alphabetized Reverse Lookup Index —
any term → its full path.

### 2. The Phrase Sentiment Index (`phrase-sentiment-index.md`)

**1,871 English idioms, proverbs, and sayings** classified onto the wheel's exact
paths. An A–Z master table drives identification; a "Production Direction"
section groups phrases by core emotion for writing.

Each entry carries:

- **Sentiment path(s)** — the first is the predominant modern usage.
- **Depth** — the deepest ring claimable with confidence (Tertiary → Secondary →
  Core → Neutral). Shallow depth signals breadth, not low quality.
- **Usage tags** — italic labels (*ironic:*, *subject:*, *recipient:*,
  *negated:*…) mark readings that only apply in a given perspective or register.
  "A knight in shining armour" is Thankful from a grateful recipient but Skeptical
  when ironic. **Connotation governs, never surface denotation.**

---

## How it works — the two modes

The skill picks a mode per request; the two can chain (assess a draft, then
produce a rewrite).

### Mode A — Assess (text → feeling)

1. Harvest every emotionally loaded signal — feeling words, idioms, evaluative
   phrasing.
2. Resolve each to a path (wheel Section 8 for words, index letter tables for
   idioms; mark judgment calls `(inferred)`).
3. Disambiguate deliberately — watch for ⚠ duplicate terms and usage tags.
4. Aggregate into a profile: dominant vs. competing feelings, and trajectory.
5. Report with a signal-by-signal table and a confidence-rated verdict.

> **Example.** *"I've been snowed under all quarter and honestly it was the last
> straw when the deadline moved again."*
> "snowed under" → Bad → Stressed → **Overwhelmed**; "the last straw" → Angry →
> Frustrated → **Infuriated**. Profile: dominant **Bad** (overload) escalating
> into **Angry**.

### Mode B — Produce (feeling → text)

1. Fix the target path (resolve a precise feeling, or infer one from a described
   situation and state the inference).
2. Gather aligned language from both references — diction from the wheel, idioms
   from the index (verifying each idiom's full path *and* register).
3. Match register — earnest, ironic, comic, formal, casual.
4. Deliver in the shape requested — a suggestion list (each item traceable to its
   path, plus an **Avoid / adjacent-but-wrong** line) or a natural rewrite.

> **Example.** *"Rewrite my team update so it lands hopeful instead of flat."*
> Target: Happy → Optimistic → **Hopeful**. Diction: encouraged, heartened,
> uplifted, buoyant. Idioms (verified): "a foot in the door," "light at the end
> of the tunnel." Weave in two, then list the path used.

---

## Using the skill

The skill is model-agnostic and self-contained — it needs only the ability to
read its two bundled markdown references. No code execution, no network access,
no vendor-specific tools.

**With Claude Code / a skills-aware harness:** place the
`skills/emotional-alignment/` directory where your harness discovers skills
(e.g. `~/.claude/skills/` or a project `.claude/skills/`), or install the
packaged `emotional-alignment.skill` bundle. The skill triggers automatically on
requests about tone, mood, vibe, sentiment, emotional register, or choosing the
right feeling-word — even when the user never says "sentiment."

**As a plain reference:** open either markdown file directly. Both are designed
to be navigated by section (they're large — ~900 and ~2,100 lines — so search for
the exact term rather than reading end to end).

---

## Boundaries and care

- **Classify text, not people.** The skill analyzes what language conveys —
  "this text reads as…", never "this person is…". It is not for diagnosing
  anyone's mental state.
- **Judgments, not measurements.** Phrase classifications are inferential
  judgments about typical usage, not corpus frequency data. Assessments are
  presented with matching humility, surfacing genuine ambiguity rather than
  manufacturing false precision.
- **Distress comes first.** If someone appears to be in distress rather than
  analyzing text, the skill sets the taxonomy aside and responds to the person.

---

## Attribution

The phrase inventory draws on the master list compiled by **Phrase Finder**
([phrases.org.uk](https://www.phrases.org.uk), "Over 2,100 English Phrases And
Sayings"), accessed July 2026 — phrase names only. This index is unaffiliated
with Phrase Finder and reproduces none of its explanatory content. All sentiment
classifications are original to this project, produced as inferential judgments
about typical usage.
