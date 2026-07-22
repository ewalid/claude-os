---
name: build-deck
description: >
  Trigger: "build a deck for [account]", "put together slides for [X]
  demo". Adapts one of the real example decks in
  `resources/deck-examples/` for a specific account — never creates
  slides from scratch. Needs `accounts/<customer>/brief.md` (from
  `process-customer`) to know what to customize.
---

# build-deck

## What it does

Produces a `.pptx` for an upcoming demo by adapting the closest-fitting
real deck already in `resources/deck-examples/`, using the account
brief to drive what changes. The template structure (documented in
`resources/style-guide.md`, confirmed against 5 real decks 2026-07-15)
is fixed — this skill fills it in, it doesn't invent new slide types.

## Prerequisites

- `accounts/<customer>/brief.md` must exist and be reasonably current
  (run `process-customer` first if not — don't build a deck off a
  guess about what the account cares about).
- At least one deck in `resources/deck-examples/` to adapt from. If
  none fit the account's profile well, say so rather than forcing a
  bad match.

## Steps

1. **Read the brief.** Snapshot, MEDDPICC gaps, their priorities,
   technical context, stakeholders — this is what decides which demo
   station themes to use and how technical/commercial the deck should be.

2. **Pick a base deck** from `resources/deck-examples/` — whichever
   existing deck is closest in size/sophistication/vertical to this
   account. Say which one and why (e.g. "similar to the mid-size,
   multi-market, editorial-autonomy pain point").

3. **Customize per the template** (see style-guide.md for the full
   structure):
   - Title slide: account name, the actual AE's name (from Notion AE
     property — never guess), Walid as the SE. Also swap the two
     headshot PICTURE shapes (easy to miss — they don't show up in a
     text-only shape scan): match each person by name against
     `resources/AEs & SEs/<Full Name>.jpeg`. If a headshot is missing
     for the AE or SE, flag it — never leave the source deck's original
     (wrong) person's photo in place unnoticed.
   - "What we know so far": Key Business Result / Observed constraints
     / What is needed / What does success look like / By when — pulled
     directly from the brief's "Their priorities" and MEDDPICC sections.
     If a row can't be filled from real discovery, mark it "need
     validation" on the slide's speaker notes — never invent a
     plausible-sounding constraint or metric.
   - Demo station themes: draft 2-4 candidates from the fixed
     vocabulary (style-guide.md step 3), tied to real evidence in the
     brief's priorities/technical context — not a default set, and
     never invented category names. **Confirm the shortlist with Walid
     before building anything** (same pattern as the Commercial-section
     gate below) — present the candidates and why each fits, get his
     pick back. Don't infer and proceed silently (learned 2026-07-20,
     the first real FR build: a first pass invented French station names instead of
     using the real theme vocabulary and skipped confirming entirely).
   - Demo slides themselves: leave BLANK ("Demo" / "CONFIDENTIAL" only)
     — these are live-demo placeholders, never scripted content. That's
     `demo-script`'s job, in a separate document, not on the slide.
   - Key takeaways slides: keep the structure (what we demonstrated /
     key benefits / one proof point) but pick the case study and
     benefit framing that's actually relevant to this account's stated
     priorities.
   - Technical Topics section: include only if the account is
     technical/enterprise enough to warrant it (see style-guide.md
     precedent — skip for smaller/less technical accounts).
   - Commercial section (Pricing/Partnership/Packages): include only
     for larger/enterprise deals, per style-guide.md precedent — ask
     Walid if it's unclear whether this deal warrants it.
   - Closing: "Thank You for Joining!" always last.

4. **Never copy leftover placeholder/instructional text** (e.g. "List
   here your talk track...") from a source deck — if a slide in the
   base deck still has that scaffolding, treat it as blank and fill it
   properly, don't paste the instructions into the new deck.

5. **Save** to `accounts/<customer>/<date>_Demo_<Account>.pptx`,
   matching the naming convention already used in
   `resources/deck-examples/` (e.g. `YYYY-MM-DD_Demo_<Account>.pptx`).

6. **Flag anything you couldn't confidently fill** — an unclear demo
   station theme, a discovery row with no real data, an ambiguous
   technical-vs-commercial call — rather than silently guessing.

7. **Suggest the logical next step**: `demo-script` to script the talk
   track around this deck's stations, or a verify pass with Walid
   before the call.

## Guardrails

- Never create a slide type that doesn't exist in the 5 source decks —
  if a new slide type is genuinely needed, flag it to Walid rather than
  inventing one.
- Never fill in demo slides with fake demo content — they stay blank.
- Never invent discovery data (constraints, metrics, quotes) to fill a
  "What we know so far" row — mark it need-validation instead.
- Decks are customer-facing — follow style-guide.md tone rules (direct,
  technical-credibility-first, customer's own language by default).
