---
name: rfp-answer
description: >
  Trigger: "answer this RFP", "help with this RFP question", or
  "harvest this RFP" after a submission closes. Two directions:
  RETRIEVE (match incoming RFP questions against the validated answer
  library, adapt, flag gaps) and HARVEST (fold new/validated answers
  back into the library afterward). NOT wired to Storyblok MCP — there
  is no such connector in this session, and product/feature facts for
  RFPs don't come from a CMS content API anyway. The knowledge source
  is the library itself: markdown files under resources/rfp-library/,
  not a vector database (see "Why not a vector DB" below).
---

# rfp-answer

## Status (2026-07-21)

Mechanism only. `resources/rfp-library/answers/` has zero real entries
— building the RETRIEVE logic against an empty library would just
produce "needs SME input" for everything, which isn't a genuinely good
skill (ROADMAP's own phase-gate rule: nothing built speculatively).
This file defines how it will work; it starts actually running once
Walid seeds real content (see "Seeding the library").

## Where the answers come from

Two, and only two, sources — never invented, never guessed:

1. **The validated library** (`resources/rfp-library/answers/*.md`,
   by category: security, architecture, integrations, editorial,
   pricing-licensing) — Walid-approved answers, harvested from real
   past RFPs. This is the primary source and the only one trusted
   for compliance/security-grade claims.
2. **Public Storyblok docs** (docs.storyblok.com, storyblok.com/security
   etc.) as a fallback for objective product facts ONLY when the
   library has nothing — always labeled `[public-docs, needs SME
   confirmation before submitting]` in the draft answer, never
   presented as equivalent to a validated library answer.

Anything neither source covers is flagged `needs SME input` — never
filled in with a best guess. This mirrors CLAUDE.md guardrail 5
(never invent Salesforce/Gong content) applied to product/security facts.

**The Storyblok MCP connector is not relevant here even if it existed.**
That connector (see `storyblok-content`) manages content *inside* a
customer's Storyblok space — stories, components. It has no bearing on
"does Storyblok support SAML SSO" or "what's the uptime SLA." Those are
answered from the knowledge library, not a content-management API.

## Why not a vector DB

Walid asked directly whether this should be a vector database instead
of a plain library. Decision: no, for this scale and environment —

- **Realistic corpus size.** One SE's validated RFP answers, even
  harvested over years, is realistically dozens to a few hundred
  entries — small enough that a flat, well-categorized set of markdown
  files (already scaffolded) is fully searchable by category + keyword
  grep, with Claude reading full matched files for actual semantic
  judgment (a vector index buys nothing extra at this size).
- **No persistent infra to run.** A real vector DB needs an embeddings
  pipeline and a store that survives across sessions; this repo's
  actual persistence is git + the filesystem. A markdown library is
  itself the durable store — no extra moving part to keep alive,
  re-embed after edits, or debug when it drifts from the source files.
- **Auditability.** Every answer is a readable file with a visible
  history (`git log`) and a clear category — a requirement for
  anything that ends up in a legally-reviewed RFP submission. A vector
  store's nearest-neighbor match is a worse fit for "show me exactly
  which answer this came from and who approved it."

If the library ever grows into the thousands of entries across many
SEs, revisit this — that's a real threshold, not a permanent ruling.

## Seeding the library (what Walid needs to provide)

Real RFP files/extracts (Cera RFP and Akeneo RFP are both live
"In progress" right now) — actual questions asked + the validated
answers Walid/the team gave. Paste them in, or point to an existing
team RFP answer bank if one already exists elsewhere (Confluence,
Drive, a shared Notion). First real run seeds the library for real
instead of building it empty.

## Steps — RETRIEVE ("answer this RFP")

1. Take the incoming RFP question set (Walid pastes it, or points to
   a file — Excel handling is a later addition once the base flow
   works with plain text/markdown input).
2. For each question, search `resources/rfp-library/answers/` by
   category + keyword. Read full candidate files, not just filenames
   — judge relevance semantically, not by exact string match.
3. If a validated answer matches: adapt it to the specific prospect's
   context (their stack, their stated requirements) — don't paste it
   verbatim if it doesn't actually fit.
4. If nothing matches: try the public-docs fallback for objective
   product facts only, clearly labeled as unvalidated.
5. If still nothing: mark `needs SME input` — never fabricate.
6. Return a draft answer set with every answer tagged by source
   (`library` / `public-docs, needs confirmation` / `needs SME input`)
   so Walid knows exactly what's safe to submit as-is.

## Steps — HARVEST ("harvest this RFP")

1. After a submission (or a won/lost outcome), take the final
   Walid-approved answers.
2. New or meaningfully-improved answers get added/updated under the
   right category file in `resources/rfp-library/answers/`, with
   `_index.md` updated to reflect what's now covered.
3. If the deal's outcome is known, log why it was won/lost — tied to
   which answers/positioning helped or hurt — in
   `resources/rfp-library/answers/won-lost-notes.md`. This file is
   gitignored (2026-07-21, guardrail 10): it will name real deals and
   customers, same class of data as `accounts/`.
4. Announce what was added/updated, same as any Notion/library write
   (CLAUDE.md guardrail 4 pattern extended to this library).

## Guardrails

- Never answer a compliance/security-grade RFP question from anything
  other than the validated library or a clearly-labeled public-docs
  fallback — never from general knowledge/training data about CMS
  products in general.
- Never invent that an answer is "validated" when it's actually a
  public-docs guess or a gap.
- `won-lost-notes.md` is customer data — local-only, gitignored, never
  pushed even though this repo is private (same rule as `accounts/`).
- Category answer files themselves (security/architecture/etc.) are
  reusable product-positioning content, not tied to one customer's
  identity — those stay tracked in git as the actual moat, unlike
  `won-lost-notes.md`.
