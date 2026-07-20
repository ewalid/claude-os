---
name: daily-briefing
description: >
  Trigger: "morning brief", "daily brief", "give me the brief", or the
  start of a session with no other explicit ask. Produces Walid's daily
  operational snapshot — scannable in under a minute.
---

# daily-briefing

## What it does

Pulls together Google Calendar, Notion ("Walid's space"), Gmail, and both
Slack channels (#se-requests C06EMPB41SL, #se-sgm C0B290Q8XDK) into one
scannable brief. Read-only — never modifies anything as part of the brief
itself (Notion hygiene fixes are proposed/applied separately, not folded
into this skill's own read-only pass — though if Walid reacts to the
brief with new evidence, e.g. "X is mine" or forwards a thread, normal
CLAUDE.md guardrail 4 applies to that follow-up).

## Steps

1. **Calendar**: list today's events (and, if relevant, the next few days)
   on walid.elmselmi@storyblok.com. Flag any demo calls explicitly.
2. **Notion**: pull the Accounts DB. For every row with a date, do NOT
   trust the "Due date" field label — cross-check against calendar and
   recent Slack/email to determine whether it's actually a **call** or a
   **deadline** (see known issue, HANDOFF.md §2 / memory.md). Mislabel
   this and the whole brief is wrong.
3. **Slack**: scan from the start of the last WORKING day through now
   (so a Monday run also covers Friday + the weekend, not just today) —
   never a flat message-count cutoff, or asks posted overnight/over a
   weekend get silently dropped. Scan #se-requests for (a) deals Walid
   has claimed — these are obligations, (b) unclaimed deals involving
   his AE pod (Thibault de Maison Rouge, Rob Scholte, Mine Heck,
   Kristoffer Strindevall) — these are "worth claiming?" flags, never
   tasks, and (c) any thread awaiting Walid's reply. Scan #se-sgm for
   anything relevant to his live accounts.
   **People check** (see `resources/people.md`): Chakit Arora, Roberto
   Butti, and Ines Akrap are Walid's SE colleagues/peers, NOT AEs — if
   one of them has already replied to or claimed a request, it's
   handled, never flag it as "worth claiming", and never confuse one
   of their names with an AE. Matthew Alberts is Walid's manager, not
   an AE or SE peer — his messages are normal context, not a flag.
4. **Email** (Gmail, walid.elmselmi@storyblok.com — added 2026-07-20):
   scan inbox from the start of the last WORKING day through now, same
   window as Slack. Read-only — never draft or send anything (CLAUDE.md
   guardrail 1, hard rule regardless of what this skill says). Surface
   two kinds of things: (a) anything tied to a live account — AE/client
   threads relevant to Walid's deals (e.g. a proposal negotiation, a
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

📧 EMAIL
- [account-relevant or urgent item, one line]
  (or: "Nothing urgent.")

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

📧 EMAIL
- Winfarm Group: Thibault confirmed Walid as SE on the account in a
  thread with the Algolia partner team — custom demo set for July 29.

✅ TODOS
- Finish onboarding (Notion ToDo).

One-liner: Implid proposal is the tightest deadline this week — due July 17.
```

## Notes

- If a connector is unreachable, say so plainly in the brief rather than
  silently omitting that section.
- Never guess a date's meaning — if it can't be cross-checked, mark it
  "need validation" instead of asserting call vs. deadline.
- Walid prefers this rendered visually, not as a chat wall of text. There is
  a live Cowork artifact (`darwin-morning-brief` — NOT `darwin-daily-briefing`;
  that id got orphaned under a different Claude account on 2026-07-15 and
  Cowork won't let it be reused, so the rebuilt artifact lives under this new
  id) that pulls calendar, the Notion Accounts DB (Next call/Deadline/AE/
  MEDDPICC), and both Slack channels fresh on every open, and uses a quick
  Haiku pass (via `askClaude`) to triage Slack into "needs reply" / "worth
  claiming" (demo-vs-deadline classification is now read directly from the
  explicit Next call/Deadline fields post-restructure, no guessing needed).
  Point Walid to that artifact for the visual version; only fall back to a
  plain chat-text brief if artifacts aren't available in context. Gmail is
  not yet wired into the artifact itself (added to the chat-text skill
  2026-07-20) — add it to the artifact's callMcpTool calls next time the
  artifact gets touched, using the same last-working-day window as Slack.
- If Walid corrects the artifact's classification (e.g. it mislabels a
  call as a deadline, or flags/misses a Slack item), that's a prompt-
  wording fix inside the artifact's askClaude prompts, not a one-off
  chat correction — update the HTML/JS and redeploy via update_artifact.
- Never draft or send emails from this skill or any other — CLAUDE.md
  guardrail 1 is absolute, read-only email access only.
