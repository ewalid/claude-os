---
name: process-customer
description: >
  Trigger: "let's process [account]", "process [account]", "brief me on
  [account]". Builds or refreshes the full account brief before a demo,
  call, or RFP push. Has a hard stop in the middle — do not skip it.
---

# process-customer

## What it does

Produces `accounts/<customer>/brief.md`: the single document Walid should
be able to walk into a demo or call with. Four phases, in order.

## Phase A — Gather everything

1. `memory.md` and any existing `accounts/<customer>/` files — don't
   re-derive what's already known.
2. Notion Accounts DB row for this customer + any linked notes.
3. Calendar — past and upcoming events with this customer/account.
4. Slack — search #se-requests, #se-sgm, and any other channel IDs
   Walid has shared, for threads mentioning this account.
5. Google Drive — decks, docs, RFP files tied to this account.

## Phase B — Ask Walid once, then HARD STOP

Before writing anything, post ONE consolidated message to Walid:
- Any Salesforce or Gong context he can paste in (never invent this —
  those tools aren't connected).
- Every inconsistency found in Phase A (e.g. Notion date vs. calendar
  mismatch, conflicting stage info, stale notes).

**Do not proceed to Phase C until Walid replies.** This is a hard stop,
not a suggestion — writing the brief on guessed or unconfirmed info
defeats the point of the skill.

## Phase C — Write the brief

`accounts/<customer>/brief.md`, structured as:

```
# <Customer> — Account Brief
Last updated: <date>

## Snapshot
Type / Stage / Priority / AE / next milestone — labeled explicitly as
CALL or DEADLINE (never just "due date").

## Their priorities
What the customer actually cares about, per stakeholders and past calls.

## Stakeholders
Names, roles, notes on each (technical buyer, economic buyer, etc.)

## Technical context
Stack, integration points, anything demo/RFP-relevant.

## Risks
Open objections, competitive threats, anything that could stall the deal.

## Verify before next call
Checklist of things to confirm live — not assumed.

## Need validation
Anything Phase A/B couldn't confirm. Never silently dropped.

## Sources
Where every above section came from (Notion link, Slack thread, calendar
event, Walid's own input) — so staleness is auditable later.
```

## Phase D — Propose + close out

1. Propose any Notion fixes found (stale stage, ambiguous date, missing
   AE property) — show the diff, execute only on Walid's OK.
2. Update `memory.md` with what changed this session.
3. Suggest the logical next step (e.g. "next: demo-script for the July
   21 call").

## Guardrails specific to this skill

- Never fabricate Salesforce/Gong content — if Walid doesn't provide it,
  the brief says "need validation."
- Never skip the Phase B hard stop, even if Phase A found nothing
  inconsistent — the point is giving Walid a chance to correct course
  before the brief is written, not just when something looks wrong.
