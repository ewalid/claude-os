# ROADMAP.md — Darwin's build plan

Rule: a skill must be genuinely good before the next one is built. Value
ships every week; nothing is built speculatively. See `HANDOFF.md` §5-6
for full rationale.

## Phase 1 — Foundation (now)
- [x] `daily-briefing` skill drafted
- [x] Scheduled: runs automatically weekdays at 9:00am Paris time via Cowork's
      native scheduler (task `darwin-daily-briefing`) — Walid opted to skip
      the original "tune manually first" gate and go straight to automation,
      using Cowork's scheduler instead of n8n. Still watch its output closely
      for the first couple weeks; correct anything wrong immediately.
- [x] Notion restructure executed: Next call / Deadline split, AE property,
      MEDDPICC property, This week / Demos upcoming / Overdue / Dashboard
      views, new Debriefs database. Dashboard view has no charts yet (API
      rejected the CHART DSL every way tried — needs a human touch or a
      retry later).

## Phase 2 — Core loop (weeks 2-3)
- [x] `process-customer` skill drafted
- [x] `call-coach` drafted — coaches from pasted Gong transcripts/notes
      (Gong itself not connected), feeds private `resources/coaching-log.md`
- [x] `post-call-update` drafted — "debrief [account]" writes to the
      Debriefs DB, updates Accounts DB, updates `accounts/<customer>/`
- [x] `darwin-improve` drafted — formalizes the "how I improve" loop
      from CLAUDE.md into a concrete classify-apply-commit-log procedure

## Phase 3 — Demo pack (weeks 3-5)
- [x] `build-deck` drafted — Walid dropped 5 real decks into
      `resources/deck-examples/`; template structure reverse-engineered
      and documented in style-guide.md. Adapts these decks, never
      builds from scratch.
- [x] `demo-script` drafted — chains off the account brief + the deck's
      demo stations; Tell-Show-Tell per station, objections sourced from
      `resources/battle-cards/` (currently empty — flag if used before
      populated), verify-before-call checklist.
- [x] `demo-setup` drafted, but **execution blocked**: no Storyblok MCP
      tool present in this Cowork session (verified 2026-07-15, same
      result as the original HANDOFF.md flag). Skill is fully specced
      (dry-run preview → OK → write → verify, per CLAUDE.md guardrail 3)
      and ready to run the moment the connector appears — don't attempt
      to improvise around its absence.

## Phase 4 — Cadence (weeks 5-6)
- [ ] `weekly-review`
- [ ] `monthly-review`
- [ ] `todo-sync`
- [ ] `dashboard`
- [x] Morning brief scheduling — done early via Cowork's native scheduler
      (see Phase 1 note); n8n is no longer needed for this specific job.

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
- 2026-07-15: daily-briefing scheduled — weekdays 9:00am Paris, Cowork native
  scheduler, task id `darwin-daily-briefing`.
- 2026-07-15: daily-briefing rebuilt as a live Cowork artifact.
- 2026-07-15: Notion restructure executed (Next call/Deadline/AE/MEDDPICC,
  new views, Debriefs DB). CHANGELOG.md added — see it for the full list.
- 2026-07-15: Corrected AE-pod vs. SE-colleague vs. manager confusion risk
  (people.md updated); widened Slack scan to last-working-day-through-now;
  decided deck-examples format is .pptx, not .odp.
- 2026-07-15: Phase 2 complete — drafted `darwin-improve`, `post-call-update`,
  `call-coach`. All of Phase 1 + Phase 2 now done; Phase 3 (demo pack) is
  next but blocked on Storyblok MCP + example decks.
- 2026-07-15: Phase 3 mostly complete — 5 real decks landed in
  `resources/deck-examples/`, unblocking `build-deck` (template
  documented in style-guide.md). Drafted `build-deck` and `demo-script`.
  `demo-setup` drafted but still can't run live — Storyblok MCP still
  absent from this Cowork session. Phase 4 (`weekly-review`,
  `monthly-review`, `todo-sync`, `dashboard`) is next.
