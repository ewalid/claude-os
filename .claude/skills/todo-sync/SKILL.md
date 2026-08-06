---
name: todo-sync
description: >
  Trigger: "sync my todos", "update my to-do list", or as a step inside
  `daily-briefing`/`weekly-review` when a new personal action item
  surfaces, or an existing one looks done or no longer relevant.
  Dynamically suggests items to add or remove from the "✅ To Do"
  checklist block on the operator's Notion space page (page id in local
  `memory.md`) — the one real to-do list, not a new system — and then
  applies the update in the same pass.
---

# todo-sync

## What it does

The operator has exactly one personal to-do list: the "✅ To Do" checklist
block at the top of their Notion space page. Everything else (account
next-steps, MEDDPICC gaps, RFP deadlines) already lives on the
Accounts DB rows — todo-sync does NOT duplicate those. It only handles
The operator's own personal action items (the kind that don't belong to a
specific account row) — things like "finish onboarding," "meet with
an SE peer on the RFP," "investigate white labelling feature."

This is intentionally narrow. It is not a task-management system, not
a second source of truth, and not a place for account-specific
next-steps — those stay on the account's own Notion page/Notes per
`process-customer`/`post-call-update`.

**Dynamic, not passive.** This isn't just an append-only log. Every run
actively looks for two kinds of changes — items to add AND items to
remove — surfaces both with its reasoning in the same output, and then
writes the update. Matches CLAUDE.md guardrail 4's Notion pattern: no
blocking proposal step, no waiting for an OK — the "suggestion" IS the
announcement that accompanies the write, so it's visible and correctable
after the fact rather than silent.

## When it runs

- On demand ("sync my todos").
- As a step folded into `daily-briefing` and `weekly-review`: if either
  surfaces a clearly personal action item for the operator (not tied to a
  specific account, e.g. "reply to Matthew about the QBR slot," "submit
  expense report") that isn't already on the checklist, add it there
  instead of just mentioning it and dropping it. Same pass: if either
  surfaces evidence that an existing item is done, superseded, or no
  longer relevant, act on that too — don't just add and never prune.

## Steps

1. **Read the current checklist.** Fetch the Notion page (page id in
   local `memory.md`) and read the "✅ To Do" block as it exists right
   now — never assume its contents from memory.

2. **Reconcile additions — don't duplicate.** For each new candidate item:
   - Skip it if an existing unchecked item is clearly the same thing
     (don't add "meet re: the RFP" if "meet with peer on the RFP" is
     already there).
   - Otherwise append it as a new unchecked item: `- [ ] <item>`. If the
     item has enough detail to be worth a nested page (context, links,
     sub-steps), create it as a child page under the same Notion page
     and link it from the checklist line, matching the style of
     existing linked items.

3. **Check off completed items** when there's real evidence they're
   done (the operator says so directly, or a downstream skill/action clearly
   completed it — e.g. `post-call-update` just logged the debrief that
   was the to-do item). Never check something off on a guess.

4. **Suggest and apply removals.** An item can leave the list two ways:
   - **Done** → check it off (step 3), never delete the line.
   - **No longer relevant** (cancelled meeting, request already handled
     by someone else, superseded by a different plan) → this is a real
     removal, not a check-off. When there's a clear reason, delete the
     line — but always name the reason in the same chat output where
     the removal happens (e.g. "Removed: prep AIR call Friday — Kateryna
     moved it to next week"), so it's a surfaced decision, not a silent
     deletion. If a nested detail page exists for that item, leave the
     page itself in place (don't delete Notion pages) — only remove the
     checklist line pointing to it.
   - Still never guess: if it's ambiguous whether an item is stale (no
     clear evidence either way), leave it on the list and flag the
     uncertainty in chat instead of removing it.

5. **Write the update** via `notion-update-page` with `update_content`
   (search-and-replace on the `## ✅ To Do` block — read the existing
   block text first, then submit the smallest accurate diff covering
   every addition, check-off, and removal from this pass). Per
   CLAUDE.md guardrail 4, no proposal step — just write it and announce
   what changed.

6. **Announce** exactly what was added, checked off, and removed (with
   the reason for each removal), in the chat response. If nothing
   changed, say so plainly rather than staying silent (so it's clear
   the step ran).

## Guardrails

- Never invent a personal to-do from an ambiguous signal — if it's not
  clearly the operator's own action item, leave it alone rather than guess.
- Never add account-specific next-steps here — those belong on the
  account's own page/Notes (CLAUDE.md guardrail 4 already covers full
  Notion-row hygiene for accounts).
- Removals need a real, statable reason — never remove an item just
  because it's been sitting there a while. If the reason is genuinely
  unclear, flag it in chat and leave the item rather than removing it.
- Deleting a checklist line is fine when justified (see step 4); deleting
  a nested detail *page* is not part of this skill — those stay even if
  their checklist line is removed, unless the operator explicitly asks
  to clean them up.
