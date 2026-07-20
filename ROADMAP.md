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
- [x] `build-deck` — drafted AND now proven on a real account: Jouet
      club deck built from the Cellbes AB example, French, 3 confirmed
      stations, 19 slides. Gained a confirm-the-station-shortlist gate
      and a title-slide-headshot step (`resources/AEs & SEs/<Name>.jpeg`)
      from real mistakes caught this session.
- [x] `demo-script` — drafted, overwritten by mistake, then restored +
      improved: Tell-Show-Tell per station, objections sourced from
      `resources/battle-cards/` (still empty — flag if used before
      populated), verify-before-call checklist, PLUS a new
      confirm-the-outline gate before finalizing (framing is a judgment
      call, not a lookup). Real run done for Jouet club.
- [x] `demo-setup` — drafted, overwritten by mistake, then restored +
      improved, then reframed (2026-07-20): this skill's job is the
      written setup script itself, full stop — not a placeholder
      waiting on the Storyblok MCP connector. Real run done for Jouet
      club. If a connector ever exists, the same script becomes
      directly executable (dry-run → OK → write → verify, CLAUDE.md
      guardrail 3) — a bonus, not the point. Gained a
      confirm-the-space-structure gate (single space vs. per-brand).
- Phase 3 is now genuinely exercised end-to-end on one real account,
  not just specced. Two self-caught mistakes this session (station/
  photo inference without confirming; overwriting real prior skill
  content without reading it first) — both fixed and documented rather
  than repeated.

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
- 2026-07-20: Phase 3 exercised for real, first time — Jouet club:
  `build-deck` (French, 19 slides), `demo-script`, `demo-setup` all run
  against a real account brief. Storyblok MCP re-verified still absent.
  Two mistakes caught and fixed same-day: silent station/photo
  inference (now gated behind explicit confirmation in `build-deck`)
  and an accidental overwrite of `demo-script`/`demo-setup`'s real
  Phase 3 content (restored, merged with the new confirmation gates).
  New reference resource: `resources/AEs & SEs/<Full Name>.jpeg` for
  deck title-slide photos — only Walid's exists so far, Thibault's (AE)
  still missing. `resources/battle-cards/` still empty. Phase 4 is next
  and genuinely unstarted.
