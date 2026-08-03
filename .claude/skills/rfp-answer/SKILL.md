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

1. **The operator's own local library** — the highest-trust source,
   and **no longer empty (seeded 2026-07-30)**. Two layers:
   - `resources/rfp-library/validated-submissions/` (gitignored) — real
     as-submitted documents. Read the most recent relevant one **before
     drafting anything**. Higher trust than official docs, because it
     carries both the facts and how the operator chooses to frame them
     commercially, already through review.
   - `resources/rfp-library/answers/<category>.md` (tracked) — reusable
     positioning harvested from those submissions, scrubbed of client
     specifics. Includes plan-gating tables and a "standing lessons"
     list in `_index.md` that exists because of real misses; read it.
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

0. **Two research passes, not one — run BOTH.** This is the single
   biggest lesson from the 2026-07 RFP that a colleague's assistant
   materially out-answered (see "Why that response was better" below).
   - **Requirement-driven** (the obvious pass): answer every numbered
     requirement in the document.
   - **Account-driven** (the pass that gets skipped, and where the deal
     is actually won): ask what makes *this* product uniquely right for
     *this* prospect — things no requirement line item will ever ask for.
     Non-negotiable checks:
     a. **Does our product natively integrate with the prospect's own
        product?** If the prospect is a software vendor, search the app
        directory, changelog and partner pages for *their company name*
        before anything else. On the 2026-07 RFP a shipped first-party
        plugin connecting the prospect's own platform to our editor
        existed, was publicly documented, and no competitor in the
        evaluation had an equivalent — the strongest differentiator
        available, and requirement-driven research walked straight past
        it because it wasn't a line item.
     b. Is the prospect an existing partner/customer/integration in any
        capacity already?
     c. Which case studies match this prospect's *profile* (industry,
        current CMS, locale count, team shape) rather than just being
        impressive?
     d. If the RFP states success metrics, tie capabilities back to each
        metric explicitly.
1. Take the incoming requirement set (from the document itself — see the
   document-production section below for keeping their format).
2. For each requirement, check in trust order: the operator's local
   library (validated submissions first, then category answers) → the
   product's official docs → the shared (unofficial) Notion library →
   other sanctioned research → `needs SME input`. If official docs and
   the POC library disagree, go with the docs and note the discrepancy
   rather than silently picking one.
3. **Always resolve the plan tier — read
   `resources/rfp-library/answers/pricing-licensing.md` before answering
   any capability question.** It holds the transcribed public plan matrix:
   real tier names, per-tier quotas, and the roughly one-third of the
   feature set that is Premium+/Elite-only. A capability answer without its
   plan gate is half an answer and creates commercial risk downstream.
   Also check the pricing page's **integration matrix** — it is the
   fastest single check for whether an integration with the prospect's own
   platform exists (see step 0a), and it reveals when a must-have
   integration sets a pricing floor. Re-fetch the live page if the local
   transcription looks stale; the fact-sheet filename is date-stamped.
4. **Before writing "Requires custom development" or calling something a
   gap, find the delivery route on the prospect's actual target stack.**
   A gap named without a route reads as a missing feature and loses points
   on the (usually heavily-weighted) platform-capabilities criterion. Real
   examples from the 2026-07 RFP where a hedged "gap" answer was corrected
   to a concrete path: forms → embed the prospect's existing MAP forms
   directly in the front end (zero CMS dependency) *or* model fields as
   components; non-developer redirects → CMS story as the data store *plus*
   the host's own dashboard-driven redirect config; canonical/hreflang →
   the framework's native metadata API, i.e. framework-native work rather
   than bespoke development. Be honest about what isn't native; be
   specific about how it actually gets delivered.
5. **Don't downgrade a feature's maturity off a changelog entry alone.**
   A "Labs"/changelog mention is a point-in-time signal. Check for a
   product landing page and current plan inclusion before hedging — one
   answer was written as "new, don't over-claim" when the feature was
   already GA and included on two plan tiers.
6. **Exhaust the sanctioned sources before writing `needs SME input`.**
   That tag is for genuine gaps, not for facts that are merely
   inconvenient to find. Company headcount, customer/enterprise counts,
   funding, certifications, support-tier response times, roadmap items and
   partnership credentials are all normally obtainable from public pages,
   the trust centre, the public roadmap, or sales-enablement material in
   Drive/Notion — trust-order step 4 exists precisely for this. On the
   2026-07 RFP, eight `needs SME input` flags were written where every
   single one was answerable; they are now recorded in
   `resources/rfp-library/answers/company-credentials.md`.
7. **Pull the live deal context, don't rely on the account brief.** Read
   the account's own Slack channel, recent email and any Gong calls for
   the *current* state before drafting. The 2026-07 RFP turned on facts
   that lived only there — the proposed implementation partner had
   dropped out, and the two teams had agreed to evaluate the platform
   first and select a partner afterwards. A draft written without that
   reads as evasive on the partner question; with it, it reads as aligned.
   The channel ID is in the account's local notes; guardrail 8 (Slack
   outranks Notion) applies.
8. Adapt whatever's found to the prospect's context — never paste verbatim
   if it doesn't actually fit.
9. Return a draft with every answer tagged by trust level so the operator
   knows what's safe to submit as-is versus what needs a second look.

## Why that response was better (2026-07, worth re-reading before each RFP)

A colleague's assistant produced the validated version of an RFP response
Darwin had drafted. It was ~39% longer and better on substance, not
formatting — the structure, co-branding and compliance work carried over
essentially unchanged. What it did that Darwin hadn't:

- Found the first-party integration with the prospect's own product.
- Answered every fact Darwin had punted to `needs SME input`.
- Resolved plan-tier gating on every capability.
- Converted "gap" answers into concrete delivery routes.
- Carried live deal context (partner situation) into the narrative.
- Added consultative judgment the RFP never asked for — e.g. flagging
  that the prospect's own timeline put migration too late and dry runs
  should start during the build phase. Reads as expertise, not scope creep.
- Answered "two contactable references" as a genuinely different ask from
  "three case studies", with a "why this maps to you" paragraph each.
- Closed with a useful-links appendix (docs, roadmap, changelog, FAQ,
  fact sheet, T&Cs) — cheap to add, gives the evaluator somewhere to go.
- Stripped all internal scaffolding (trust tags, draft notices) for the
  final version — those are Darwin's working aids, never the deliverable.

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
   operator-approved answers. **Also harvest whenever a validated or
   externally-proofed version comes back** — that version is now the
   highest-trust source and supersedes Darwin's draft, even mid-cycle.
2. Save the document itself to
   `resources/rfp-library/validated-submissions/` as
   `YYYY-MM-DD_<Account>-<Topic>_VALIDATED.<ext>` — **gitignored**, it
   names real clients/stakeholders/partners (guardrail 10).
3. Then extract the *reusable* positioning into
   `resources/rfp-library/answers/<category>.md`, scrubbed of every
   client/stakeholder/deal specific so those files stay safe to track in
   git (guardrail 6). Update `_index.md`. This is the compounding step —
   skipping it means the next RFP relearns everything.
4. **If the validated version differs from Darwin's draft, diff it and
   record why in the category file**, not just the corrected fact. "Plan
   tier was missing" / "a gap was really a delivery route" / "punted to
   needs-SME-input when it was public" are reusable lessons; the fact
   alone is not. `_index.md`'s "standing lessons" list is exactly this.
   Where the diff reveals a process gap rather than a content gap, that's
   a `darwin-improve` trigger — take it.
5. This is local harvesting — separate from the shared Notion library,
   which stays read-only unless the operator says otherwise.
6. If the deal's outcome is known, log why it was won/lost in
   `resources/rfp-library/answers/won-lost-notes.md` — gitignored
   (2026-07-21, guardrail 10), it will name real deals/customers, same
   class of data as `accounts/`.
7. Announce what was added/updated in chat.

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
