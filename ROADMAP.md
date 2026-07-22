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
- [x] `build-deck` — drafted AND now proven on a real account: an FR
      demo deck built by adapting the closest example deck, French, 3
      confirmed stations, 19 slides. Gained a confirm-the-station-shortlist gate
      and a title-slide-headshot step (`resources/AEs & SEs/<Name>.jpeg`)
      from real mistakes caught this session.
- [x] `demo-script` — drafted, overwritten by mistake, then restored +
      improved: Tell-Show-Tell per station, objections sourced from
      `resources/battle-cards/` (still empty — flag if used before
      populated), verify-before-call checklist, PLUS a new
      confirm-the-outline gate before finalizing (framing is a judgment
      call, not a lookup). Real run done on a live FR account.
- [x] `demo-setup` — drafted, overwritten by mistake, then restored +
      improved, then reframed (2026-07-20): this skill's job is the
      written setup script itself, full stop — not a placeholder
      waiting on the Storyblok MCP connector. Real run done on a
      live FR account. If a connector ever exists, the same script becomes
      directly executable (dry-run → OK → write → verify, CLAUDE.md
      guardrail 3) — a bonus, not the point. Gained a
      confirm-the-space-structure gate (single space vs. per-brand).
- Phase 3 is now genuinely exercised end-to-end on one real account,
  not just specced. Two self-caught mistakes this session (station/
  photo inference without confirming; overwriting real prior skill
  content without reading it first) — both fixed and documented rather
  than repeated.

## Phase 4 — Cadence (weeks 5-6)
- [x] `weekly-review` — drafted, output is plain chat text (no artifact,
      standing preference), scheduled Mondays 08:30 Paris
      (`darwin-weekly-review`). Not yet exercised on a real Monday.
- [x] `monthly-review` — drafted, self-audits memory.md/CHANGELOG.md for
      recurring friction and runs `darwin-improve` on real patterns,
      scheduled 1st of month 08:00 Paris (`darwin-monthly-review`). Not
      yet exercised on a real run.
- [x] `todo-sync` — drafted: creates/checks off items on the single
      real "✅ To Do" checklist block on Walid's Notion space page,
      deliberately narrow (personal action items only, never
      account-specific next-steps, which stay on account rows).
- [x] `dashboard` — not a routine skill, but a live Cowork artifact
      (`walid-deals-dashboard`), now a real Kanban: "Walid's Pipeline"
      (his deals, Notion-sourced, grouped by Stage) + a unified 7-day
      #se-requests scan that routes every AE-pod request one of two
      places — "Team Pipeline" (grouped by whichever of Chakit/
      Roberto/Ines actually claimed it, verified per-thread) or "Worth
      Claiming" (everything else). Rebuilt 2026-07-21 to replace the
      earlier batch-AI extraction (fragile, slow, caused a real
      multi-minute hang) with deterministic regex parsing of the raw
      Slack dump plus one small scoped AI call per thread — smaller,
      more reliable, and matches how Walid actually wants it checked.
      Claimed-but-untracked deals now auto-sync into Notion (Walid's
      own DB — edit-freely guardrail applies). One-click Claim still
      writes to Notion, never auto-sends to Slack. Mirrored on the
      Notion side too: "Pipeline board" view enriched + renamed
      "🗂️ Sales Pipeline (Board)", chart view renamed "📊 Analytics
      (charts)" to stop the two being confused, "By status" renamed
      "By AE" (it was mislabeled — it groups by AE, not Stage).
- [x] Morning brief scheduling — done early via Cowork's native scheduler
      (see Phase 1 note); n8n is no longer needed for this specific job.

Phase 4 is now fully drafted. `weekly-review`/`monthly-review`/
`todo-sync` are mechanism-complete but not yet exercised on a real
scheduled run — worth watching the first real firings closely.

## Phase 5 — Heavy artillery (weeks 6-10)
- [~] `rfp-answer` — mechanism drafted (RETRIEVE/HARVEST against a
      markdown answer library, not a vector DB — corpus is realistically
      too small to need one, and a flat library is more auditable for
      anything that ends up in a legally-reviewed submission). Not
      genuinely running yet: `resources/rfp-library/answers/` has zero
      real entries. Blocked on Walid seeding it with real content from
      his first live RFP deals or an existing team RFP bank. `won-lost-notes.md` gitignored on creation
      (guardrail 10) since it will name real deals/customers.
- [~] `storyblok-content` — mechanism drafted (dry-run → confirm →
      write → verify). Walid provided a Management API token
      (2026-07-21, stored gitignored in `storyblok.env`) — but a live
      test showed this Cowork sandbox's network proxy blocks
      `storyblok.com` entirely (`blocked-by-allowlist`), independent of
      having a token. Two unblock paths: run it from Walid's own
      terminal (the workspace folder, and the token file with it,
      already exists on his real machine — no sandbox in the way), or
      have a Team/Enterprise org Owner add `storyblok.com` to Cowork's
      network allowlist. No MCP connector exists for Storyblok either
      way (checked directly).

## Phase 0 — Portability (added 2026-07-21)
- [x] `darwin-setup` — the one-time interview that makes this repo
      correct for whoever is actually running it, on any computer or
      account. Wired to fire automatically the first time Darwin runs
      with no `memory.md` present (CLAUDE.md "First run" check) — not
      something the person has to remember to invoke. Introduces
      Darwin first, then rebuilds Identity/Voice/guardrails in
      CLAUDE.md, `resources/people.md`, and `ROADMAP.md`/`memory.md`
      from scratch for the new person, rather than silently inheriting
      the previous account's specifics. Not yet exercised on a real
      fresh install — first real test will be whenever this repo is
      actually reused on another machine or for another person.

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
- 2026-07-20: Phase 3 exercised for real, first time — an FR account:
  `build-deck` (French, 19 slides), `demo-script`, `demo-setup` all run
  against a real account brief. Storyblok MCP re-verified still absent.
  Two mistakes caught and fixed same-day: silent station/photo
  inference (now gated behind explicit confirmation in `build-deck`)
  and an accidental overwrite of `demo-script`/`demo-setup`'s real
  Phase 3 content (restored, merged with the new confirmation gates).
  New reference resource: `resources/AEs & SEs/<Full Name>.jpeg` for
  deck title-slide photos — only Walid's exists so far, Thibault's (AE)
  still missing. `resources/battle-cards/` still empty.
- 2026-07-20 (cont'd): Phase 4 started and mostly landed same day —
  `call-coach`/`post-call-update` exercised for real on a live deal, full
  audit run + monthly-review skill built, no-artifact-for-routines
  standing preference set, `weekly-review` drafted and scheduled,
  `monthly-review` scheduled, and a live `walid-deals-dashboard`
  Cowork artifact built (Notion "My Deals" + Slack-triaged "Worth
  Claiming", 24h window, Claim writes to Notion only). Only
  `todo-sync` remains genuinely unstarted in Phase 4.
