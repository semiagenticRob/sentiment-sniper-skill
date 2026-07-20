---
name: emotional-alignment
description: >-
  Assess the emotional sentiment of any piece of text, or produce emotionally
  aligned wording, using a 500-term extended Feeling Wheel and a 1,871-phrase
  idiom sentiment index that share one path notation. Use this skill whenever a
  user asks what a text "feels like," what emotion a message conveys, how a
  reader might react to a draft, to run sentiment or tone analysis, to name a
  feeling more precisely ("I feel swamped — what is that really?"), OR to write,
  rewrite, or suggest wording with a specific emotional tone (e.g. "make this
  sound hopeful," "give me words for betrayal-flavored anger," "find an idiom
  that conveys quiet contempt"). Trigger even when the user never says
  "sentiment" — requests about tone, mood, vibe, emotional register, empathy in
  writing, or choosing the right feeling-word all belong here.
compatibility: >-
  Model-agnostic. Requires only the ability to read the two bundled markdown
  references (via file tools, grep/search, or by loading them into context
  whole). No code execution, no network access, no vendor-specific tools.
---

# Emotional Alignment

Assess the sentiment of text, or produce emotionally precise language, using two
bundled references that share a single spine: the path notation
`Core → Secondary → Tertiary`.

## Bundled references

| File | What it contains | When to open it |
|---|---|---|
| `references/feeling-wheel.md` | 4-ring taxonomy: 7 core emotions → 41 secondary → 82 tertiary → 500 path-bounded synonyms. Sections 1–7 are forward (production) tables per core emotion; Section 8 is an alphabetized Reverse Lookup Index (any term → full path). | Resolving a feeling word to its path; generating single-word/short-phrase diction for a target feeling. |
| `references/phrase-sentiment-index.md` | 1,871 English idioms/sayings classified onto the same paths. Alphabetical master table (Letter A–Z sections) for identification; "Production Direction" section groups phrases by core emotion. | Resolving an idiom to a feeling; finding idiomatic phrasing for a target feeling; checking register (ironic, comic, earnest, dated, offensive-history). |

The two files are joinable: any path found in one can be carried verbatim into
the other. That join is the core move of this skill — word-level diction from
the wheel, idiom-level phrasing from the index, both guaranteed consistent with
the same emotional path.

**Reading strategy.** The files are large (≈900 and ≈2,100 lines). Do not read
them end-to-end. Navigate by section: the wheel's Section 8 and the index's
letter tables are alphabetized, so search for the exact term. If your
environment cannot search within files, load only the section you need; if it
loads whole files into context, that also works — the instructions below are
unchanged.

## Mode selection

Decide which mode the request is, then follow that workflow. Requests can chain
(assess a draft, then produce a rewrite): run Mode A first and feed its paths
into Mode B.

- **Mode A — Assess:** the user supplies text and wants to know what it
  conveys or what the writer/subject feels.
- **Mode B — Produce:** the user names (or implies) a target feeling and wants
  words, phrases, idioms, or a rewrite aligned with it.

---

## Mode A — Sentiment assessment (text → feeling)

1. **Harvest emotional signals.** Read the text and list every emotionally
   loaded item: explicit feeling words ("furious," "swamped"), idioms and
   sayings ("last straw," "over the moon"), and evaluative phrasing whose
   connotation carries feeling even without a feeling-word.
2. **Resolve each signal to a path.**
   - Single words / short descriptors → wheel Section 8 (Reverse Lookup
     Index). Read off the full path; the bolded term marks the tertiary ring.
   - Idioms / sayings → phrase index letter tables. Take the *primary* path
     unless a usage tag fits the context better (see step 3).
   - Signals in neither reference → classify by judgment onto the nearest
     wheel path, and mark them `(inferred)` in your output so classifications
     from the references stay distinguishable from your own.
3. **Disambiguate deliberately.** Two traps to check every time:
   - **⚠ duplicate terms** (wheel): the term sits on multiple paths (e.g.
     *Overwhelmed* is Bad → Stressed workload-overload OR Fearful → Anxious
     panic-flooding). Choose by asking which *parent feeling* fits the
     context; consult the Duplicate-term notes below Section 8 for the four
     major structural duplicates (Disappointed, Embarrassed, Inferior,
     Overwhelmed). If context cannot settle it, report both paths and say so.
   - **Usage tags** (index): a path prefixed with an italic label (*ironic:*,
     *subject:*, *recipient:*, *negated:*...) applies only in that perspective
     or register. "A knight in shining armour" is Thankful from a grateful
     recipient but Skeptical when ironic. Match the tag to the text's actual
     stance; connotation governs, never surface denotation.
4. **Aggregate into a profile.** Group the resolved paths by core emotion and
   weigh them: how much of the text each core covers, whether one path
   dominates or several compete, and whether the feeling shifts across the
   text (note trajectory, e.g. "opens Fearful → Anxious, resolves Happy →
   Optimistic → Hopeful").
5. **Report using this template** (scale it down for one-line inputs — a
   single sentence may need only the table and verdict):

```markdown
## Sentiment Assessment
**Overall:** <dominant core emotion(s)>, <one-sentence characterization>
**Confidence:** High / Medium / Low — <why, in one clause>

| Signal (quoted from text) | Path | Source |
|---|---|---|
| "..." | Core → Secondary → Tertiary | wheel / phrase-index / inferred |

**Reading:** <2–5 sentences: dominant vs. secondary feelings, trajectory,
any ambiguity (⚠ terms, register uncertainty) and how you resolved it.>
```

**Depth honesty.** The references themselves only commit to the deepest ring
they can defend (many phrases are Secondary- or Core-depth; some are Neutral).
Mirror that: never report a tertiary-precision claim for a signal the index
classifies at Secondary depth, and let Neutral phrases stay neutral rather than
forcing them onto a path.

---

## Mode B — Emotionally aligned production (feeling → text)

1. **Fix the target path.** If the user gives a precise feeling, resolve it via
   wheel Section 8 to a full path. If they give a broad one ("angry,"
   "positive"), open that core emotion's forward table (wheel Sections 1–7),
   pick the 1–3 tertiary paths that best match their described situation, and
   say which you chose and why — or ask, if the request is genuinely
   ambiguous between distant paths. If they describe a *situation* rather
   than a feeling ("words for being deceived by a friend"), infer the path
   (here: Angry → Let down → Betrayed) and state the inference.
2. **Gather aligned language from both references.**
   - **Diction:** the target tertiary's bounded-synonym set (wheel Sections
     1–7). Every term there is pre-filtered for consistency with the whole
     path, so any of them is safe for that feeling.
   - **Idioms:** the phrase index's Production Direction section under the
     target core emotion, then verify each candidate's full path *and
     register* in the master table before offering it — the production
     grouping keys off primary paths only, and a phrase listed under Happy
     may be gallows humor unsuited to a sincere register.
3. **Match register.** Ask (or infer from context) whether the output should
   be earnest, ironic, comic, formal, or casual. Prefer phrases whose
   *predominant* reading already matches the register over phrases that need
   their secondary tagged reading; flag anything the index notes as dated or
   carrying an offensive history, and omit it unless the user asks.
4. **Deliver in the shape requested.**
   - *Word/phrase suggestions:* present diction and idioms separately, each
     item traceable to its path, with 1-clause guidance where register
     matters.
   - *Rewrite/draft:* write the text using the gathered language naturally —
     do not stuff every synonym in; two or three well-placed aligned choices
     beat ten. After the text, list which paths you drew on.

Suggested template for suggestion-style requests:

```markdown
## Emotionally Aligned Language
**Target:** Core → Secondary → Tertiary — <one-line gloss of the shade>

**Diction (bounded synonyms):** word1, word2, word3, ...
**Idioms & phrases:** "phrase one" (register note if any); "phrase two"; ...
**Avoid / adjacent-but-wrong:** <terms or phrases that look close but sit on a
different path or wrong register, with the path they actually belong to>
```

The **Avoid** line is what makes this reference-driven rather than
free-association: e.g. for sincere betrayal-anger, "et tu, Brute" looks
on-path but its predominant register is mock-reproach — offer it only for
comic use.

---

## Worked examples

**Mode A.** Input: *"I've been snowed under all quarter and honestly it was the
last straw when the deadline moved again."*
"snowed under" → wheel Section 8 → Bad → Stressed → Overwhelmed (workload
sense, not the Fearful ⚠ twin); "the last straw" → phrase index Letter T →
Angry → Frustrated → Infuriated. Profile: dominant **Bad** (stress/overload)
escalating into **Angry**; report both paths with the escalation noted.

**Mode B.** Request: *"Rewrite my team update so it lands hopeful instead of
flat."* Target: Happy → Optimistic → Hopeful. Diction: encouraged, expectant,
heartened, uplifted, buoyant. Idiom candidates from Production/Happy, verified
in the master table: "a foot in the door," "a good beginning makes a good
ending," "light at the end of the tunnel." Rewrite weaving in two of these,
then list the path used.

## Boundaries and care

- **Classify text, not people.** This skill analyzes what language conveys. Do
  not use it to diagnose anyone's mental state or psychoanalyze an author;
  frame findings as "this text reads as..." not "this person is...".
- **These are judgments, not measurements.** The phrase classifications are
  documented as inferential judgments about typical usage, not corpus data.
  Present assessments with matching humility, and surface genuine ambiguity
  instead of manufacturing false precision.
- **If a user seems to be in distress** (rather than analyzing text or
  drafting), set the taxonomy aside and respond to the person first,
  following your platform's guidance for supportive conversation.
