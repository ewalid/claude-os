---
name: rfp-answer
description: >
  Trigger: "answer this RFP", "help with this RFP question", or
  "harvest this RFP" after a submission closes. Two directions:
  RETRIEVE (match incoming RFP questions against known answers, adapt,
  flag gaps) and HARVEST (fold new/validated answers back into the operator's
  own local library afterward). Primary knowledge source is a real,
  live, company-wide Notion database (see below) — not empty, not
  hypothetical. Product/feature facts for RFPs come from the product's
  official docs and the company knowledge base — never from a
  content-management API (a CMS content connector like the one
  `storyblok-content` uses is unrelated to RFP answers).
---

# rfp-answer

## Status (2026-07-21)

Mechanism drafted, and the primary source is real: there's a live
"RFP Answer Library (POC)" database in the company Notion (its
collection id is in local `memory.md`; ~19 Q&A rows as of 2026-07-21 —
checked directly, not assumed) that someone else on
the team already started. Every row's Status is currently "needs
review," not "approved" — treat accordingly (see "Trust levels"
below). The operator is also going to seed `resources/rfp-library/` with their
own validated RFP files once they have them (their first live RFP deals are
both live "In progress" right now) — that becomes a second,
higher-trust local source layered on top of the shared one.

## Where the answers come from, in trust order

1. **The operator's own local library** (`resources/rfp-library/answers/*.md`)
   — once seeded, this is the operator-validated and the highest-trust source.
   Currently empty.
2. **The product's official public docs** (the operator's product and its
   docs URL are recorded in `memory.md`) — the
   authoritative source for product/feature facts. Ranks above the
   library in #3 (confirmed by the operator, 2026-07-21): docs are official,
   the POC library is not.
3. **The shared company Notion library** ("RFP Answer Library (POC)",
   collection id in local `memory.md`) — **not an
   official resource.** A coworker put it together on their own
   initiative; it's real, usable content (19 rows, genuinely
   well-written) but every row's Status is "needs review," and the operator
   confirmed it has no official standing — if it conflicts with the
   official docs, the docs win. Use it, but always surface the Status
   alongside the answer and never present a "needs review" row as
   equivalent to an approved one. **Read-only** for now — this is
   someone else's resource; writing back to it (marking rows
   "approved," adding new ones) needs an explicit ask from the operator first,
   unlike their own Accounts DB (CLAUDE.md guardrail 4 is about the operator's
   own Notion content, not someone else's unofficial database).
4. **Other sanctioned research, when 1-3 don't cover it** (The operator
   explicitly authorized these, 2026-07-21):
   - **The rest of the company Notion**, starting from the RFP Answer
     Library page as an anchor and branching out via `notion-search`
     — other teams (product, security, RevOps) may have written
     official material that isn't in the docs or the POC library.
   - **Slack search** (not just #se-requests/#se-sgm) — past threads
     where a similar question was actually answered.
   - **Google Drive** — security whitepapers, SOC2/ISO reports,
     one-pagers.
   Anything sourced this way is labeled `[research: <source>, needs
   SME confirmation before submitting]` — never presented as
   equivalent to a validated or official-docs answer.
5. **Nothing else.** If none of the above cover a question, mark it
   `needs SME input` — never fabricated from general CMS-market
   knowledge. This mirrors CLAUDE.md guardrail 5 (never invent
   Salesforce/Gong content) applied to product/security facts.

**A CMS content connector is not relevant here.** A connector like
the one `storyblok-content` uses manages content *inside* a customer's
CMS space — stories, components. It has no bearing on "does the product
support SAML SSO" or "what's the uptime SLA." Those are answered from
the knowledge sources above, never from a content-management API.

## Trust levels — always shown in the draft answer

- `validated` — from the operator's own local library.
- `official-docs` — from the product's official docs. Authoritative; still worth
  a quick sanity check for anything contractual, but not a guess.
- `needs review (unofficial POC)` — from the coworker's shared Notion
  library. Real content, no official standing, status is literally
  "needs review" — flag this explicitly, don't smooth it over, and
  defer to official docs if the two ever disagree.
- `research: <source>` — from wider Notion/Slack/Drive, needs SME
  confirmation before it goes in a real submission.
- `needs SME input` — nothing found anywhere; a real gap, not a guess.

## Why the local library isn't a vector DB

The operator asked directly whether their own library should be a vector
database instead of plain markdown. Decision: no, for this scale —

- **Realistic corpus size.** Even harvested over years, one SE's
  validated answers is realistically dozens to a few hundred entries —
  small enough for category + keyword search with Claude reading full
  matched files for real semantic judgment. A vector index buys
  nothing extra at this size, and the shared Notion library above is
  already a real structured DB for the company-wide content anyway.
- **No persistent infra to run.** This repo's actual persistence is
  git + the filesystem. A markdown library is itself the durable
  store — no embeddings pipeline to keep alive or re-run after edits.
- **Auditability.** Every local answer is a readable file with
  `git log` history — needed for anything that ends up in a
  legally-reviewed RFP submission. A vector store's nearest-neighbor
  match is a worse fit for "show me exactly which answer this came
  from and who approved it."

If the local library ever grows into the thousands of entries, revisit
this — that's a real threshold, not a permanent ruling.

## Steps — RETRIEVE ("answer this RFP")

1. Take the incoming RFP question set (pasted text for now — Excel
   handling is a later addition once the base flow works).
2. For each question, check in trust order: The operator's local library →
   the product's official docs → the shared (unofficial) Notion library
   (query by keyword/category, read full candidate rows for relevance)
   → other sanctioned research → `needs SME input`. If official docs
   and the POC library disagree, go with the docs and note the
   discrepancy rather than silently picking one.
3. Adapt whatever's found to the specific prospect's context — don't
   paste verbatim if it doesn't actually fit.
4. Return a draft answer set with every answer tagged by its trust
   level (see above) so the operator knows exactly what's safe to submit as-is
   versus what needs a second look.

## Steps — producing an actual response document (not just chat text)

Once real answers exist (RETRIEVE done) and the operator wants an actual
file, not just a chat draft:

1. **Default to annotating the prospect's own document in place**, when
   they sent one (a Word/PDF/etc., as opposed to a spreadsheet grid they
   want filled a specific way): keep every original section, heading,
   table, and word of theirs untouched; insert the operator's answers as
   extra columns appended onto their own requirement tables, and as
   clearly-labeled response blocks right after their narrative sections.
   Never rebuild it as a separate from-scratch proposal unless asked —
   confirmed with the operator 2026-07-22 that the annotated-in-place
   version is what's actually wanted, not a restructure.
2. **Ask the operator first, before checking anything** (see CLAUDE.md
   guardrail 11): does this response need to actually follow the
   operator's brand guidelines throughout, or does it just need the
   prospect's own document format kept, with a co-branded logo at most?
   Most RFP responses are the second case (step 1 above) — don't spend
   tokens reading brand assets that turn out not to be needed. Only if
   the answer is "yes, follow them" or a logo is genuinely needed: real
   logo files and a local guideline-doc snapshot live in
   `resources/brand-guidelines/` if the operator has added them — but
   treat that local copy as possibly stale. If a `SOURCES.md`-style note
   in that folder points at a live source (typically a Notion page),
   check that directly rather than the snapshot, especially for fonts —
   Notion tends to hold the current type/token detail that a one-time
   PDF export won't get updated with. If the response document combines
   the operator's own logo with the prospect's (a cover page, for
   example), pull both logos from real sources (the prospect's from
   their own document, the operator's from `resources/brand-guidelines/`)
   and follow whatever co-branding rule the actual guidelines specify
   (safe area, separator style) — never invent a lockup.
3. Keep new table columns/response blocks visually distinct enough to
   spot (a labeled box, or matching the original document's own header
   color for a seamless look — ask the operator which they prefer,
   don't assume) but never touch the original content itself.
4. Every trust-level tag and internal note (open questions, NEEDS SME
   INPUT flags) stays in the draft and gets called out explicitly as
   "delete before sending" — this is a working draft, not the final
   submission.

## Steps — HARVEST ("harvest this RFP")

1. After a submission (or a won/lost outcome), take the final
   operator-approved answers.
2. New/improved answers go into the operator's own
   `resources/rfp-library/answers/` (by category), with `_index.md`
   updated. This is the operator's local harvesting — separate from the
   shared Notion library, which stays read-only unless they say
   otherwise.
3. If the deal's outcome is known, log why it was won/lost in
   `resources/rfp-library/answers/won-lost-notes.md` — gitignored
   (2026-07-21, guardrail 10), it will name real deals/customers, same
   class of data as `accounts/`.
4. Announce what was added/updated in chat.

## Guardrails

- Never answer a compliance/security-grade question from anything
  other than the sources listed above, in trust order — never from
  general training-data knowledge about CMS products.
- Never present a "needs review" or "research"-sourced answer as
  equivalent to a validated one — the trust-level tag is not optional.
- Never write to the shared company "RFP Answer Library (POC)" DB
  without the operator explicitly asking for that — it's read-only by default.
- `won-lost-notes.md` is customer data — local-only, gitignored, never
  pushed even though this repo is private (same rule as `accounts/`).
- the operator's local category answer files are reusable product-positioning
  content, not tied to one customer's identity — those stay tracked in
  git, unlike `won-lost-notes.md`.
