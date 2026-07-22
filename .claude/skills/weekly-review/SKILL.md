---
name: weekly-review
description: >
  Trigger: "weekly review", "how's the pipeline looking", or a natural
  Monday-morning cadence. A pipeline/account-health pulse across all of
  Walid's live accounts — distinct from `monthly-review`, which audits
  Darwin's own behavior/patterns, not the pipeline. Chat text output
  only, never an artifact (standing preference, 2026-07-20).
---

# weekly-review

## What it does

Looks across the whole Notion Accounts DB + Debriefs DB once a week
and answers: what moved, what stalled, what's coming up, what needs a
decision. `daily-briefing` is today's snapshot; this is the week's
shape — it should never just be seven daily-briefings stitched
together.

## Steps

1. **Read the Accounts DB** (all rows, not just today's) and the
   Debriefs DB. Compare against last week's state if `memory.md` has
   it noted; if not, just work from current Notion state and say so.

2. **Stage movement**: which accounts changed Stage since the last
   review (or since a rough week-ago baseline if no prior review
   exists). Forward movement and backward/stalled movement both matter
   — don't only report good news.

3. **Stalled accounts**: any High-priority account with no Notes
   update, no debrief, and no calendar activity in the last ~2 weeks.
   Flag it, don't guess why — that's a real gap Walid should notice,
   not something to explain away.

4. **This week ahead**: every Next call and Deadline landing in the
   next 7 days, grouped by account, demos first (same priority logic
   as `daily-briefing`).

5. **RFP status**: any RFP-type row with a deadline in the next 2
   weeks — flag explicitly, these tend to stack (2026-07-20: two RFP
   deadlines both landed on July 31 in the same week — exactly the kind
   of collision this section exists to catch early).

6. **Open MEDDPICC gaps on High-priority accounts**: not a full re-run
   of `process-customer`, just a one-line flag per account with a real
   gap (e.g. "no confirmed Economic Buyer") so it doesn't quietly sit
   unaddressed for weeks.

7. **Never modify anything** — this is a read-only pulse, same as
   `daily-briefing`. If something needs fixing (a stale Notion field,
   a debrief that never got logged), flag it and suggest the right
   skill (`post-call-update`, `process-customer`), don't fix it inline.

## Output format

Plain chat text, blank line between every section, no artifact.

📊 STAGE MOVEMENT
- [Account]: [old stage] → [new stage] (or "no change" only if notable,
  otherwise omit accounts with no movement to avoid padding)

🐌 STALLED
- [Account] — no update in [X] — HIGH priority, worth a nudge
  (or: "Nothing stalled.")

📅 THIS WEEK
- 🎯 [demo/call] — [account], [day/time]
- ⏰ [deadline] — [account], [day]

📋 RFP DEADLINES (next 2 weeks)
- [Account]: due [date] — [owner(s), status]
  (or: "None in the next two weeks.")

🎯 MEDDPICC GAPS (High priority only)
- [Account]: [the one real gap]
  (or: "No flagged gaps on High-priority accounts.")

One-liner: [the single thing most worth Walid's attention this week]

## Guardrails

- Read-only, like `daily-briefing` — never edits Notion.
- Never invent stage history if last week's state wasn't captured —
  say "no prior baseline, comparing isn't possible yet" rather than
  guessing what changed.
- Stay at the meta level for anything that would repeat real customer
  specifics beyond what's already safely summarized in Notion itself
  (CLAUDE.md guardrail 6).
