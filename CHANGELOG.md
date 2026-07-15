# CHANGELOG

All notable changes Darwin makes to itself, this repo, or Walid's connected
tools (Notion, scheduled tasks, artifacts) are logged here — newest first.
Session-level narrative detail still lives in `memory.md`; this file is the
short, skimmable "what changed and when" record. Update it alongside any
`improve:` commit.

## 2026-07-15

- **Notion — Accounts DB restructured.** Split the ambiguous "Due date"
  property (kept as "Due date (legacy)") into explicit **Next call** and
  **Deadline** date properties. Added an **AE** select property (Thibault
  de Maison Rouge, Rob Scholte, Mine Heck, Kristoffer Strindevall,
  Unassigned/need validation) and a **MEDDPICC** multi-select property
  (Metrics, Economic Buyer, Decision Criteria, Decision Process, Paper
  Process, Identify Pain, Champion, Competition) for deal qualification
  tracking. Populated the 3 live rows: Jouet club → Next call 2026-07-21,
  AE Thibault de Maison Rouge; Implid → Deadline 2026-07-17; Cera RFP →
  Deadline 2026-07-31 (Implid/Cera AE left unassigned — not confirmed by
  any source, flagged need-validation rather than guessed).
- **Notion — new views added** on the Accounts DB: "This week", "Demos
  upcoming" (filtered to Type=Demo, sorted by Next call), "Overdue"
  (sorted by Deadline), and a "Dashboard" view tab (created, but Notion's
  CHART configuration DSL rejected every syntax tried via the API this
  session — the tab exists empty; charts still need to be added by hand
  in the Notion UI, or scripted again once the DSL issue is understood).
- **Notion — new "📞 Debriefs" database** created and linked inline on
  Walid's space, with a relation back to Accounts. Empty for now — will
  be populated by the future `post-call-update` skill (Phase 2).
- **daily-briefing artifact updated** to read the new Next call/Deadline/
  AE/MEDDPICC fields directly instead of guessing from one ambiguous
  date field, and to surface AE + MEDDPICC status on each account card.
- **daily-briefing artifact links Slack permalinks** — every needs-reply
  and worth-claiming item now links to the actual message in #se-requests
  or #se-sgm (constructed from channel ID + Slack "Message TS").
- **daily-briefing became a live Cowork artifact** (`darwin-daily-briefing`)
  instead of chat text — pulls Calendar/Notion/Slack fresh on every open,
  uses a Haiku pass (`askClaude`) to classify call-vs-deadline and triage
  Slack into needs-reply / worth-claiming.
- **daily-briefing scheduled** via Cowork's native scheduler (task
  `darwin-daily-briefing`, weekdays 9:00am Paris) — n8n no longer needed
  for this job.
- **Repo created**: private GitHub repo `ewalid/claude-os`, scaffolded
  from HANDOFF.md — CLAUDE.md, ROADMAP.md, `.claude/skills/{daily-
  briefing,process-customer}`, `resources/`, `accounts/`.

## 2026-07-15 (later same day)

- **People model corrected**: `resources/people.md` now distinguishes
  the AE pod from SE colleagues (Chakit Arora, Roberto Butti, Ines
  Akrap) and Walid's manager (Matthew Alberts). Wired into the
  `darwin-daily-briefing` artifact's Slack triage prompt and
  `daily-briefing/SKILL.md` so an SE peer's reply/claim is never
  mistaken for an unclaimed AE opportunity.
- **Slack scan window widened**: from a flat 25-message cap to "start
  of the last working day through now" (a Monday run also covers
  Friday + the weekend). Applied in the artifact and documented in
  `daily-briefing/SKILL.md`.
- **Deck format decided**: `resources/deck-examples/` will hold .pptx
  exports from Google Slides, not .odp — see `style-guide.md`.

## Known gaps / carried forward

- Notion "Dashboard" view has no charts yet (API limitation hit today).
- Implid and Cera RFP have no confirmed AE — need validation from Walid.
- MEDDPICC not yet populated for any account — needs Walid's input per
  deal, or for `process-customer` (Phase 2 rebuild) to start capturing it.
