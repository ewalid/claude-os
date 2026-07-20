---
name: demo-script
description: >
  Trigger: "write the demo script for [account]", "script the talk
  track for [X]'s deck". Writes the Tell-Show-Tell talk track for a
  deck already built by `build-deck` — never scripts content onto the
  deck's blank Demo slides themselves.
---

# demo-script

## What it does

Produces the internal talk track (Tell-Show-Tell per demo station) that
Walid uses to actually run the live demo behind a deck's blank "Demo"
slides. Always in English, regardless of the deck's language — this is
an internal working doc, not customer-facing (style-guide.md's FR/EN
rule: decks match the customer's language, internal notes/briefs stay
English).

## Prerequisites

- The account's deck must already exist (`build-deck` output) — the
  script is structured around that deck's actual stations and slide
  numbers, not invented independently.
- `accounts/<customer>/brief.md` for the real pain points/evidence each
  station should tie back to.

## Steps

1. **Read the deck and the brief.** Confirm the station names/order and
   slide numbers directly from the built deck — don't assume they match
   whatever the brief's "Demo — what to show" section implied, since
   station naming can change during `build-deck`.

2. **Draft the outline first**, before writing full prose: station
   order, and for each station, the one-line Tell/Show/Tell angle and
   which specific pain point or MEDDPICC element it's answering.

3. **Confirm the outline with Walid before writing the final doc.**
   Present the draft angles and ask for edits or an OK. This is a
   judgment call about framing and emphasis (what to lead with, what to
   downplay) — not a factual lookup, so it needs a real checkpoint, not
   an assumption. (Learned 2026-07-20, Jouet club: the first version was
   written end-to-end with no chance to adjust framing before it was
   finalized.)

4. **Write the full script** once confirmed: opening framing, then
   per-station Tell (opening) / Show (what to actually click through,
   live) / Tell (closing — tie back to the pain point), a closing
   section with direct questions to ask the room, and a "Don't" list for
   known traps (e.g. showing mock data without saying so).

5. **Flag open dependencies** — an unconfirmed date, an attendee list
   not yet locked, data caveats (mock/fictional data, no live API) —
   rather than writing around them silently.

6. **Save** to `accounts/<customer>/demo-script.md` (local-only, per
   guardrail 6 — never committed).

## Guardrails

- Never invent talking points not grounded in the brief's real
  evidence — if a station's benefit framing isn't backed by something
  actually in the brief, flag it as need-validation rather than
  inventing a plausible-sounding one.
- Always confirm the outline (step 3) before finalizing — this is the
  one skill in the demo-prep chain that's pure judgment/framing, not
  a factual writeup, so it gets a real checkpoint every time.
