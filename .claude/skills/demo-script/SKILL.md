---
name: demo-script
description: >
  Trigger: "script the [account] demo", "write a demo script for [X]".
  Produces the talk track Walid actually speaks during a demo — chains
  off `accounts/<customer>/brief.md` and the deck built by `build-deck`.
---

# demo-script

## What it does

The deck (`build-deck`) has blank "Demo" slides by design — this skill
writes what Walid actually says and does at each one: Tell-Show-Tell
per station, anticipated objections pulled from `resources/battle-cards/`,
and a pre-call verify checklist. Output lives in
`accounts/<customer>/demo-script-<date>.md`, not on the slides themselves.

## Prerequisites

- `accounts/<customer>/brief.md` (from `process-customer`) — the
  script is critiqued against this account's actual priorities and
  MEDDPICC gaps, not generic Storyblok talking points.
- Ideally the deck itself (from `build-deck`), so the script's demo
  stations match the deck's stations exactly — if no deck exists yet,
  ask whether to build one first or script against a station list Walid
  gives directly.

## Steps

1. **Read the brief and the deck's demo stations** (or ask Walid for
   the station list if no deck exists yet).

2. **For each demo station, write Tell-Show-Tell:**
   - **Tell (opening)**: 2-3 sentences setting up the problem/context —
     tied to something specific from the brief (a stated pain point, a
     constraint from discovery), not a generic Storyblok pitch.
   - **Show**: the concrete sequence of actions Walid will actually
     perform in Storyblok — numbered steps, not vague ("open the Visual
     Editor, drag in a Teaser component, publish").
   - **Tell (close)**: the "so what" — the specific business outcome
     this ties back to for this account (their KPI/pain, not a generic
     benefit).

3. **Anticipated objections** — check `resources/battle-cards/` for
   this account's known competitors/objection patterns. If
   battle-cards is empty or has nothing relevant, say so rather than
   inventing objections; ask Walid what he's actually heard from this
   account so far (Salesforce/Gong context he'd need to paste in
   manually — never invented).

4. **Verify-before-call checklist** — concrete things to check live
   before the call, not assumed: demo environment data is current,
   any account-specific config still working, whether stakeholders
   confirmed for the call match who the script assumes is in the room.

5. **Save** to `accounts/<customer>/demo-script-<date>.md`:
   ```
   # <Account> — Demo Script — <date>
   Deck: <link/filename from build-deck, if any>

   ## Station 1: <name>
   Tell (open): ...
   Show: 1. ... 2. ... 3. ...
   Tell (close): ...

   ## Anticipated objections
   - <objection> — <response>, source: battle-cards/<file> or "Walid,
     <date>" if given directly, or "need validation" if neither.

   ## Verify before the call
   - [ ] ...
   ```

6. **Suggest the logical next step**: run through it once with Walid
   before the call, or flag if `resources/battle-cards/` is thin and
   could use populating.

## Guardrails

- Never invent objections or competitor claims not sourced from
  battle-cards or something Walid directly told you — label the
  source on every objection, or mark "need validation."
- Never fabricate Salesforce/Gong-derived context (CLAUDE.md guardrail
  5) — if Walid hasn't pasted it in, don't assume it.
- This is customer-facing prep material, not a deliverable sent to the
  customer — but should still follow style-guide.md tone since it may
  shape what Walid actually says live.
