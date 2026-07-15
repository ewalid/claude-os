---
name: daily-briefing
description: >
  Trigger: "morning brief", "daily brief", "give me the brief", or the
  start of a session with no other explicit ask. Produces Walid's daily
  operational snapshot — scannable in under a minute.
---

# daily-briefing

## What it does

Pulls together Google Calendar, Notion ("Walid's space"), and both Slack
channels (#se-requests C06EMPB41SL, #se-sgm C0B290Q8XDK) into one
scannable brief. Read-only — never modifies anything (Notion hygiene
fixes are proposed separately, not folded into this skill).

## Steps

1. **Calendar**: list today's events (and, if relevant, the next few days)
   on walid.elmselmi@storyblok.com. Flag any demo calls explicitly.
2. **Notion**: pull the Accounts DB. For every row with a date, do NOT
   trust the "Due date" field label — cross-check against calendar and
   recent Slack to determine whether it's actually a **call** or a
   **deadline** (see known issue, HANDOFF.md §2 / memory.md). Mislabel
   this and the whole brief is wrong.
3. **Slack**: scan #se-requests for (a) deals Walid has claimed —
   these are obligations, (b) unclaimed deals involving his AE pod
   (Thibault de Maison Rouge, Rob Scholte, Mine Heck, Kristoffer
   Strindevall) — these are "worth claiming?" flags, never tasks, and
   (c) any thread awaiting Walid's reply. Scan #se-sgm for anything
   relevant to his live accounts.
4. **Assemble** using the priority logic (CLAUDE.md): demo today first,
   deadline closing today/this week second, everything else after.

## Output format

```
🎯 DEMOS TODAY
- [Account] at [time] — [one-line context]. Check before call: [items].
  (or: "None today.")

⏰ DEADLINES
- [Account]: [deadline] — [what's due, status]
  (EOD-today first, then this week; skip if none)

💬 SLACK
- Needs reply: [thread/link, one line]
- Worth claiming?: [account, AE, one line] — unclaimed, not a task

✅ TODOS
- [from Notion ToDo checklist]

One-liner: [the single most important thing today, in one sentence]
```

## Good output example

```
🎯 DEMOS TODAY
None today. Next demo: Jouet club, July 21 (first demo call — Notion
lists this as "due June 21," which is wrong; the real date is the call
itself, July 21).

⏰ DEADLINES
- Implid: proposal due July 17 (2 days out) — Proposal stage, HIGH.
  Real deadline, confirmed against Notion + no conflicting calendar entry.
- Cera: RFP due July 31 — CMS section owned by Walid, joint with Ines.
  Shared validation sheet in progress.

💬 SLACK
- Needs reply: Rob Scholte asked in #se-requests about PoC timeline for
  [account] — no response yet (posted yesterday).
- Worth claiming?: unclaimed request in #se-requests for an account
  tied to Kristoffer Strindevall — not yet picked up by any SE.

✅ TODOS
- Finish onboarding (Notion ToDo).

One-liner: Implid proposal is the tightest deadline this week — due July 17.
```

## Notes

- If a connector is unreachable, say so plainly in the brief rather than
  silently omitting that section.
- Never guess a date's meaning — if it can't be cross-checked, mark it
  "need validation" instead of asserting call vs. deadline.
