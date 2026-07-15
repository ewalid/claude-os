---
name: process-customer
description: >
  Trigger: "let's process [account]", "process [account]", "brief me on
  [account]". Builds or refreshes the full account brief before a demo,
  call, or RFP push — including a MEDDPICC analysis derived from
  whatever Salesforce/Gong extracts Walid pastes in, written back to
  Notion. Has a hard stop in the middle — do not skip it.
---

# process-customer

## What it does

Produces `accounts/<customer>/brief.md`: the single document Walid should
be able to walk into a demo or call with, plus a MEDDPICC read on the
deal that gets written back to the Notion Accounts DB. Five phases, in order.

## Phase A — Gather everything

1. `memory.md` and any existing `accounts/<customer>/` files — don't
   re-derive what's already known.
2. Notion Accounts DB row for this customer + any linked notes. Read
   **Next call**, **Deadline**, **AE**, and **MEDDPICC** directly (the
   DB was restructured 2026-07-15 — these are real properties now, not
   inferred). "Due date (legacy)" may still be set on old rows; treat it
   as unverified, never as the source of truth.
3. Calendar — past and upcoming events with this customer/account.
4. Slack — search #se-requests, #se-sgm, and any other channel IDs
   Walid has shared, for threads mentioning this account.
5. Google Drive — decks, docs, RFP files tied to this account.
6. Notion "📞 Debriefs" database — any past debrief rows related to this
   account (relation property).

## Phase B — Ask Walid once, then HARD STOP

Before writing anything, post ONE consolidated message to Walid:
- Any Salesforce or Gong context he can paste in (never invent this —
  those tools aren't connected). Ask specifically for anything that
  speaks to MEDDPICC — who's been in the room, budget/timeline
  discussion, competitive mentions, stated pain — not just a general
  "anything to add?"
- Every inconsistency found in Phase A (e.g. Notion date vs. calendar
  mismatch, conflicting stage info, stale notes).

**Do not proceed to Phase C until Walid replies.** This is a hard stop,
not a suggestion — writing the brief (and the MEDDPICC analysis) on
guessed or unconfirmed info defeats the point of the skill.

## Phase C — MEDDPICC analysis

Once Walid's reply is in hand, work through all 8 elements (Metrics,
Economic Buyer, Decision Criteria, Decision Process, Paper Process,
Identify Pain, Champion, Competition) against every real input
available: the pasted Gong/Salesforce extracts, Slack threads, past
Debriefs DB rows, calendar context, and anything Walid said directly.
Never scan for signal in tools that aren't connected (Salesforce, Gong
themselves) — only in what Walid actually pasted or said.

For each of the 8 elements, assign one status:
- **Confirmed** — there's a specific, quotable piece of evidence (a
  line from the Gong/SF paste, a Slack message, a direct statement from
  Walid). Quote it.
- **Partial** — there's a signal but it doesn't fully establish the
  element (e.g. a stakeholder attended a call but their role/authority
  as economic buyer was never stated). Note what's missing to close it.
- **Gap** — no evidence either way. Say so plainly; don't guess.

Only elements marked **Confirmed** are candidates for the Notion
MEDDPICC multi-select — Partial and Gap never get checked, no matter
how plausible they seem.

**Regressions**: if this analysis would *remove* an element that was
previously marked confirmed in Notion (new information contradicts it,
or it was set without real evidence originally), flag this explicitly
to Walid before touching Notion — downgrading is a judgment call in a
way that adding new confirmed elements isn't.

## Phase D — Write the brief

`accounts/<customer>/brief.md`, structured as:

```
# <Customer> — Account Brief
Last updated: <date>

## Snapshot
Type / Stage / Priority / AE / next milestone — labeled explicitly as
CALL or DEADLINE (never just "due date"), pulled from the Next call /
Deadline properties directly.

## MEDDPICC
Full Phase C analysis — all 8 elements, each with its status (Confirmed
/ Partial / Gap), the quoted evidence or the specific gap, and the
source (Gong extract, Salesforce extract, Slack thread + link, or
"Walid, <date>"). Not just the ones going into Notion — show the full
picture including partials and gaps, since those are exactly what's
worth closing before the next call.

## Their priorities
What the customer actually cares about, per stakeholders and past calls.

## Stakeholders
Names, roles, notes on each (technical buyer, economic buyer, etc.)

## Technical context
Stack, integration points, anything demo/RFP-relevant.

## Risks
Open objections, competitive threats, anything that could stall the deal.

## Verify before next call
Checklist of things to confirm live — not assumed. Should include
closing the biggest MEDDPICC gaps/partials where possible.

## Need validation
Anything Phase A/B/C couldn't confirm. Never silently dropped.

## Sources
Where every above section came from (Notion link, Slack thread, calendar
event, Walid's own input, Gong/SF extract) — so staleness is auditable later.
```

## Phase E — Update Notion, propose fixes, close out

1. **Write the MEDDPICC result to Notion** — set the Accounts DB row's
   MEDDPICC multi-select to exactly the elements marked Confirmed in
   Phase C. This is a direct edit, done now, not gated on Walid's OK
   (per CLAUDE.md guardrail 4: edit Notion freely, announce at the
   end) — *except* any regression flagged in Phase C, which needs
   Walid's confirmation first.
2. Propose any other Notion fixes found (stale stage, ambiguous date,
   missing AE) — show the diff, execute only on Walid's OK. This part
   is unchanged: MEDDPICC is now automatic because it's a direct
   readout of evidence Walid himself supplied; stage/date/AE changes
   still involve judgment calls that deserve a check first.
3. Update `memory.md` with what changed this session (including the
   MEDDPICC read and what it's based on), and add an entry to
   `CHANGELOG.md` if anything in Notion or the repo structure changed.
4. Announce the Notion changes made (guardrail 4 — always announce,
   even when no OK was required).
5. Suggest the logical next step (e.g. "next: demo-script for the July
   21 call" or "the biggest MEDDPICC gap is Economic Buyer — worth
   confirming before the next call").

## Guardrails specific to this skill

- Never fabricate Salesforce/Gong content — if Walid doesn't provide
  it, the brief says "need validation" and the MEDDPICC element stays
  Gap, never guessed into Confirmed.
- Never mark a MEDDPICC element Confirmed without a specific, quotable
  piece of evidence and its source. "Seems likely" is Partial or Gap,
  not Confirmed.
- Never skip the Phase B hard stop, even if Phase A found nothing
  inconsistent — the point is giving Walid a chance to correct course
  before the brief (and the MEDDPICC analysis) is written, not just
  when something looks wrong.
- Downgrading a previously-confirmed MEDDPICC element always needs
  Walid's confirmation before it's written to Notion — never silently
  remove something a prior session or Walid himself had confirmed.
