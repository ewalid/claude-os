---
name: storyblok-content
description: >
  Trigger: "set up the Storyblok space for [account]", "create these
  stories/components in [space]". Writes real content into a
  Storyblok space: dry-run preview → explicit OK → write → verify.
  NOT runnable yet — no execution path is wired up in this session
  (see "Execution paths" below). Mechanism only until one is chosen
  and connected.
---

# storyblok-content

## Status (2026-07-21)

Blocked on an execution path, not on the design. There's no Storyblok
MCP connector in this Cowork session's tool list (checked directly,
not assumed). Walid gave two options; there's actually a third worth
weighing too:

## Execution paths (pick one to unblock this)

1. **Storyblok MCP connector**, if/when one gets added to this Cowork
   session. Cleanest long-term path — same guardrail-3 dry-run/confirm
   pattern as Notion, no separate tooling to maintain.
2. **A Claude Code prompt** — Walid runs this skill's logic through
   Claude Code instead of Cowork, where Claude Code's own tool access
   could reach Storyblok. Works, but means this deliverable lives
   outside this repo's normal Cowork-driven flow.
3. **Direct Storyblok Management API via bash/curl, right here in
   Cowork** — worth considering before picking 1 or 2. Cowork's shell
   can already run authenticated HTTPS requests. Given a Storyblok
   **personal access token** (Settings → My account → Personal access
   tokens in the Storyblok UI), stored as an env var
   (`STORYBLOK_TOKEN`, never committed — CLAUDE.md guardrail 6), this
   skill could call `https://mapi.storyblok.com/v1/spaces/{space_id}/stories`
   directly for dry-run reads and writes, with zero new connector and
   zero dependency on a separate product. This is the fastest path if
   Walid has (or can generate) a token.

**Open question for Walid**: do you have a Storyblok personal access
token you're comfortable putting in an env var for option 3? If yes,
that's likely the quickest way to make this real without waiting on a
new MCP connector.

## What it will do, once unblocked

1. **Dry-run preview.** Given a target space and a set of
   stories/components to create or update (from a demo-setup script,
   an account brief, or a direct request), produce a full preview of
   exactly what would be written — story slugs, component structure,
   field values — without writing anything.
2. **Explicit confirmation.** Show the dry-run to Walid, wait for an
   explicit OK (CLAUDE.md guardrail 3). If the target is a production
   space (not a dev/test space), require a second, explicit
   confirmation naming the space — guardrail 3 is stricter there.
3. **Write.** Execute the writes (create/update stories, components,
   assets) via whichever execution path is active.
4. **Verify.** Re-fetch what was written and confirm it matches the
   dry-run exactly — same principle as this session's `update_artifact`
   → `verify_artifact` pattern. Report any mismatch rather than
   assuming success from a 200 response alone.
5. **Announce** what was written, in chat — never silent.

## Guardrails

- Never write to a Storyblok space without the dry-run → OK step,
  regardless of execution path (CLAUDE.md guardrail 3).
- Production spaces need a second, explicit confirmation naming the
  space — a dev/test space does not.
- Never put a Storyblok token in a committed file — env var only
  (guardrail 6). If option 3 is chosen, confirm the token is set as an
  env var before the first real run, not pasted into chat.
- This skill has no bearing on RFP/security-question answers
  (`rfp-answer`) — that's a knowledge-library lookup, not a content API
  call, even once this skill is live.
