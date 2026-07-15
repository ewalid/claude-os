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

## Open items carried forward
- [ ] Notion restructure: awaiting Walid's OK to execute.
- [ ] Watch the scheduled daily-briefing's first several unattended runs
      closely (weekdays 9am Paris) — correct anything wrong immediately.
- [ ] Confirm Storyblok MCP reachability in Cowork (Phase 5, not urgent;
      currently not present as a tool in this session).
- [ ] Build `call-coach`, `post-call-update`, `darwin-improve` (Phase 2).
- [ ] Rob Scholte's RFP help request in #se-requests (July 15) — confirm
      whether Walid is picking this up.
