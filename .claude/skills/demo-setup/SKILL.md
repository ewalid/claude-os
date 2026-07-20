---
name: demo-setup
description: >
  Trigger: "set up the demo for [account]", "prep the Storyblok space
  for [X]". Configures a Storyblok demo space (content, components,
  sample data) ahead of a call. BLOCKED for live execution as of
  2026-07-20 — no Storyblok MCP tool is present in this Cowork session
  (re-verified, genuinely absent, not a permissions issue). Draft the
  checklist now regardless — don't wait on the connector to start
  planning what the space needs.
---

# demo-setup

## What it does

Prepares the actual Storyblok space a demo will run in — populating
content/components/sample data so the "Show" steps in `demo-script`
work live, without last-minute scrambling. Turns a deck's demo stations
into a concrete space-structure plan.

## Current status: blocked for live execution

No Storyblok MCP tool exists in this Cowork session (re-verified
2026-07-20 — same result as 2026-07-15). **This does not block drafting
the checklist** — read the deck's stations and the brief, plan the
space structure, and write the plan now. It blocks step 4 (actually
writing to a space), which stays a manual to-do for Walid until the
connector exists.

**If asked to execute before the connector exists**: say so plainly,
don't attempt to fake it via Storyblok's public web app through
browser tools or guess at an API shape — a demo space misconfigured by
a wrong guess is worse than no automation at all.

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

4. **Dry-run preview before any write** — once the Storyblok connector
   exists, produce a preview of exactly what will be created/changed
   (new stories, components, sample content, front-end wiring,
   permissions/roles) and show it to Walid before touching the space.
   This is CLAUDE.md guardrail 3 (never write to a Storyblok space
   without preview + OK), not optional for this skill.

5. **Get explicit confirmation before writing.** If the space is a
   **production** space rather than a sandbox/demo space, get a
   second, separate confirmation — never treat a first OK as covering
   both.

6. **Execute the write** (connector-live only) — create/update the
   stories, components, sample data, front-end wiring, and permissions
   the dry-run described. Nothing beyond what was previewed.

7. **Verify live** — check the space renders/behaves as expected for
   each demo station before calling it done (don't just trust the
   write succeeded; confirm the actual content is there and correct).
   Test the live-update loop for any architecture-focused station
   specifically — don't assume it works.

8. **Flag data caveats explicitly** in the plan and in what Walid says
   on the call — mock/fictional data, no live client API, no finalized
   client UX/UI — as checklist items to verbally caveat, not hide.

9. **Save** to `accounts/<customer>/demo-setup.md` (local-only, per
   guardrail 6). While blocked, this is the full checklist Walid runs
   by hand; once the connector is live, it becomes the plan step 4-7
   execute directly.

10. **Report back**: what was planned/created/changed, which space,
    and a direct link if the tool provides one. Flag anything that
    couldn't be set up cleanly rather than leaving it silently
    incomplete.

## Guardrails

- Never write to a Storyblok space without a preview + explicit OK
  (CLAUDE.md guardrail 3) — no exceptions, no matter how small the
  change.
- Production spaces always need a second, separate confirmation beyond
  the initial OK.
- Never improvise this skill's execution through browser automation or
  a guessed API call while the MCP connector is absent — say it's
  blocked, don't work around the absence. Drafting the plan is never
  blocked; only the write is.
- Never invent a space structure without checking whether the account
  has a multi-brand/multi-site Decision Criterion the structure itself
  needs to prove (step 2's confirmation gate exists for this).
- Never treat mock/fictional data as if it were live — always carried
  through as an explicit caveat, both here and in `demo-script`.
