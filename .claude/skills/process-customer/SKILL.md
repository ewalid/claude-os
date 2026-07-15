---
name: process-customer
description: >
  Trigger: "let's process [account]", "process [account]", "brief me on
  [account]". Builds or refreshes the full account brief before a demo,
  call, or RFP push — and rewrites the ENTIRE Notion Accounts DB row
  (Stage, Priority, Notes, AE, Next call, Deadline, MEDDPICC) from real
  Salesforce/Gong/Slack evidence. Walid's manual notes are not the
  source of truth — this skill's analysis is. Has a hard stop in the
  middle — do not skip it.
---

# process-customer

## What it does

Produces `accounts/<customer>/brief.md`: the single document Walid should
be able to walk into a demo or call with. It also rewrites the account's
entire Notion Accounts DB row from whatever real Gong/Salesforce/Slack
evidence is available — not just the MEDDPICC column, the whole row.
Five phases, in order.

## Phase A — Gather everything

1. `memory.md` and any existing `accounts/<customer>/` files — don't
   re-derive what's already known, but don't treat old notes as
   authoritative either; they get superseded by this pass's analysis.
2. Notion Accounts DB row for this customer + any linked notes — this is
   what's about to be rewritten, so read the current state fully first.
   "Due date (legacy)" may still be set on old rows; treat it as
   unverified, never as the source of truth, and never write to it (it's
   deprecated — **Next call** / **Deadline** are the real fields).
3. Calendar — past and upcoming events with this customer/account.
4. Slack — search #se-requests, #se-sgm, and any other channel IDs
   Walid has shared, for threads mentioning this account.
5. Google Drive — decks, docs, RFP files tied to this account.
6. Notion "📞 Debriefs" database — any past debrief rows related to this
   account (relation property).

## Phase B — Ask Walid once, then HARD STOP

Before writing anything, post ONE consolidated message to Walid:
- Any Salesforce or Gong context he can paste in (never invent this —
  those tools aren't connected). This is the real input this skill runs
  on — ask for whatever's available: call transcripts, opportunity
  notes, stage/close-date fields, stakeholder info, anything.
- Every inconsistency found in Phase A (e.g. Notion date vs. calendar
  mismatch, conflicting stage info, stale notes).

**Do not proceed to Phase C until Walid replies.** This is a hard stop,
not a suggestion — the whole point of this skill is analyzing real
evidence instead of stale manual notes, so it can't run on nothing.

## Phase C — Full account analysis

Once Walid's reply is in hand, extract everything relevant from the
real evidence (pasted Gong/Salesforce extracts, Slack threads, past
Debriefs DB rows, calendar context, anything Walid said directly).
Never invent signal from tools that aren't connected (Salesforce, Gong
themselves) — only from what was actually pasted or said. For each
Notion field below, either derive a real value with its evidence, or
mark it "no change / need validation" if the evidence doesn't support
one:

- **Stage** — does the evidence show real progression (e.g. "moved to
  proposal," "verbal commit," "lost to <competitor>")?
- **Priority** — does the evidence justify a change (deal size, urgency,
  exec sponsorship signal)?
- **AE** — does the evidence name the actual AE on this deal? Never
  guess from role plausibility.
- **Next call / Deadline** — the concrete next milestone with a real
  date, labeled explicitly as CALL or DEADLINE.
- **Notes** — a fresh, concise summary of where the deal actually
  stands, replacing the old Notes text (which may be stale/wrong —
  don't just append to it, rewrite it to reflect current reality).
- **MEDDPICC** — all 8 elements (Metrics, Economic Buyer, Decision
  Criteria, Decision Process, Paper Process, Identify Pain, Champion,
  Competition), each tagged:
  - **Confirmed** — specific quotable evidence + source.
  - **Partial** — a signal but incomplete; note what's missing.
  - **Gap** — no evidence either way.

## Phase D — Write the brief

`accounts/<customer>/brief.md`, structured as:

```
# <Customer> — Account Brief
Last updated: <date>

## Snapshot
Type / Stage / Priority / AE / next milestone — labeled explicitly as
CALL or DEADLINE (never just "due date"), reflecting what was just
written to Notion in Phase E, not the pre-analysis state.

## MEDDPICC
All 8 elements, each with its status (Confirmed / Partial / Gap), the
quoted evidence or the specific gap, and the source (Gong extract,
Salesforce extract, Slack thread + link, or "Walid, <date>"). Show the
full picture, not just what made it into Notion.

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

## Phase E — Rewrite the Notion row, close out

1. **Write every field derived in Phase C directly to the Notion
   Accounts DB row** — Stage, Priority, Notes, AE, Next call, Deadline,
   MEDDPICC. Per CLAUDE.md guardrail 4, this is a direct edit: no
   proposal step, no waiting for an OK, on any field, including
   downgrading a previously-set MEDDPICC element or overwriting stale
   Notes — the analysis in Phase C is the new source of truth. The two
   limits that still apply: never write a field Phase C couldn't
   actually derive from real evidence (leave it as-is / need validation
   rather than guess), and never touch a human-only column.
2. **Announce every field that changed** — old value → new value → the
   evidence it's based on, so Walid can spot-check anything that
   surprises him even though no approval gate blocked the write.
3. Update `memory.md` with what changed this session (including the
   full before/after on the Notion row and what it's based on), and add
   an entry to `CHANGELOG.md`.
4. Suggest the logical next step (e.g. "next: demo-script for the July
   21 call" or "the biggest MEDDPICC gap is Economic Buyer — worth
   confirming before the next call").

## Guardrails specific to this skill

- Never fabricate Salesforce/Gong content — if Walid doesn't provide
  it, the field is left as-is (or "need validation" in the brief), and
  the MEDDPICC element stays Gap. Not writing something is always
  safer than guessing it.
- Never mark a MEDDPICC element Confirmed, or change Stage/Priority/AE/
  Next call/Deadline, without a specific, traceable piece of evidence
  and its source in the brief's Sources section.
- Never skip the Phase B hard stop — the whole row rewrite depends on
  having real evidence in hand first; without it there's nothing to
  analyze.
- Rewriting the whole row means old manual Notes/Stage/Priority values
  are superseded, not preserved — that's the point (Walid's manual
  notes are treated as unreliable by design). Always show the old ->
  new diff when announcing so nothing changes invisibly.
