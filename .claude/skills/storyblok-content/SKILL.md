---
name: storyblok-content
description: >
  Trigger: "set up the Storyblok space for [account]", "create these
  stories/components in [space]". Writes real content into a
  Storyblok space: dry-run preview → explicit OK → write → verify.
  Token is in place (option 3 below); still blocked on network egress
  from this Cowork sandbox specifically — see "Status" for the actual
  unblock path.
---

# storyblok-content

## Status (2026-07-21)

Walid provided a Storyblok Management API personal access token.
Stored at `storyblok.env` in the repo root (gitignored via the
existing `*.env` pattern — confirmed with `git check-ignore -v` before
writing the token in, per CLAUDE.md guardrail 10) as
`STORYBLOK_MANAGEMENT_TOKEN`. **Never commit this file, never paste
the token into chat or into any tracked file again.**

Tested it immediately: `curl` from this Cowork sandbox to
`https://mapi.storyblok.com/v1/spaces/` was blocked by the sandbox's
own network proxy — `403 Forbidden`, `X-Proxy-Error: blocked-by-allowlist`.
This is NOT a token problem and NOT a missing-connector problem
anymore — it's specifically that this Cowork session's shell can only
reach an admin-controlled allowlist of domains, and `storyblok.com`
isn't on it.

Two real ways to unblock, in order of likely speed:

1. **Run it from Walid's own terminal instead of Cowork.** The
   workspace folder (`~/dev/claude-os`) persists on his real machine,
   so `storyblok.env` already exists there with the token in it. A
   Claude Code session (or plain `curl`) run directly on his machine
   has normal network access — no sandbox proxy in the way. This
   works today, no admin action needed.
2. **Ask a Team/Enterprise org Owner to add `storyblok.com` /
   `mapi.storyblok.com` to Cowork's network allowlist** (Admin
   settings → Capabilities). If that's done, this same token and this
   same skill work directly inside Cowork with no other change.

Still no Storyblok MCP connector anywhere in the registry (checked
2026-07-21) — not pursuing that path further; direct Management API
calls (once network access exists one of the two ways above) are
simpler anyway.

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
   assets) via `https://mapi.storyblok.com/v1/spaces/{space_id}/stories`
   and related endpoints, authenticated with `STORYBLOK_MANAGEMENT_TOKEN`
   from `storyblok.env`.
4. **Verify.** Re-fetch what was written and confirm it matches the
   dry-run exactly — same principle as this session's `update_artifact`
   → `verify_artifact` pattern. Report any mismatch rather than
   assuming success from a 200 response alone.
5. **Announce** what was written, in chat — never silent.

## Guardrails

- Never write to a Storyblok space without the dry-run → OK step,
  regardless of where it's run from (CLAUDE.md guardrail 3).
- Production spaces need a second, explicit confirmation naming the
  space — a dev/test space does not.
- The token lives only in `storyblok.env` (gitignored). Never commit
  it, never echo it into chat, never write it into any other file.
- This skill has no bearing on RFP/security-question answers
  (`rfp-answer`) — that's a knowledge-library lookup, not a content API
  call, even once this skill is live.
