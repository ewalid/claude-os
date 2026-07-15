# ROADMAP.md — Darwin's build plan

Rule: a skill must be genuinely good before the next one is built. Value
ships every week; nothing is built speculatively. See `HANDOFF.md` §5-6
for full rationale.

## Phase 1 — Foundation (now)
- [x] `daily-briefing` skill drafted
- [ ] Run daily, tune via corrections
- [ ] Ship the Notion restructure proposal (see HANDOFF.md §2), execute on OK

## Phase 2 — Core loop (weeks 2-3)
- [x] `process-customer` skill drafted
- [ ] `call-coach`
- [ ] `post-call-update`
- [ ] `darwin-improve`

## Phase 3 — Demo pack (weeks 3-5)
- [ ] `demo-script`
- [ ] `demo-setup` (needs Storyblok MCP working in Cowork)
- [ ] `build-deck` (needs example decks in `resources/deck-examples/`)

## Phase 4 — Cadence (weeks 5-6)
- [ ] `weekly-review`
- [ ] `monthly-review`
- [ ] `todo-sync`
- [ ] `dashboard`
- [ ] n8n scheduling of the morning brief (once its output is trusted)

## Phase 5 — Heavy artillery (weeks 6-10)
- [ ] `rfp-answer` (+ Excel handling, + library harvesting)
- [ ] `storyblok-content`

## Not skills
- Coding help (native to Cowork)
- Notion restructure (one-time task, not recurring)
- Morning automation (n8n schedules `daily-briefing` later, not a skill itself)

## Status log
- 2026-07-15: Repo scaffolded. Connectors verified: Calendar ✅, Notion ✅,
  Slack ✅, GitHub ✅ (repo `ewalid/claude-os`). Storyblok MCP: not present
  in this Cowork session — flagged, not urgent (Phase 5).
