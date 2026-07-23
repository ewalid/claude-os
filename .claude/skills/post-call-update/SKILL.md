---
name: post-call-update
description: >
  Trigger: "debrief [account]", "debrief the [account] call", or right
  after the operator mentions a call just happened. Captures the outcome and
  propagates it everywhere it needs to live — Notion Debriefs DB,
  Accounts DB, the account's own file, and memory.md.
---

# post-call-update

## What it does

Closes the loop after a call: one debrief in, four places updated out.
Never invents outcomes — this skill is only as good as what the operator
actually tells it, or a Gong transcript (pulled directly if Gong MCP is
connected this session, otherwise pasted by the operator).

## Steps

1. **Get the debrief.**
   - **If a Gong MCP connector is available this session**, offer to
     pull the call transcript directly (find it by account/date,
     discover the actual Gong tool names at runtime — don't assume
     them). Summarize the outcome/objections/next-step from the
     transcript, then **still have the operator confirm** before writing
     anything — a misread transcript writing the wrong stage into
     Notion is a real cost, and Slack outranks Notion for deals but a
     transcript summary doesn't outrank the operator's own read of their call.
   - **Otherwise**: if the operator's trigger message already contains the
     outcome, objections, and next steps, use it. If it's thin
     ("debrief [account]"), ask: what happened, what was the outcome,
     what objections came up, what's the next step (and is it a CALL or
     a DEADLINE — label it explicitly, same rule as everywhere else in
     this repo). If they paste a Gong transcript manually, treat it the
     same as a Gong-pulled one: summarize, then confirm before writing.

2. **Write a new row to the Notion "Debriefs" database:**
   - Call: `<Account> — <date>`
   - Account: relation to the matching Accounts DB row
   - Call date: the actual call date
   - Outcome: one of Went well - next step booked / Mixed - follow-up
     needed / Stalled or at risk / Lost / Won
   - Objections, Next steps, Notes: from what the operator gave you

3. **Update the Accounts DB row** for that account:
   - Stage, if it changed.
   - Next call or Deadline — whichever the new next-step actually is
     (never write into "Due date (legacy)").
   - Notes — refresh if stale.
   - MEDDPICC — only add elements the operator explicitly confirmed as part of
     this call; never infer "Economic Buyer" confirmed just because a
     senior person attended, for instance.

4. **Update `accounts/<customer>/`** — append to (or create)
   `accounts/<customer>/debriefs.md`, newest entry on top:
   ```
   ### <date> — <one-line outcome>
   Objections: ...
   Next steps: ...
   Notes: ...
   ```
   If `accounts/<customer>/brief.md` exists (from `process-customer`),
   flag whether anything in it is now stale (e.g. a "verify before next
   call" item that got resolved, a new risk that surfaced).

5. **Update `memory.md`** with what changed this session.

6. **Announce the changes** (Notion guardrail: edit freely, announce at
   the end) and suggest the logical next step — e.g. "next: process-
   customer refresh before the follow-up call" or "call-coach on this
   call" (which, if Gong is connected, can now pull the same transcript
   directly).

## Output example (shape only — no real account names in git)

```
Debriefed <account> (call <date>):
- Outcome: Went well - next step booked. Technical deep-dive scheduled.
- Objections: pricing vs. incumbent CMS; rollout timeline.
- Next steps: AE to send technical session invite for <date> (CALL).

Updated:
- Notion Debriefs: new row "<account> — <date>".
- Notion Accounts: Stage -> Deep Dive/Demo, Next call -> <date>.
- accounts/<account>/debriefs.md: entry added.
- memory.md: session log updated.

Next: process-customer refresh before the deep-dive to fold in the
pricing objection.
```

## Guardrails

- Never fabricate outcomes, objections, or next steps — if the input
  (Gong transcript or the operator's own words) is incomplete, ask rather than
  filling gaps.
- A Gong transcript is confirmed with the operator before it writes to Notion —
  never let an auto-pulled summary set a Stage on its own.
- MEDDPICC updates require an explicit statement from the operator, not an
  inference from attendee seniority or call tone.
- This is Notion writes, not a live product environment — no second confirmation needed,
  but still announce what changed at the end (CLAUDE.md guardrail 4).
