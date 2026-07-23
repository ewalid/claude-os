---
name: daily-briefing
description: >
  Trigger: "morning brief", "daily brief", "give me the brief", or the
  start of a session with no other explicit ask. Produces the operator's daily
  operational snapshot — scannable in under a minute.
---

# daily-briefing

## What it does

Pulls together Google Calendar, Notion ("the operator's space"), Gmail, and both
Slack channels (#se-requests and #se-sgm, or the operator's
equivalents — channel IDs in local `memory.md`) into one
scannable brief. Read-only — never modifies anything as part of the brief
itself (Notion hygiene fixes are proposed/applied separately, not folded
into this skill's own read-only pass — though if the operator reacts to the
brief with new evidence, e.g. "X is mine" or forwards a thread, normal
CLAUDE.md guardrail 4 applies to that follow-up).

## Steps

1. **Calendar**: list today's events (and, if relevant, the next few days)
   on the operator's work email (recorded in `memory.md`). Flag any demo calls explicitly.
2. **Notion**: pull the Accounts DB. For every row with a date, do NOT
   trust the "Due date" field label — cross-check against calendar and
   recent Slack/email to determine whether it's actually a **call** or a
   **deadline** (see known issue, HANDOFF.md §2 / memory.md). Mislabel
   this and the whole brief is wrong.
3. **Slack**: scan from the start of the last WORKING day through now
   (so a Monday run also covers Friday + the weekend, not just today) —
   never a flat message-count cutoff, or asks posted overnight/over a
   weekend get silently dropped. Scan #se-requests for (a) deals the operator
   has claimed — these are obligations, (b) unclaimed deals involving
   their AE pod (listed in `resources/people.md`) — these are
   "worth claiming?" flags, never
   tasks, and (c) any thread awaiting the operator's reply. Scan #se-sgm for
   anything relevant to their live accounts.
   **People check** (see `resources/people.md` for the actual names):
   the operator's SE colleagues/peers listed there are NOT AEs — if one
   of them has already replied to or claimed a request, it's handled,
   never flag it as "worth claiming", and never confuse one of their
   names with an AE. The operator's manager (also in `people.md`) is not
   an AE or SE peer — their messages are normal context, not a flag.
4. **Email** (Gmail, the operator's work address from `memory.md` — added 2026-07-20):
   scan inbox from the start of the last WORKING day through now, same
   window as Slack. Read-only — never draft or send anything (CLAUDE.md
   guardrail 1, hard rule regardless of what this skill says). Surface
   two kinds of things: (a) anything tied to a live account — AE/client
   threads relevant to the operator's deals (e.g. a proposal negotiation, a
   scheduling thread with a partner like Algolia, a client reply) — fold
   these into the relevant account's context, and if it's real new
   evidence about Stage/AE/Next call/etc., that's a normal Notion
   full-row update per CLAUDE.md guardrail 4, not something to just
   mention and drop; (b) anything genuinely urgent/time-sensitive
   regardless of account (a real deadline, an escalation, something
   needing a same-day reply). Ignore routine platform/tool notifications
   (SaaS invites, digests, delivery notices, access-request emails)
   unless one of them is actually time-sensitive — don't pad the brief
   with noise.
5. **Assemble** using the priority logic (CLAUDE.md): demo today first,
   deadline closing today/this week second, everything else after.

## Output format

Plain chat text, not an artifact (see Notes — 2026-07-20, standing
preference: no artifacts from routines). Opens with the ASCII signature
from CLAUDE.md's "Signature" section (the cat) — always first, before
anything else. Leave a blank line between every section so it's
actually scannable, not a wall of text.

🎯 DEMOS TODAY
- [Account] at [time] — [one-line context]. Check before call: [items].
  (or: "None today.")

⏰ DEADLINES
- [Account]: [deadline] — [what's due, status]
  (EOD-today first, then this week; skip if none)

💬 SLACK
- Needs reply: [thread/link, one line]
- Worth claiming?: [account, AE, one line] — unclaimed, not a task

📧 EMAIL
- [account-relevant or urgent item, one line]
  (or: "Nothing urgent.")

✅ TODOS
- [from Notion ToDo checklist]

One-liner: [the single most important thing today, in one sentence]

## Good output example

```
     /\_/\
    ( o.o )
     > ^ <
    Darwin

🎯 DEMOS TODAY
None today. Next demo: <Account A>, July 21 (first demo call — Notion
lists this as "due June 21," which is wrong; the real date is the call
itself, July 21).

⏰ DEADLINES
- <Account B>: proposal due July 17 (2 days out) — Proposal stage, HIGH.
  Real deadline, confirmed against Notion + no conflicting calendar entry.
- <Account C>: RFP due July 31 — CMS section owned by the operator, joint with
  an SE peer. Shared validation sheet in progress.

💬 SLACK
- Needs reply: <AE> asked in #se-requests about PoC timeline for an
  account — no response yet (posted yesterday).
- Worth claiming?: unclaimed request in #se-requests for an account
  tied to another AE in the pod — not yet picked up by any SE.

📧 EMAIL
- <Account D>: an AE confirmed the operator as SE on the account in a thread
  with a search/integration partner team — custom demo set for July 29.

✅ TODOS
- Finish onboarding (Notion ToDo).

One-liner: the <Account B> proposal is the tightest deadline this week — due July 17.
```

## Notes

- If a connector is unreachable, say so plainly in the brief rather than
  silently omitting that section.
- Never guess a date's meaning — if it can't be cross-checked, mark it
  "need validation" instead of asserting call vs. deadline.
- **Standing preference (2026-07-20): no artifacts for routines.** This
  brief — and `weekly-review`/`monthly-review` — always output as plain,
  well-formatted chat text with blank lines between sections. There is
  a legacy Cowork artifact (`darwin-morning-brief`) from an earlier
  design that pulled the same sources into a live visual dashboard —
  it's no longer the preferred output and isn't actively maintained;
  leave it alone rather than updating it going forward. If the operator ever
  wants a visual dashboard again, that's a new explicit ask, not a
  default to fall back into.
- Never draft or send emails from this skill or any other — CLAUDE.md
  guardrail 1 is absolute, read-only email access only.
