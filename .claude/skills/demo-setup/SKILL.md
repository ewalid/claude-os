---
name: demo-setup
description: >
  Trigger: "set up the demo for [account]", "prep the Storyblok space
  for [X]". Configures a Storyblok demo space (content, components,
  sample data) ahead of a call. BLOCKED as of 2026-07-15 — no
  Storyblok MCP tool is present in this Cowork session. Drafted so
  it's ready the moment that connector shows up; do not attempt to
  run this by improvising against a different tool.
---

# demo-setup

## What it does

Prepares the actual Storyblok space a demo will run in — populating
content/components/sample data so the "Show" steps in `demo-script`
work live, without last-minute scrambling.

## Current status: blocked

As of 2026-07-15, this Cowork session has no Storyblok MCP tool at
all (verified — not just unauthorized, genuinely absent from the tool
list). HANDOFF.md flagged this as "verify in Cowork"; verified answer
is "not currently there." This is tracked in `memory.md` and
`CHANGELOG.md`'s open items.

**If triggered before the connector exists**: say so plainly, don't
attempt to fake it via Storyblok's public web app through browser
tools or guess at an API shape — a demo space misconfigured by a wrong
guess is worse than no automation at all. Point Walid at doing it
manually for now, and flag that this skill is ready to go live the
moment the connector is confirmed reachable.

## Steps (once the Storyblok MCP connector is live)

1. **Read the account brief** (`accounts/<customer>/brief.md`) and the
   deck's demo stations (from `build-deck`, if built) — these define
   what content/components need to exist and in what state.

2. **Dry-run preview first, always** — before writing anything to a
   Storyblok space, produce a preview of exactly what will be
   created/changed (new stories, components, sample content) and show
   it to Walid.

3. **Get explicit confirmation before writing** (CLAUDE.md guardrail 3:
   preview + OK for any Storyblok space write). If the space is a
   **production** space rather than a sandbox/demo space, get a
   second, explicit confirmation — never treat a first OK as covering
   both.

4. **Execute the write** — create/update the stories, components, and
   sample data the dry-run described. Nothing beyond what was previewed.

5. **Verify live** — check the space renders/behaves as expected for
   each demo station before calling it done (don't just trust the
   write succeeded; confirm the actual content is there and correct).

6. **Report back**: what was created/changed, which space, and a
   direct link if the tool provides one. Flag anything that couldn't
   be set up cleanly (e.g. a component type that doesn't exist yet in
   that space) rather than leaving it silently incomplete.

## Guardrails

- Never write to a Storyblok space without a preview + explicit OK
  (CLAUDE.md guardrail 3) — no exceptions, no matter how small the change.
- Production spaces always need a second, separate confirmation beyond
  the initial OK.
- Never improvise this skill's function through browser automation or
  a guessed API call while the MCP connector is absent — say it's
  blocked, don't work around the absence.
