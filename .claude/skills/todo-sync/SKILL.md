---
name: todo-sync
description: >
  Trigger: "sync my todos", "update my to-do list", or as a step inside
  `daily-briefing`/`weekly-review` when a new personal action item
  surfaces. Creates and checks off items on the "✅ To Do" checklist
  block on Walid's Notion space page (page id
  7e2677cd8aef48c3bee3136fdbb6a536) — the one real to-do list, not a
  new system.
---

# todo-sync

## What it does

Walid has exactly one personal to-do list: the "✅ To Do" checklist
block at the top of his Notion space page. Everything else (account
next-steps, MEDDPICC gaps, RFP deadlines) already lives on the
Accounts DB rows — todo-sync does NOT duplicate those. It only handles
Walid's own personal action items (the kind that don't belong to a
specific account row) — things like "finish onboarding," "meet with
an SE peer on the RFP," "investigate white labelling feature."

This is intentionally narrow. It is not a task-management system, not
a second source of truth, and not a place for account-specific
next-steps — those stay on the account's own Notion page/Notes per
`process-customer`/`post-call-update`.

## When it runs

- On demand ("sync my todos").
- As a step folded into `daily-briefing` and `weekly-review`: if either
  surfaces a clearly personal action item for Walid (not tied to a
  specific account, e.g. "reply to Matthew about the QBR slot," "submit
  expense report") that isn't already on the checklist, add it there
  instead of just mentioning it and dropping it.

## Steps

1. **Read the current checklist.** Fetch the Notion page
   (`7e2677cd8aef48c3bee3136fdbb6a536`) and read the "✅ To Do" block
   as it exists right now — never assume its contents from memory.

2. **Reconcile, don't duplicate.** For each new candidate item:
   - Skip it if an existing unchecked item is clearly the same thing
     (don't add "meet re: the RFP" if "meet with peer on the RFP" is
     already there).
   - Otherwise append it as a new unchecked item: `- [ ] <item>`.

3. **Check off completed items** when there's real evidence they're
   done (Walid says so directly, or a downstream skill/action clearly
   completed it — e.g. `post-call-update` just logged the debrief that
   was the to-do item). Never check something off on a guess.

4. **Write the update** via `notion-update-page` with `update_content`
   (search-and-replace on the `## ✅ To Do` block — read the existing
   block text first, then submit the smallest accurate diff). Per
   CLAUDE.md guardrail 4, no proposal step — just write it and announce
   what changed.

5. **Announce** exactly what was added/checked off, in the chat
   response. If nothing changed, say so plainly rather than staying
   silent (so it's clear the step ran).

## Guardrails

- Never invent a personal to-do from an ambiguous signal — if it's not
  clearly Walid's own action item, leave it alone rather than guess.
- Never add account-specific next-steps here — those belong on the
  account's own page/Notes (CLAUDE.md guardrail 4 already covers full
  Notion-row hygiene for accounts).
- Never remove/delete a checklist item outright — only check it off.
  If an item looks stale or wrong, flag it in chat rather than
  silently deleting.
