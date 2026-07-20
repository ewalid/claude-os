---
name: demo-setup
description: >
  Trigger: "set up the demo space for [account]", "draft the space
  setup for [X]". Produces the Storyblok space/component setup script
  needed to actually run a demo's stations — manual checklist if the
  Storyblok MCP connector isn't live, direct actions once it is.
---

# demo-setup

## What it does

Turns a deck's demo stations into a concrete space-structure checklist
(spaces/folders, components, front-end wiring, permissions, AI
features) so the live demo actually works when Walid runs it. Doesn't
require the Storyblok connector to exist yet — draft the checklist now,
automate it later once the connector is live.

## Prerequisites

- The account's deck (`build-deck` output) and `accounts/<customer>/brief.md`
  — the setup is driven by what the deck's stations actually need to
  show, not a generic space template.

## Steps

1. **Read the deck's stations and the brief's "Demo — what to show"
   section.** Every setup item should trace back to something a
   station actually needs to demonstrate — don't add generic Storyblok
   features that aren't part of this account's planned demo.

2. **Decide space structure** (one shared space vs. space-per-brand/
   entity, folder structure, shared vs. duplicated component library).
   **Flag this decision to Walid before finalizing** if the account has
   more than one brand/site involved — this is a structural call that
   affects how convincingly the demo proves a Decision Criterion (e.g.
   "single-environment multi-site management"), not just a formatting
   detail. (Learned 2026-07-20, Jouet club.)

3. **List components needed**, each tied to a specific station/pain
   point — fields, not just names, where that's known from the brief.

4. **Front-end wiring** — what needs to be connected (e.g. a decoupled
   React/Next.js front-end via the Content Delivery API) for the
   architecture-focused station to actually work live, plus a note to
   test the live-update loop before the call, not assume it works.

5. **Permissions/governance and AI features** — anything the demo needs
   to show on-screen to back up a talking point (e.g. a role/permission
   split for a security argument), not just discuss verbally.

6. **Flag data caveats explicitly** — mock/fictional data, no live
   client API, no finalized client UX/UI — as checklist items to
   verbally caveat during the demo, not to hide.

7. **Save** to `accounts/<customer>/demo-setup.md` (local-only, per
   guardrail 6 — never committed).

## Guardrails

- Never invent a space structure without checking whether the account
  has a multi-brand/multi-site Decision Criterion that the structure
  itself needs to prove (step 2's confirmation gate exists for this).
- Never treat mock/fictional data as if it were live — always carried
  through as an explicit caveat, both here and in `demo-script`.
