# memory.md — rolling context

Read this fully at the start of every session. Update it at the end of
every session, before signing off. This is the only continuity mechanism
Darwin has — no update, no memory.

---

## Session log

### 2026-07-15 — Session 1 (build + first run)
- Built the repo from HANDOFF.md: CLAUDE.md, ROADMAP.md,
  `.claude/skills/{daily-briefing,process-customer}/SKILL.md`,
  `resources/` scaffold, `accounts/` scaffold (jouet-club, implid, cera).
- Created private GitHub repo `ewalid/claude-os`, pushed initial commit.
- Verified connectors live:
  - Google Calendar ✅ — walid.elmselmi@storyblok.com reachable.
  - Notion ✅ — "Walid's space" + Accounts DB queried directly. HANDOFF's
    known issue confirmed exactly as described: Jouet club's Notion "Due
    date" is 2026-06-21 but the Notes field itself says "First demo on
    july 21" — the property is stale/wrong, not just conceptually risky.
  - Slack ✅ — #se-requests (C06EMPB41SL) reachable by search. #se-sgm
    (C0B290Q8XDK) does NOT show up via slack_search_channels by name —
    only works when read directly by channel ID. Use the ID directly for
    #se-sgm going forward, don't rely on name search for it.
  - GitHub ✅ — repo `ewalid/claude-os` created and pushed.
  - Google Drive — tool available, not yet exercised.
  - Storyblok MCP — no tool present in this Cowork session at all. Not
    urgent (Phase 5), but flagged as still unresolved (HANDOFF said
    "verify in Cowork" — verified: not currently there).
- Notion Accounts DB rows confirmed directly (2026-07-15):
  - Jouet club: Due date 2026-06-21 (WRONG), Notes "First demo on july
    21", Stage In progress, Priority High.
  - Implid: Due date 2026-07-17 (real deadline), Stage Proposal, Notes
    "Sending Proposal", Priority High.
  - Cera RFP: Due date 2026-07-31, Stage In progress, Notes "complete
    the CMS RFP with Ines", Priority High.
  - 4 other rows in the DB are blank/template stubs — ignore.
- New context not in HANDOFF.md: Walid is shadowing Ines Akrap's Stokke
  account this week (Demo Prep July 15, Demo Shadow July 16) — this is
  Ines's account, not Walid's; include in calendar context but not as
  an owned obligation.
- Slack scan of #se-requests: Rob Scholte (AE pod) posted July 15 asking
  for help on an RFP document he sent out — needs-reply candidate, not
  yet confirmed claimed. Thibault de Maison Rouge (AE pod) has two other
  live opportunities in-flight besides Jouet club (EM Normandie, Winfarm
  Group) — both already have next steps assigned to Thibault's side, not
  obviously needing an SE yet; not flagged as "worth claiming" this pass.
- Ran first live morning briefing (full content given in chat, July 15).
- Proposed Notion restructure (split Due date → Next call / Deadline,
  add AE property, add views, add Debriefs DB) — awaiting Walid's OK
  before executing.
- Walid asked to keep moving on the roadmap and confirmed daily-briefing
  should be a routine. Scheduled it via Cowork's native scheduler (task
  `darwin-daily-briefing`, weekdays 9:00am Paris) — he explicitly chose to
  skip the original "tune manually first" gate, and to use Cowork's
  scheduler instead of n8n. Still watch its unattended output closely for
  the first couple of weeks.
- Walid asked for the daily-briefing to be more visual with more context.
  Built a live Cowork artifact (`darwin-daily-briefing`) instead of just
  reformatting chat text — it calls Calendar/Notion/Slack directly via
  callMcpTool on every open, and uses askClaude (Haiku) to classify
  accounts as CALL vs. DEADLINE and triage Slack into needs-reply /
  worth-claiming. Verified live: all 4 underlying tool calls succeeded
  (calendar, notion-query-database-view, notion-fetch, slack x2) with
  the expected shapes. Updated daily-briefing/SKILL.md to point future
  sessions at this artifact instead of only the chat-text format.
- Executed the Notion restructure (Walid gave full latitude — "execute
  now, feel free to change statuses or types, maybe a dashboard is better
  than a table"): split "Due date" -> "Due date (legacy)" + new **Next
  call** / **Deadline** date properties; added **AE** select (4 pod names
  + Unassigned/need validation) and **MEDDPICC** multi-select (8 standard
  elements) to the Accounts DB. Populated the 3 live rows: Jouet club ->
  Next call 2026-07-21 + AE Thibault de Maison Rouge (confirmed via
  Slack); Implid -> Deadline 2026-07-17; Cera RFP -> Deadline 2026-07-31
  (Implid/Cera AE left unassigned, not guessed). Added views: This week,
  Demos upcoming (Type=Demo), Overdue, and a Dashboard tab. IMPORTANT
  LIMITATION: Notion's CHART configure DSL rejected every syntax I tried
  via this API this session (quote handling errors, then "Unknown
  directive: CAPTION", then the view stopped resolving entirely on
  retry) — the Dashboard tab exists but is empty. Needs a human to add
  charts in the Notion UI, or another attempt once the DSL quirk is
  understood (see CHANGELOG.md).
- Created a new "📞 Debriefs" database (Call/Account relation/Call date/
  Outcome/Objections/Next steps/Notes), linked inline on Walid's space,
  for the future `post-call-update` skill (Phase 2).
- Added `CHANGELOG.md` at repo root — short newest-first log of concrete
  changes, separate from this file's longer narrative. Updated CLAUDE.md
  to make maintaining it part of the standing "how I improve" practice.
- Updated `process-customer/SKILL.md` to read Next call/Deadline/AE/
  MEDDPICC directly, added a MEDDPICC section to the brief template, and
  pointed Phase A at the new Debriefs DB.
- Updated the `darwin-daily-briefing` artifact twice more: (1) to use the
  new Next call/Deadline/AE/MEDDPICC fields instead of guessing from one
  ambiguous date, surfacing AE and MEDDPICC status per account card; (2)
  to link every Slack needs-reply/worth-claiming item to its real message
  permalink (constructed client-side from the channel ID + "Message TS"
  already present in the raw Slack text — format confirmed against real
  forwarded-message links seen in #se-sgm: https://storyblok.slack.com/
  archives/<channel_id>/p<ts with dot removed>). Verified via
  verify_artifact after each change — all live tool calls succeeded.
- Correction from Walid: Chakit Arora, Roberto Butti, Ines Akrap are his
  SE colleagues/peers, NOT AEs (current output wasn't wrong, but this
  context needed to be recorded — e.g. Chakit had already replied "I'll
  take it" to an ask, which is a valid claim from an SE, not something
  to flag as unclaimed). Matthew Alberts is Walid's manager. Recorded in
  `resources/people.md`, and wired into the `darwin-daily-briefing`
  artifact's Slack triage prompt + `daily-briefing/SKILL.md` so these
  three groups (AE pod / SE colleagues / manager) are never confused.
- Correction from Walid: check Slack "from the last working day to the
  day of the routine", not just the most recent N messages. Added a
  last-working-day window (skips weekends: Monday runs also cover Friday)
  to the artifact's Slack calls (`oldest` param), replacing the flat
  limit:25 cap. Updated `daily-briefing/SKILL.md` to describe this.
- Answered Walid's question on deck-examples file format: .pptx, not
  .odp — Google Slides round-trips to .pptx natively, and pptx has much
  better tooling support for the future `build-deck` skill to parse.
  Recorded in `resources/style-guide.md` and `ROADMAP.md` Phase 3.

### 2026-07-15 — Session 2 (scheduled daily-briefing, unattended)
- First unattended run of `darwin-daily-briefing`. All connectors
  reachable: Calendar, Notion, Slack (#se-requests + #se-sgm by ID),
  GitHub. No Storyblok MCP tool present (still unresolved, Phase 5).
- Accounts DB unchanged since last session: same 3 live rows (Jouet
  club, Implid, Cera RFP), same 4 blank stubs. No new accounts.
- Cross-checked Jouet club's Next call (2026-07-21) directly against
  calendar — confirmed live: "JouéClub-LGR / Storyblok (CMS)" event at
  17:00 Paris on July 21. Also spotted a "prep call Jouet club" on
  Walid's own calendar July 16, 10:00-12:00 (self-created, not in
  Notion) — worth having ready before then.
- Resolved last session's open item: Rob Scholte's P&V Group RFP ask
  in #se-requests. Read the full thread — Chakit Arora (SE colleague,
  not AE) already asked Rob on July 14 "anything needed from the SE
  team?"; Rob replied July 15 "yes I need help on the RFP document."
  This is Chakit's engagement to follow up on, not Walid's — correctly
  not flagged as a Walid obligation or worth-claiming.
- New Slack finding: Play'n GO (Kristoffer Strindevall, AE pod) posted
  in #se-requests July 14 — "Additional Discovery Needed," no SE has
  replied in-thread. Flagged as "worth claiming?" this run.
- Today's calendar (July 15) has no demo owned by Walid — Stokke Demo
  Prep (11:00) is Ines's account (shadowing), QBR Q3 Strategic Growth
  (14:00-17:00) is internal. Confirmed no false "demo today" trigger.
- Notion ToDo checklist: "investigate white labelling feature" still
  open; other two items checked off.
- No corrections from Walid this run (unattended, no reply expected
  before this entry was written).

### 2026-07-15 — Session 3 (Phase 2 build-out)
- Walid said "keep with the roadmap" — built the three remaining Phase 2
  skills:
  - `darwin-improve` — formalizes CLAUDE.md's "how I improve" loop:
    identify friction -> classify (CLAUDE.md for recurring rules / a
    skill's SKILL.md for task-specific procedure / resources/ for
    reference material / accounts/<customer>/ for one-off account facts
    / a live artifact via update_artifact+verify_artifact) -> apply
    everywhere it touches in one pass -> commit as `improve: ...` -> log
    in CHANGELOG.md -> update memory.md. Documents today's people-model
    correction as its worked example.
  - `post-call-update` — trigger "debrief [account]": writes a new row
    to the Debriefs DB (Outcome/Objections/Next steps), updates the
    Accounts DB row (Stage/Next call/Deadline/MEDDPICC — MEDDPICC only
    from explicit statements, never inferred from attendee seniority),
    appends to `accounts/<customer>/debriefs.md`, updates memory.md.
    Never fabricates outcomes — asks Walid if the trigger message
    doesn't already state what happened.
  - `call-coach` — coaches from a pasted Gong transcript or notes (Gong
    not connected, never claims otherwise): 3-5 things done well + 3-5
    to improve, each quoting an actual moment, critiqued against the
    call's stated goal from `accounts/<customer>/brief.md`, one focus
    for next call. Feeds private `resources/coaching-log.md` — never
    surfaced anywhere customer-facing.
  All three are drafted only — none has had a real run yet, so treat
  their first uses as a trial the same way the artifact's first runs
  were watched closely.
- Phase 2 of ROADMAP.md is now fully checked off. Updated ROADMAP.md
  and CHANGELOG.md accordingly, pushed everything to GitHub.

### 2026-07-15 — Session 4 (Phase 3 build-out)
- Walid said "phase 3 now." Found Walid had already dropped 5 real demo
  decks into `resources/deck-examples/` (Stokke, fashioncheque, Payabl,
  Cellbes AB, Orange Cyberdefense) — unblocking `build-deck`. Parsed all
  5 with python-pptx to find the shared template: title (account + AE +
  Walid as SE) → "What we know so far" (Key Business Result / Observed
  constraints / What is needed / What does success look like / By when)
  → themed demo-stations overview (2-4 stations, picked per account
  priorities — seen: Commerce & Integrations, AI Capabilities, Developer
  Experience & Integration, Structured Content & Reuse, Platform
  Architecture & Security) → per-station triptych (transition slide /
  blank "Demo" slide / "KEY TAKEAWAYS" slide with a real customer-proof
  quote, e.g. Spendesk, Marc O'Polo) → optional Technical Topics
  (FlowMotion, Storyblok MCP Server, Composable Headless, Performance &
  Scalability, security) for more technical/enterprise accounts →
  optional commercial section (Pricing, Partnership Orchestration,
  Implementation Methodology, Premium vs Elite packages) for larger
  deals → "Thank You for Joining!" close. Also noticed some source decks
  still carry leftover instructional placeholder text on unfilled
  slides (literal "Tell-Show-Tell" authoring instructions) — documented
  that `build-deck` must treat this as empty scaffolding, never copy it
  forward as real content. Wrote all of this into `style-guide.md`.
- Built `build-deck` — adapts the closest-fitting example deck per the
  account brief; demo slides always stay blank (never fake demo
  content); never invents a slide type outside the 5-deck template.
- Built `demo-script` — Tell-Show-Tell per demo station, chained off
  `build-deck`'s stations + the account brief; objections sourced from
  `resources/battle-cards/` (currently empty, flagged); verify-before-
  call checklist. Output goes to a separate markdown file, not onto the
  deck itself.
- Built `demo-setup`, but confirmed it still can't run live — no
  Storyblok MCP tool present in this Cowork session (checked the tool
  list again, same absence as Session 1's original HANDOFF.md flag).
  Fully specced with the dry-run-preview → OK → write → verify flow so
  it's ready to go the moment the connector shows up; explicitly told
  not to improvise around the absence (e.g. via browser automation).
- Phase 3 of ROADMAP.md is now checked off except demo-setup's live
  execution. Updated ROADMAP.md and CHANGELOG.md, pushed to GitHub.
- New known gaps surfaced by this pass: `resources/battle-cards/` is
  empty (demo-script has nothing to source objections from yet); no
  `accounts/<customer>/brief.md` exists for any account yet (process-
  customer hasn't had a real run, so build-deck/demo-script have
  nothing to chain off until one is written).

### 2026-07-15 — Session 5 (process-customer MEDDPICC upgrade)
- Walid asked: process-customer should analyze pasted Gong + Salesforce
  notes for MEDDPICC and update Notion accordingly — not just report
  whatever was already checked in the MEDDPICC property. Rewrote
  `process-customer/SKILL.md`:
  - New Phase C: works through all 8 MEDDPICC elements against the
    pasted Gong/SF extracts + Slack/Debriefs/calendar context, tagging
    each Confirmed (with a quoted piece of evidence + source) / Partial
    (signal but incomplete) / Gap (nothing) — never inferred from tools
    that aren't connected, only from what Walid actually provided.
  - Phase E now writes Confirmed elements straight to the Notion
    MEDDPICC multi-select automatically (CLAUDE.md guardrail 4: edit
    Notion freely, announce at the end) — except a *downgrade* of a
    previously-confirmed element, which still needs Walid's OK first
    since removing something is a different kind of judgment call than
    adding.
  - The brief's MEDDPICC section now shows all 8 elements' full status
    (Confirmed/Partial/Gap + evidence), not just a checklist mirroring
    Notion — so gaps are visible even if Notion only shows confirmed ones.
- Logged in CHANGELOG.md, pushed to GitHub.
- Not yet tested live — no account has real Gong/SF extracts pasted in
  yet, so this hasn't had a real run. First use will show whether the
  Confirmed/Partial/Gap bar is calibrated right.

### 2026-07-15 — Session 6 (process-customer: full-row rewrite, not just MEDDPICC)
- Walid: "I need the agent to be able to update everything inside the
  Notion doc. Not just MEDDPICC, everything... my manual notes are
  worthless, the agent handles everything, so from the SF and Gong info
  you update everything." This is a bigger correction than the last
  one — it's not just "add MEDDPICC," it's "stop treating Stage/
  Priority/Notes/AE/dates as things that need my sign-off before you
  touch them."
- Checked CLAUDE.md guardrail 4 — it already said "I can edit Notion
  freely, announce at the end." The friction was that
  `process-customer/SKILL.md`'s Phase E had over-restricted itself
  relative to that standing rule (added a "propose diff, wait for OK"
  step for Stage/Priority/AE/dates, and a downgrade-gate on MEDDPICC)
  — a task-specific procedure quietly contradicting the general rule.
  Fixed in two places:
  - CLAUDE.md guardrail 4: added an explicit sentence that "freely"
    means the whole row, not one column, and that Walid's manual notes
    are not the source of truth relative to real Gong/SF/Slack evidence.
  - `process-customer/SKILL.md`: renamed Phase C to "Full account
    analysis" — now derives Stage/Priority/AE/Next call/Deadline/Notes
    in addition to MEDDPICC, all from real evidence only (still never
    invents anything Salesforce/Gong-shaped that wasn't actually
    pasted in). Phase E now writes all of it directly, no OK gate on
    any field including MEDDPICC downgrades — just an old→new→evidence
    announcement, so Walid can spot-check without having to greenlight
    each write.
- What did NOT change: the Phase B hard stop (still must have Walid
  paste real Gong/SF content before analysis runs — nothing to analyze
  otherwise), never-fabricate-Salesforce/Gong content, never touch
  human-only columns, never touch "Due date (legacy)" (still deprecated,
  Next call/Deadline are the real fields).
- Logged in CHANGELOG.md, pushed to GitHub.
- Untested live — same as the last MEDDPICC change, no account has had
  a real process-customer run with actual Gong/SF pastes yet. Both
  changes will get their first real exercise together whenever that
  happens.

---

## Standing facts (update if they change; don't duplicate HANDOFF.md)

- AE pod: Thibault de Maison Rouge, Rob Scholte, Mine Heck, Kristoffer
  Strindevall. Walid claims only some of their accounts.
- Slack channels: #se-requests (C06EMPB41SL) — searchable by name.
  #se-sgm (C0B290Q8XDK) — search by name fails, always use the ID.
- Notion "Walid's space": Due date property is actively wrong for Jouet
  club right now (2026-06-21 shown, 2026-07-21 is the real call date).
  Always cross-check before treating a date as a deadline.
- GitHub repo: `ewalid/claude-os` (private).
- Stokke: Ines Akrap's account, Walid is shadowing (not his to run).
- Notion Accounts DB schema (as of 2026-07-15): Name, Type, Stage,
  Priority, Notes, Owner(s), AE, Next call, Deadline, Due date (legacy),
  MEDDPICC. New Debriefs DB exists (empty). Dashboard view exists but
  has no charts (API limitation).
- People groups (see resources/people.md): AE pod = Thibault de Maison
  Rouge, Rob Scholte, Mine Heck, Kristoffer Strindevall. SE colleagues
  (peers, not AEs) = Chakit Arora, Roberto Butti, Ines Akrap. Manager =
  Matthew Alberts.
- deck-examples/ file format: .pptx (not .odp) — decks authored in
  Google Slides, exported to .pptx.
- process-customer's Phase C/E (as of Session 6) rewrites the whole
  Notion row from real evidence, no OK gate on any field — see
  CLAUDE.md guardrail 4.

## Open items carried forward
- [ ] Notion restructure: DONE (2026-07-15) — no longer open.
- [ ] Watch the scheduled daily-briefing's first several unattended runs
      closely (weekdays 9am Paris) — correct anything wrong immediately.
      (2026-07-15 unattended run: clean, no corrections needed.)
- [ ] Confirm Storyblok MCP reachability in Cowork (Phase 5, not urgent;
      currently not present as a tool in this session).
- [x] Build `call-coach`, `post-call-update`, `darwin-improve` (Phase 2)
      — all drafted 2026-07-15 Session 3. Watch their first real runs.
- [ ] Play'n GO (Kristoffer Strindevall, AE pod) — unclaimed discovery
      follow-up in #se-requests (July 14) — "worth claiming?", not yet
      picked up by any SE as of July 15.
- [ ] Notion "Dashboard" view has no charts — needs a human touch in the
      Notion UI, or another scripted attempt later.
- [ ] Implid and Cera RFP have no confirmed AE — ask Walid.
- [ ] MEDDPICC not populated for any account yet — needs real Gong/SF
      extracts pasted into a `process-customer` run to populate it.
- [ ] Scheduled `darwin-daily-briefing` task's prompt still describes
      chat-text output, not the live artifact — needs updating.
- [x] Phase 3 (demo pack): `build-deck` and `demo-script` drafted
      2026-07-15 Session 4, using the 5 real decks Walid dropped into
      `resources/deck-examples/`. `demo-setup` drafted but still can't
      run live — Storyblok MCP still absent.
- [ ] `resources/battle-cards/` is empty — `demo-script`'s objection-
      sourcing has nothing to draw on until it's populated.
- [ ] No `accounts/<customer>/brief.md` exists for any account yet —
      run `process-customer` for real before `build-deck`/`demo-script`
      can actually be used on a live account.
- [x] `process-customer` now does a real MEDDPICC analysis from Gong/SF
      pastes and auto-updates Notion (2026-07-15 Session 5) — untested
      live, no account has real extracts pasted in yet.
- [x] `process-customer` rewrites the ENTIRE Notion row (Stage/Priority/
      Notes/AE/dates/MEDDPICC), no OK gate on any field (2026-07-15
      Session 6) — untested live, same reason.
- [ ] Phase 4 (`weekly-review`, `monthly-review`, `todo-sync`,
      `dashboard`) is next on the roadmap.
