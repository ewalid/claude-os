---
name: storyblok-content
description: >
  Trigger: "set up the Storyblok space for [account]", "create these
  stories/components in [space]". Writes real content into a
  Storyblok space: dry-run preview → explicit OK → write → verify.
  Checks for a live Storyblok MCP connector first (works today from
  Claude Code on Walid's own machine); falls back to the Management
  API token, which is blocked from inside a Cowork sandbox specifically
  by network egress — see "Status".
---

# storyblok-content

## Status (2026-07-22)

Two live paths now, checked in this order every time — never assume
which one is available, this varies by *where* Darwin is running,
not by session:

1. **Storyblok MCP connector.** Walid confirmed Claude Code (run
   directly on his machine, outside Cowork) has a working Storyblok
   MCP connection. If running there, check for it first — discover
   the actual tool names at runtime (same rule as Gong in CLAUDE.md
   guardrail 5, never assume a name). If found, use it directly for
   all read/write calls below; no network-egress concern applies,
   since the call isn't going through Cowork's sandbox proxy at all.
2. **Management API token (fallback).** Walid provided a Storyblok
   Management API personal access token. Stored at `storyblok.env`
   in the repo root (gitignored via the existing `*.env` pattern —
   confirmed with `git check-ignore -v` before writing the token in,
   per CLAUDE.md guardrail 10) as `STORYBLOK_MANAGEMENT_TOKEN`.
   **Never commit this file, never paste the token into chat or into
   any tracked file again.**

   Tested from *inside a Cowork sandbox*: `curl` to
   `https://mapi.storyblok.com/v1/spaces/` was blocked by the
   sandbox's own network proxy — `403 Forbidden`,
   `X-Proxy-Error: blocked-by-allowlist`. Not a token problem, not a
   missing-connector problem — Cowork sandboxes can only reach an
   admin-controlled domain allowlist, and `storyblok.com` isn't on
   it (as of 2026-07-21).

   This path works with no admin action needed as long as it's run
   somewhere with normal network access — Walid's own terminal, or a
   Claude Code session on his machine (the repo lives at
   `~/dev/darwin`, and `storyblok.env` already exists there with the
   token in it). It would also work directly inside Cowork if a
   Team/Enterprise org Owner adds `storyblok.com` /
   `mapi.storyblok.com` to Cowork's network allowlist (Admin settings
   → Capabilities) — not done as of this writing, not blocking since
   path 1 exists now.

Net effect: this skill is completable *today* from Claude Code (path
1, or path 2 with normal network access) even though a Cowork
session on its own is still blocked on path 2 specifically.

## What it will do

1. **Check for the MCP connector** (see Status, path 1). If present,
   all steps below go through it. If absent, fall back to path 2
   (Management API + token).
2. **Dry-run preview.** Given a target space and a set of
   stories/components to create or update (from a demo-setup script,
   an account brief, or a direct request), produce a full preview of
   exactly what would be written — story slugs, component structure,
   field values — without writing anything.
3. **Explicit confirmation.** Show the dry-run to Walid, wait for an
   explicit OK (CLAUDE.md guardrail 3). If the target is a production
   space (not a dev/test space), require a second, explicit
   confirmation naming the space — guardrail 3 is stricter there.
4. **Write.** Execute the writes (create/update stories, components,
   assets) via the MCP connector's tools if available, otherwise via
   `https://mapi.storyblok.com/v1/spaces/{space_id}/stories` and
   related endpoints, authenticated with `STORYBLOK_MANAGEMENT_TOKEN`
   from `storyblok.env`.
5. **Verify.** Re-fetch what was written and confirm it matches the
   dry-run exactly — same principle as the deals-dashboard's
   `update_artifact` → `verify_artifact` pattern. Report any mismatch
   rather than assuming success from a 200 response alone.
6. **Announce** what was written, in chat — never silent.

## Guardrails

- Never write to a Storyblok space without the dry-run → OK step,
  regardless of which path (MCP or API) or which environment
  (Cowork or Claude Code) it's run from (CLAUDE.md guardrail 3).
- Production spaces need a second, explicit confirmation naming the
  space — a dev/test space does not.
- The token lives only in `storyblok.env` (gitignored). Never commit
  it, never echo it into chat, never write it into any other file.
- This skill has no bearing on RFP/security-question answers
  (`rfp-answer`) — that's a knowledge-library lookup, not a content
  API call, even once this skill is live.
