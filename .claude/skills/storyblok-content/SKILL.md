---
name: storyblok-content
description: >
  Trigger: "set up the Storyblok space for [account]", "create these
  stories/components in [space]". Writes real content into a
  Storyblok space: dry-run preview → explicit OK → write → verify.
  Checks for a live Storyblok MCP connector first (works today from
  Claude Code on the operator's own machine); falls back to the Management
  API token, which is blocked from inside a Cowork sandbox specifically
  by network egress — see "Status".
---

# storyblok-content

## Scope — this is Darwin's one opt-in, product-specific skill

Darwin ships company- and product-agnostic (CLAUDE.md "Identity"). This
skill is the sanctioned exception: it's only relevant if the operator's
company uses **Storyblok** as its CMS (whether that's the product they
sell, or just the CMS they run). If the operator's product/stack has
nothing to do with Storyblok, this skill simply doesn't apply — it makes
no assumption that anyone selling anything uses it. Everything else in
Darwin stays product-neutral.

## Status (2026-07-22)

Two live paths now, checked in this order every time — never assume
which one is available, this varies by *where* Darwin is running,
not by session:

1. **Storyblok MCP connector.** the operator confirmed Claude Code (run
   directly on their machine, outside Cowork) has a working Storyblok
   MCP connection. If running there, check for it first — discover
   the actual tool names at runtime (same rule as Gong in CLAUDE.md
   guardrail 5, never assume a name). If found, use it directly for
   all read/write calls below; no network-egress concern applies,
   since the call isn't going through Cowork's sandbox proxy at all.
2. **Management API token (fallback).** the operator provided a Storyblok
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
   somewhere with normal network access — the operator's own terminal, or a
   Claude Code session on their machine (the repo lives at
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
3. **Explicit confirmation.** Show the dry-run to the operator, wait for an
   explicit OK (CLAUDE.md guardrail 3). If the target is a production
   space (not a dev/test space), require a second, explicit
   confirmation naming the space — guardrail 3 is stricter there.
4. **Re-fetch immediately before any update-in-place write.** For any
   `updateStory` / `updateComponent` call (or MCP equivalent), fetch
   that exact story/component fresh right before writing — never
   reuse content held from earlier in the session or conversation,
   even content fetched moments ago. These endpoints replace their
   target in full; stale content silently clobbers anything changed
   in between by a human (e.g. a manual Visual Editor edit made while
   Darwin was mid-task) or by another process. This applies per-write,
   not once per task — if a task does three sequential updates to the
   same story, re-fetch before each one, not just the first.
5. **Write.** Execute the writes (create/update stories, components,
   assets) via the MCP connector's tools if available, otherwise via
   `https://mapi.storyblok.com/v1/spaces/{space_id}/stories` and
   related endpoints, authenticated with `STORYBLOK_MANAGEMENT_TOKEN`
   from `storyblok.env`.
6. **Verify.** Re-fetch what was written and confirm it matches the
   dry-run exactly — same principle as the deals-dashboard's
   `update_artifact` → `verify_artifact` pattern. Report any mismatch
   rather than assuming success from a 200 response alone.
7. **Announce** what was written, in chat — never silent.

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
- **Never reuse held-in-context story/component content across
  multiple writes in the same task** — see step 4. A full-content
  overwrite built from anything but a fresh fetch is a real risk on
  every single write, not just the first one in a task.
- **When extending an existing component's schema to add a new
  field or feature, touch only the new field(s).** Never add
  restrictions or `conditional_settings` to a *pre-existing* field to
  support the new feature — put any new conditional-visibility logic
  on the new field(s) only, and leave every pre-existing field
  byte-for-byte as it was. Adding a `filetypes` restriction plus a
  `conditional_settings` hide-rule to an existing image field (to
  support a new "toggle between image and video" feature) once broke
  that field's focal-point/crop editor in the Visual Editor — caught
  only because the operator noticed the hero looked different and the focal
  point stopped saving. The fix was to restore the pre-existing field
  to its exact original definition and move the conditional logic
  onto the new field instead. Diff discipline on schema edits is not
  optional: a schema update should read as "N new fields added," never
  as changes to lines that were already there.
