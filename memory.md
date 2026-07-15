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

## Open items carried forward
- [ ] Notion restructure: DONE (2026-07-15) — no longer open.
- [ ] Watch the scheduled daily-briefing's first several unattended runs
      closely (weekdays 9am Paris) — correct anything wrong immediately.
- [ ] Confirm Storyblok MCP reachability in Cowork (Phase 5, not urgent;
      currently not present as a tool in this session).
- [ ] Build `call-coach`, `post-call-update`, `darwin-improve` (Phase 2).
- [ ] Rob Scholte's RFP help request in #se-requests (July 15) — confirm
      whether Walid is picking this up.
- [ ] Notion "Dashboard" view has no charts — needs a human touch in the
      Notion UI, or another scripted attempt later.
- [ ] Implid and Cera RFP have no confirmed AE — ask Walid.
- [ ] MEDDPICC not populated for any account yet — needs Walid's input.
