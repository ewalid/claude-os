---
name: post-call-update
description: >
  Trigger: "debrief [account]", "debrief the [account] call", or right
  after Walid mentions a call just happened. Captures the outcome and
  propagates it everywhere it needs to live — Notion Debriefs DB,
  Accounts DB, the account's own file, and memory.md.
---

# post-call-update

## What it does

Closes the loop after a call: one debrief in, four places updated out.
Never invents outcomes — this skill is only as good as what Walid
actually tells it (or a Gong transcript he pastes in; Gong itself isn't
connected).

## Steps

1. **Get the debrief.** If Walid's trigger message already contains the
   outcome, objections, and next steps, use it. If it's thin ("debrief
   Jouet club"), ask: what happened, what was the outcome, what
   objections came up, what's the next step (and is it a CALL or a
   DEADLINE — label it explicitly, same rule as everywhere else in this
   repo). If he pastes a Gong transcript, summarize it but still have
   him confirm the outcome/next-step before writing anything — a
   misread transcript writing the wrong stage into Notion is a real cost.

2. **Write a new row to the Notion "📞 Debriefs" database:**
   - Call: `<Account> — <date>`
   - Account: relation to the matching Accounts DB row
   - Call date: the actual call date
   - Outcome: one of Went well - next step booked / Mixed - follow-up
     needed / Stalled or at risk / Lost / Won
   - Objections, Next steps, Notes: from what Walid gave you

3. **Update the Accounts DB row** for that account:
   - Stage, if it changed.
   - Next call or Deadline — whichever the new next-step actually is
     (never write into "Due date (legacy)").
   - Notes — refresh if stale.
   - MEDDPICC — only add elements Walid explicitly confirmed as part of
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
   customer refresh before the follow-up call" or "call-coach if you
   have the Gong recording."

## Good output example

```
Debriefed Jouet club (call 2026-07-21):
- Outcome: Went well - next step booked. Technical deep-dive scheduled.
- Objections: pricing vs. incumbent CMS; timeline for LGR rollout.
- Next steps: Anush to send technical session invite for July 28 (CALL).

Updated:
- Notion Debriefs: new row "Jouet club — 2026-07-21".
- Notion Accounts: Stage -> Deep Dive/Demo, Next call -> 2026-07-28.
- accounts/jouet-club/debriefs.md: entry added.
- memory.md: session log updated.

Next: process-customer refresh before July 28 to fold in the pricing
objection and prep the deep-dive.
```

## Guardrails

- Never fabricate outcomes, objections, or next steps — if Walid's
  input is incomplete, ask rather than filling gaps.
- MEDDPICC updates require an explicit statement from Walid, not an
  inference from attendee seniority or call tone.
- This is Notion writes, not Storyblok — no second confirmation needed,
  but still announce what changed at the end (CLAUDE.md guardrail 4).
