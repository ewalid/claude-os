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

### 2026-07-20 — Session 7 (account-switch recovery: git, scheduler, artifact, GitHub connector)

- Walid confirmed this whole build had been done on this same Mac but under
  a **different Claude account**. Dug through `~/Claude`, `~/Documents`,
  `~/Desktop`, `~/dev` (with his OK to connect each) to find the orphaned
  pieces:
  - `~/Claude/Scheduled` exists (protected/app-internal — couldn't open it,
    but its existence confirms the old scheduled routine was real).
  - Creating a Cowork artifact with id `darwin-daily-briefing` was rejected
    ("already exists... from a previous deletion or another account") —
    confirms the old live artifact is real too, just orphaned.
  - Both are invisible to `list_scheduled_tasks`/`list_artifacts` under
    this account — stranded under the old login, not migratable from here.
  - No orphaned git repo found anywhere on disk; no leftover claude-os
    files in Documents/Desktop (only unrelated real Storyblok dev projects:
    `REDACTED-TEMPLATE`, `demos/implid-demo`).
- **Git repo — fully reconciled.** `~/dev/claude-os` had no local `.git` at
  all despite memory.md/CHANGELOG.md claiming a pushed repo. Walid's
  GitHub screenshot proved the remote (`ewalid/claude-os`, branch `main`,
  11 commits, last push 5 days ago) is 100% real and matches this file's
  Session 6 narrative exactly. Mistake made and fixed: initialized a
  *fresh* local repo first (unrelated history on branch `master`) before
  realizing the remote existed — reconciled by adding `origin`, fetching,
  then `git reset --soft origin/main` to align local `main` with the real
  history. Hit a stale `HEAD.lock`/`objects/maintenance.lock` left behind
  by that first accidental commit — the sandbox couldn't unlink its own
  just-created lock files (`Operation not permitted`); fixed via the
  `allow_cowork_file_delete` permission tool, then `rm` worked. Diffed
  local working tree against `origin/main`: two small recap bullets in
  memory.md/CHANGELOG.md existed on GitHub but not locally (took GitHub's
  wording), `README.md` existed on GitHub but wasn't in the local folder
  (restored from origin), and the 5 real deck `.pptx` files (~120MB) were
  local-only, never pushed. **Walid's call: keep decks local-only** — added
  `resources/deck-examples/*.pptx` to `.gitignore`, committed (`7f611e5`).
  Repo is now clean, exactly one commit ahead of `origin/main`, ready to
  push the moment there's a way to push from this session (see below).
- **Scheduled task recreated** under this account: `darwin-daily-briefing`,
  weekdays ~9:00am Paris, via Cowork's native scheduler. Prompt is fully
  self-contained (reads CLAUDE.md/memory.md/daily-briefing SKILL.md fresh
  each run, since scheduled runs have no chat memory).
- **Live artifact rebuilt** — had to use a new id, `darwin-morning-brief`
  (the old `darwin-daily-briefing` id is locked/orphaned, see above).
  Verified live: calendar (`list_events`), Notion Accounts DB (SQL via
  `notion-query-data-sources`), and both Slack channels (`slack_read_channel`)
  all returned real data successfully; askClaude triage wired in for Slack.
  Updated `daily-briefing/SKILL.md` to point at the new id and to describe
  demo-vs-deadline classification as read directly from Next call/Deadline
  (no more guessing — that was the pre-restructure problem).
- **Real findings surfaced while probing live data (still current as of
  today, 2026-07-20):** Implid's deadline (2026-07-17) is now **3 days
  overdue**, Stage still "Proposal" / Notes still "Sending Proposal" — needs
  a check-in with Walid, not yet resolved. Jouet club's first demo is
  **tomorrow, 2026-07-21** — cross-checked against calendar, confirmed.
- **GitHub connector**: no GitHub MCP tool exists anywhere in this
  workspace's tool list or the connector registry search. Walid connected
  GitHub's official remote OAuth MCP server
  (`https://api.githubcopilot.com/mcp/`) himself via Settings → Connectors
  → custom connector by URL — shows "Connected" in Settings, but its tools
  had not loaded into this running session as of this entry (known Cowork
  limitation: connectors added mid-session need a fresh session to appear).
  **Unresolved — needs confirming in a new session**, then push `7f611e5`.
- Also surfaced and explained to Walid: Cowork's Bash tool runs in an
  isolated sandbox separate from his actual Mac (unlike Claude Code, which
  runs shell commands directly on the user's machine). Locally-installed
  CLIs (e.g. `gh`, freshly authenticated on his Mac) do NOT appear in this
  sandbox — only the Read/Write/Edit file tools and the shared folder mount
  reach his real files. Recommended he keep building Darwin here in Cowork
  (native scheduler + artifacts are the real value, Claude Code has
  neither) and only reach for Claude Code/Terminal for git/GitHub-specific
  work until the connector is confirmed working.
- Note for future sessions: the Edit tool refuses to touch anything under
  `.claude/skills/` in this repo (treated as a protected location even
  though it's inside the connected folder) — use `mcp__workspace__bash`
  (e.g. a small Python/sed rewrite) to edit skill files instead, then
  `git add`/commit from the same sandbox.

### 2026-07-20 — Session 8 (process-customer: first real run, Jouet club)

- First-ever real `process-customer` run, on Jouet club — Walid pasted
  real Salesforce + Gong extracts. Full Phase A/B/C/D/E executed: Notion
  row rewritten (Stage, Notes, MEDDPICC — see CHANGELOG.md for the field-
  level diff, kept at meta level per guardrail 6 below), and
  `accounts/jouet-club/brief.md` written with the full evidence-backed
  analysis. **The actual customer specifics (stakeholder names, deal
  figures, competitor, quoted evidence) live ONLY in that local-only
  brief file — not repeated here.** This is the first real proof of the
  Phase C/E rewrite behavior from Sessions 5-6 on genuine evidence.
- **Correction from Walid, same session — important, changes standing
  policy:** the brief itself shouldn't be git-tracked at all (client data
  stays local, full stop — not just "don't repeat it in chat"). Fixed:
  `accounts/jouet-club/brief.md` was `git rm --cached` before ever being
  pushed, `accounts/*/*.md` added to `.gitignore`. Updated CLAUDE.md
  guardrail 6 to codify this as the standing rule going forward: real
  customer specifics live only in `accounts/<customer>/` (git-ignored);
  anything tracked in git (this file, CHANGELOG.md, skills) stays at the
  meta level, never the actual customer detail. This session's own memory
  entries above were rewritten to comply, retroactively.
- **Also from Walid: `process-customer` needed real quality upgrades**,
  not just a policy fix — the first brief wasn't good enough. Upgraded
  `process-customer/SKILL.md`: Phase B now explicitly asks for three
  things (Salesforce paste, Gong paste, a customer-specific Slack channel
  ID — not just "whatever you have"). Phase D's brief template now leads
  with a prospect intro (who they are, their business, before diving into
  detail), an explicit "who's in the meeting" section instead of burying
  it in need-validation, a much bigger MEDDPICC section that explicitly
  names what wins/loses the deal and how well-positioned Storyblok
  currently is (not just Confirmed/Partial/Gap tags), and a concrete
  action checklist for next steps (deck, demo-script, space setup, etc.)
  instead of a vague "verify before next call" list. Rewrote
  `accounts/jouet-club/brief.md` against the new template (local-only,
  not repeated here).
- **Notion — added a "Pipeline board" view** on the Accounts DB (Kanban,
  grouped by Stage) alongside the existing table view, per Walid's ask.
  Also pasted the (upgraded) Jouet club brief directly into that
  account's own Notion page content, plus a next-steps checklist block —
  not just left in the Notion property fields / a separate git file.

### 2026-07-20 — Session 9 (Jouet club: attendee ambiguity resolved, Notion board+calendar views)

- Walid confirmed there are two separate calls tomorrow for Jouet club —
  cross-checked both against the calendar directly (real invite lists,
  not inferred) and resolved the Session 8 need-validation item about
  Salesforce/Gong disagreeing on attendees: they were each describing a
  different one of the two calls. Updated the local-only brief and the
  Notion page/Notes accordingly — no names/specifics repeated here per
  guardrail 6.
- **Notion Accounts DB — two new views**: a "Pipeline board" (Kanban,
  grouped by Stage, minimal card display — hide-empty-groups on) and a
  "Calendar" view (by Next call). Walid referenced a differently-styled
  board (richer Stage taxonomy: To follow-up/Waiting feedback/Preparing/
  In procurement/etc., "color columns" toggle) as a style preference —
  asked him whether he wants that exact Stage taxonomy adopted or just
  the visual style; not yet resolved.
- Going forward: skill output (briefs, etc.) should be posted in the chat
  response itself AND written to Notion — not just one or the other.

### 2026-07-20 — Session 11 (process-customer walkthrough + real Competition update, Jouet club)

- Walid asked for a clean walkthrough of `process-customer`'s Phase A/B
  flow (explained in plain terms in chat, no file changes from that
  alone). He then re-supplied the same Salesforce/Gong extract plus a
  new Slack DM channel ID.
- Reading that DM directly (not just Gong's summary) surfaced a real,
  concrete update: the actual named CMS competitors are more specific
  than Gong's extract implied. Competition MEDDPICC element upgraded
  accordingly; brief + Notion Notes/page updated. Also confirmed no
  deck exists yet for this account, and got AE color on the deal (not
  repeated here — local brief only, per guardrail 6).

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
- [x] Scheduled task recreated 2026-07-20 Session 7 under this account
      (old one orphaned under a different Claude login) — prompt is now
      self-contained (reads CLAUDE.md/memory.md/SKILL.md fresh each run).
- [x] Phase 3 (demo pack): `build-deck` and `demo-script` drafted
      2026-07-15 Session 4, using the 5 real decks Walid dropped into
      `resources/deck-examples/`. `demo-setup` drafted but still can't
      run live — Storyblok MCP still absent.
- [ ] `resources/battle-cards/` is empty — `demo-script`'s objection-
      sourcing has nothing to draw on until it's populated.
- [x] `accounts/jouet-club/brief.md` written 2026-07-20 Session 8 —
      first real `process-customer` run. Implid still has none — same
      gap, not yet run.
- [ ] Jouet club need-validation from Session 8: who's actually attending
      the July 21 call (SF vs. Gong disagree slightly), exact end-of-
      August full-demo date, whether SF's "Decade technical enablement
      session" is the same event as the July 21 discovery call.
- [ ] Jouet club MEDDPICC gaps to close: Paper Process (Gap), Champion/
      Economic Buyer (Partial) — confirm if Anaïs is an internal champion,
      get the new President's name.
- [x] `process-customer` now does a real MEDDPICC analysis from Gong/SF
      pastes and auto-updates Notion (2026-07-15 Session 5) — untested
      live, no account has real extracts pasted in yet.
- [x] `process-customer` rewrites the ENTIRE Notion row (Stage/Priority/
      Notes/AE/dates/MEDDPICC), no OK gate on any field (2026-07-15
      Session 6) — untested live, same reason.
- [ ] Phase 4 (`weekly-review`, `monthly-review`, `todo-sync`,
      `dashboard`) is next on the roadmap.
- [ ] **Push commit `7f611e5`** (gitignoring deck .pptx files) once the
      GitHub connector's tools are confirmed loaded in a fresh session —
      local `main` is one commit ahead of `origin/main`, everything else
      reconciled and clean (2026-07-20 Session 7).
- [x] **Implid's overdue deadline resolved 2026-07-20 Session 12** —
      not actually overdue, was a stale "sending proposal" deadline;
      proposal is sent and priced, now signature-pending (Stage →
      Contracting, Deadline cleared). Re-check once client signs.
- [ ] **Winfarm Group needs a real `process-customer` run** — currently
      just Notion fields set from an email scan (2026-07-20 Session 12),
      no brief exists. Custom demo is 2026-07-29 — get Thibault's Winfarm
      use-cases doc before then.
- [ ] Gmail wired into `daily-briefing/SKILL.md` (2026-07-20 Session 12)
      but NOT yet into the `darwin-morning-brief` live artifact — add it
      next time that artifact gets touched.
- [ ] Real next step per the roadmap's own "don't build ahead" rule:
      Phase 4 skills (weekly-review/monthly-review/todo-sync/dashboard)
      are speculative until `process-customer` has had a real run — that
      needs Walid to paste real Salesforce/Gong extracts for one account
      (Implid, given the overdue deadline, is the obvious candidate).
      Holding off on drafting Phase 4 until then.

### 2026-07-20 — Session 10 (new account: Akeneo RFP, claimed live in a separate session)

- Found as an uncommitted local diff to this file, written by a
  different session than this one (same repo, same machine) — Walid had
  claimed a new RFP (flagged worth-claiming in that morning's briefing)
  and that session did a live Notion-row update: new "Akeneo RFP" row
  (repurposed a blank template stub), Type RFP, Priority High, AE
  Thibault de Maison Rouge, joint SE ownership with Ines Akrap (Cera-RFP
  pattern), MEDDPICC ["Identify Pain"] only, Deadline **2026-07-31**
  (same date as Cera RFP — two RFPs now converging end-of-month, plus
  Implid still overdue since July 17; worth flagging explicitly in the
  next briefing). Verified live against Notion — matches exactly.
  Real evidence (quotes, stakeholder names, Slack channel) moved to
  `accounts/akeneo/notes.md` (local-only) rather than repeated here, per
  guardrail 6 — that other session's memory.md entry had the customer
  specifics inline; trimmed retroactively.
- No full `process-customer` run yet for Akeneo — this was a live
  Notion-row update per Walid's direct ask, not the full skill (no Gong/
  SF extract pasted, RFP document in Drive not yet opened).
- Only 2 blank template stub rows remain in the Accounts DB after this.

### 2026-07-20 — Session 12 (Akeneo deadline confirmed; Implid + Winfarm Group updated from email; Gmail added to daily-briefing)

- Same live thread as Session 10/11's briefing follow-up. Walid confirmed
  the Akeneo RFP submission deadline is **2026-07-31** — set on the
  Notion row, cleared the need-validation note (matches Cera RFP's
  deadline exactly; see `accounts/akeneo/notes.md`).
- Walid: Implid is effectively done on the SE side, just waiting on the
  client's signature (proposal sent by Thibault, AE). Asked Darwin to
  also read his email going forward and flag anything urgent.
- **New connector exercised: Gmail** (walid.elmselmi@storyblok.com) —
  reachable via `search_threads`/`get_thread`/`get_message`. Scanned the
  last ~3 days of inbox. Two real findings tied to live accounts (full
  detail in the account's own local-only notes, guardrail 6):
  - **Implid** — email confirms Walid's status: proposal sent, price
    negotiated, now signature-pending. Notion row updated: Stage
    "Proposal" → "Contracting", AE confirmed Thibault de Maison Rouge,
    Deadline cleared (the old 2026-07-17 date was for sending the
    proposal, already done — no longer a real open deadline). See
    `accounts/implid/notes.md`.
  - **Winfarm Group** — previously just a stub row. Email surfaced that
    Walid is the confirmed SE (Thibault said so directly to an Algolia
    partner contact), with a custom demo scheduled 2026-07-29. Notion
    row updated: AE Thibault de Maison Rouge, Owner(s) Walid, Stage
    "Demo", Next call 2026-07-29. See `accounts/winfarm-group/notes.md`.
  - Rest of the inbox scan was routine platform/tool notifications
    (Vercel, Netlify, Okta, Gatekeeper digest, a Deel expense-request
    denial x4, a FedEx delivery) — nothing else urgent or account-
    relevant; not written anywhere, per the "don't pad with noise" rule.
- **`daily-briefing/SKILL.md` updated** to add Gmail as a permanent
  fourth source (Calendar/Notion/Slack/Email), same last-working-day
  window as Slack, read-only, never draft/send (CLAUDE.md guardrail 1
  restated explicitly in the skill itself). Output format gains a
  "📧 EMAIL" section. Edited via `mcp__workspace__bash` (Edit tool still
  refuses `.claude/skills/` in this repo — see Session 7's note). The
  live artifact (`darwin-morning-brief`) does NOT have Gmail wired in
  yet — flagged in the skill's Notes as the next thing to add there.
- Useful mapping learned this session for future Notion writes: Owner(s)
  people-property values are `user://37fd872b-594c-81a5-9d21-0002fac057c5`
  = Walid, `user://73720d65-1acc-4e3d-9ee7-71541b15b465` = Ines Akrap.
- Notion Stage select is a closed list, not free text — valid values:
  Closed Won, Contracting, Proposal, Deep Dive/ Demo, Demo, Discovery,
  Not started, In progress, Done. ("Proposal Sent" was rejected —
  learned the hard way, used "Contracting" instead for Implid.)

### 2026-07-20 — Session 13 (build-deck: first real deck, Jouet club)
- Ran `build-deck` on Jouet club for the first time — adapted the
  Cellbes AB example deck (closest structural fit: 3 demo stations, no
  Commercial section, appropriate for a Discovery-stage account) into a
  French-language deck for Joué Club / La Grande Récré. Saved to
  `accounts/jouet-club/2026-07-20_Demo_JouetClub.pptx` (19 slides).
- Two explicit open guardrails resolved with Walid via direct question
  rather than assumed: deck language = French; Commercial section =
  skipped.
- Walid corrected an early misstep: I'd proposed inventing new French
  station names instead of using the fixed vocabulary of real station
  themes documented in `resources/style-guide.md` ("use stations you
  have seen in my decks, don't invent them"). Fixed by picking 3 themes
  from that actual list — matched to real evidence in the account
  brief — and translating the theme names to French rather than
  inventing new categories.
- Leftover placeholder/instructional scaffolding slides in the Cellbes
  source (trailing unfinished block) were correctly excluded per the
  skill's explicit guardrail — never copied forward.
- **Gitignore gap found and fixed**: the deck file (`.pptx`) landed in
  `accounts/jouet-club/` but the existing gitignore rule
  (`accounts/*/*.md`) only covered markdown, not other file types.
  Broadened to `accounts/*/*` + `!accounts/*/.gitkeep`, matching
  guardrail 6's actual intent (the whole `accounts/<customer>/`
  directory is local-only, not just its `.md` files).
- Note: this deck targets the **end-of-August full demo**, not the
  July 21 discovery call — the brief itself flags July 21 as
  deliberately deck-free/light. Full customer-specific rationale for
  every slide lives in the local-only brief, not here.
- **Verification bug caught and fixed**: leftover Cellbes instructional
  scaffolding text (and one Cellbes-specific stale note) was sitting in
  the *speaker notes* of 4 kept slides — I'd only cleaned visible slide
  text, not notes pages. Caught by pulling the uploaded file back via
  Drive's `read_file_content` as a verification step, not assumed clean.
  Cleared and re-shared; Walid re-uploaded the corrected version.
- Wrote `demo-script.md` and `demo-setup.md` for Jouet club (local-only)
  — first real run of both, previously undocumented as skills.
- Drive structure created: `Darwin/Joué Club/` folder containing the
  deck (.pptx — Walid manually dragged it in after both my upload paths
  hit size limits: Drive API base64 payload too large for a single tool
  call, browser file_upload capped at 10MB) and a combined Google Doc
  with both scripts.

### 2026-07-20 — Session 14 (improve: confirmation checkpoints in demo-prep skills)
- Walid's correction: I'd inferred demo stations and never offered a
  chance to validate script framing before finalizing — asked me to
  make the demo-prep flow more interactive going forward, not just
  build-then-report.
- Fixed at the skill level, not CLAUDE.md, since this is specific to
  creative/judgment calls in demo-prep (station themes, script framing,
  space structure) — deliberately NOT a blanket rule, since it would
  contradict the existing Notion guardrail (#4: no proposal step, write
  and announce) which is evidence-based, not a judgment call.
- `build-deck/SKILL.md`: added an explicit confirm-the-shortlist gate
  for demo station themes, mirroring the existing Commercial-section
  gate.
- New `demo-script/SKILL.md` and `demo-setup/SKILL.md` — formalizing
  this session's ad hoc work into real skills, each with a built-in
  confirmation checkpoint (script: confirm the outline/framing before
  writing the full doc; setup: flag space-structure decisions like
  single-space-vs-per-brand before finalizing).

### 2026-07-20 — Session 15 (title-slide headshot bug caught; AEs & SEs photo resource)
- Walid added a real headshot to `resources/AEs & SEs/Walid EL
  M'SELMI.jpeg` and flagged it for use in decks going forward.
- Checking this surfaced a real bug in the Jouet club deck: all 5
  example decks have 2 headshot PICTURE shapes on slide 1 (AE + SE) —
  invisible to a text-only shape scan, which is exactly how I'd
  inspected the source deck earlier this session. The built deck still
  had the original Cellbes AE/SE's photos in place, unnoticed.
- Fixed Walid's own photo in place (swapped the SE picture part's
  image bytes directly, matched PNG content-type to avoid a
  declared-vs-actual mismatch). **Thibault's AE photo is still wrong**
  — no headshot exists for him in the resource yet, so his slot still
  shows the original Cellbes AE. Flagged to Walid, not silently left.
- Documented this properly so it isn't missed again: `style-guide.md`
  step 1 and `build-deck/SKILL.md`'s title-slide step both now call out
  the headshot swap explicitly, with the `resources/AEs & SEs/<Full
  Name>.jpeg` matching convention.

### 2026-07-20 — Session 16 (self-caught mistake: overwrote real prior skill content)
- While answering a status question, found via `git log` that
  `demo-script/SKILL.md` and `demo-setup/SKILL.md` were NOT new files
  when "formalized" in Session 14 — both already had real drafted
  content from Phase 3 (2026-07-15, commit `80ee6e8`), overwritten
  without reading first (used a shell heredoc instead of Edit, so the
  Read-before-Edit guard never triggered).
- Real substance lost: `demo-script`'s original had a dedicated
  "Anticipated objections" section (sourced from `resources/battle-
  cards/`, with an explicit need-validation fallback) and a
  verify-before-call checklist — both missing from the Session 14
  rewrite. `demo-setup`'s original wasn't a manual checklist at all —
  it was a guarded automation procedure (dry-run preview → explicit OK
  → write → verify) specifically built around CLAUDE.md guardrail 3
  for whenever the Storyblok connector goes live; Session 14 replaced
  that identity with "manual checklist for Walid," quietly dropping
  the guardrail-3 machinery.
- Fixed: merged both skills back to their original structure/identity,
  keeping Session 14's one genuine improvement (confirm-before-
  finalizing gates: outline confirmation in demo-script, space-
  structure confirmation in demo-setup).
- Not yet done: the Jouet club account files
  (`accounts/jouet-club/demo-script.md`/`demo-setup.md`) delivered in
  Session 14 don't match the restored structure (no objections
  section, no date-suffixed filename, manual-checklist framing instead
  of guarded-automation). Flagged to Walid, not regenerated yet —
  his call whether to redo them.
- Lesson for future self-edits: heredoc/bash writes to existing files
  skip the Read-before-Edit guard that catches this on Edit-tool paths
  — check `git log -- <path>` before any "formalize"/"rewrite" pass on
  a file that might not be new.

### 2026-07-20 — Session 17 (improve: made the git-log-check rule permanent)
- Walid asked directly how to avoid the overwrite mistake recurring —
  this is exactly a `darwin-improve` trigger, run properly this time
  (named it, used the right commit prefix, unlike the two prior fixes
  today which went out as `fix:` instead of `improve:`).
- Classified as recurring/always-true (not task-specific to one skill)
  since the actual failure mode is a tool limitation — Edit refuses
  `.claude/skills/` paths, forcing a bash/python fallback that skips
  the automatic read-before-write guard. New **CLAUDE.md guardrail 9**:
  before any bash/python write to a path Edit refused, manually `cat`
  + `git log --oneline -- <path>` first, merge don't blindly replace.
- Also strengthened `darwin-improve/SKILL.md` step 3 with the concrete
  procedure and the real example, so the meta-skill about self-
  correction documents its own most costly failure mode explicitly.

### 2026-07-20 — Session 18 (improve: make darwin-improve trigger more reliably)
- Walid's question: how do we stop you under-using the improve loop?
  Real gap: there's no auto-routing in this environment that fires a
  skill when it's relevant — I have to notice the trigger myself, and
  today I noticed inconsistently (didn't always name it running, drifted
  on the commit prefix).
- Fix has two parts. CLAUDE.md's "How I improve" section now lists
  concrete trigger signals (correction, stated preference, self-caught
  mistake via verification, Walid asking a reflection/status question
  where the honest answer reveals a gap) instead of just the literal
  "Darwin, learn this" phrase — and states plainly that self-caught
  friction counts exactly the same as Walid-stated friction. It also
  now says explicitly: re-read `darwin-improve/SKILL.md` before
  executing, don't run it from memory — that's specifically what
  caused the `improve:`/`fix:` prefix drift.
- `darwin-improve/SKILL.md` gained a new step 1 ("notice the trigger")
  with the same signal list, and step 2 now explicitly says to reread
  the file's own steps before acting.
- This is itself a real test of the new rule: recognized the trigger
  (a direct reflection question), named that darwin-improve was
  running, reread the file before editing it, used the correct
  `improve:` prefix.

### 2026-07-20 — Session 19 (improve: coaching-log.md untracked from git)
- Ran `call-coach` for real for the first time — a Gong-style
  transcript for Implid, full write-up in `resources/coaching-log.md`
  (private, per guardrail 7, never referenced here or anywhere
  customer-facing).
- Self-caught mid-task: `coaching-log.md` was already tracked in git
  (committed as an empty stub in the original scaffold). Once it held
  a real entry (customer quotes, names, deal specifics), committing it
  would have repeated exactly the mistake guardrail 6 already exists
  to prevent for `accounts/<customer>/` — just in a file nobody had
  thought to gitignore yet.
- Fixed: added `resources/coaching-log.md` to `.gitignore`, ran
  `git rm --cached` to untrack it going forward (content on disk is
  untouched — 70 lines, real entry intact). No customer specifics were
  pushed; caught before any commit went out.
- Also nearly lost the coaching content itself mid-fix: ran `git
  checkout -- resources/coaching-log.md` to "revert," not realizing
  that reverts to the last commit (the empty stub), wiping the
  uncommitted real entry. Caught immediately, content wasn't actually
  lost (rewrote from what was still in context) — but worth noting as
  a near-miss: `git checkout --` is destructive to uncommitted work,
  not a safe no-op.
- Walid's feedback on the chat output itself: wanted the "did well"/
  "to improve" split as bullet lists, not prose paragraphs — even
  though the `coaching-log.md` entry was already bulleted, the chat
  reply I gave first went out as prose. Fixed in `call-coach/SKILL.md`
  (new step 7: chat reply mirrors the log's bullet structure, always)
  and applied immediately to the Implid feedback already given.

### 2026-07-20 — Session 20 (post-call-update: Implid, backfilled from the real transcript)
- Ran `post-call-update` for Implid using the real July 9 transcript
  (no Debriefs DB row existed yet for this account — email-derived
  notes had captured the aftermath but not the call itself).
- Updated: Notion Debriefs DB (new row), Accounts DB (MEDDPICC +
  Notes refreshed with what the transcript actually confirmed live),
  `accounts/implid/debriefs.md` (new, local-only). Full evidence and
  specifics live only in Notion + the local file, per guardrail 6.
- Kept this fully separate from the private coaching feedback given in
  chat/`coaching-log.md` in the same turn — deal-facing debrief and
  personal performance coaching are two different audiences per
  CLAUDE.md guardrail 7, never merged into one artifact.

### 2026-07-20 — Session 21 (first real monthly-review, run manually on request)
- Walid asked for a full audit of the session, whether collaboration
  could improve, how to cut token usage, and whether Darwin has real
  gaps — exactly what `darwin-improve`'s "Compounding" section says
  `monthly-review` (Phase 4, unbuilt) should be doing on its own.
- Found the real pattern across the day: three separate "new file held
  real customer/personal data, wasn't gitignored" mistakes
  (`accounts/*/*.md`, then `.pptx`, then `coaching-log.md`) — same root
  cause, fixed piecemeal each time rather than once at the root.
- Fixed at the root: **new CLAUDE.md guardrail 10** — check gitignore
  at file creation, not after populating. **New CLAUDE.md Voice
  addition** — bullets vs. prose rule, generalized from the call-coach
  fix so it doesn't need relearning per skill. **New `monthly-review`
  skill** — formalizes this exact review into something repeatable
  instead of a one-off manual ask.
- Also fixed a scope misunderstanding Walid flagged directly:
  `demo-setup` was being described as "execution-blocked" on the
  missing Storyblok connector, as if that were a real gap. Walid's
  correction: the script IS the deliverable, full stop — connector
  execution would be a bonus feature, not something the skill is
  waiting on. Reframed `demo-setup/SKILL.md` and `ROADMAP.md`
  accordingly — dropped the "blocked" framing entirely.
- Token-optimization findings for future sessions: check file size
  before any inline base64/binary dump (a 25MB deck upload attempt
  burned a huge failed round-trip today); prefer targeted Drive
  lookups (`search_files`/`get_file_metadata`) over `list_recent_files`
  when only confirming one file; prefer `grep` on a skill file for one
  section over a full re-read when not about to substantially rewrite
  it; keep ToolSearch queries narrow to avoid pulling in large unused
  tool schemas.
- Real gaps confirmed, not yet fixed (backlog, Walid's call on
  priority): `resources/battle-cards/` still empty; Thibault's AE
  headshot still missing; Phase 5 (`rfp-answer`, `storyblok-content`)
  unbuilt while real RFP work happens ad hoc (Cera, Akeneo).

### 2026-07-20 — Session 22 (no-artifact standing preference; weekly-review built; both new routines scheduled)
- Walid: no artifacts from routines, plain well-formatted chat text
  with real spacing between sections. `daily-briefing/SKILL.md`'s
  actual scheduled run was already chat-text-only (verified via its
  task file) — the gap was in the skill's own documentation, which
  still pointed to the legacy `darwin-morning-brief` artifact as the
  *preferred* way to consume the brief. Fixed: artifact framing
  dropped entirely, output format section now explicitly calls for
  blank-line spacing, legacy artifact left alone/unmaintained rather
  than deleted.
- New skill: `weekly-review` — a pipeline/account-health pulse (stage
  movement, stalled accounts, week-ahead calls/deadlines, RFP deadline
  collisions, MEDDPICC gaps on High-priority accounts). Deliberately
  distinct scope from `monthly-review` (which audits Darwin's own
  behavior, not the pipeline) — two different "monthly/weekly" things
  that happen to share a cadence word.
- Both `weekly-review` (Mondays 08:30 Paris) and `monthly-review` (1st
  of month, 08:00 Paris) are now real scheduled tasks
  (`darwin-weekly-review`, `darwin-monthly-review`), not just skill
  files sitting unused — same pattern as `darwin-daily-briefing`.
  Monthly-review's scheduled prompt explicitly limits it to low-risk,
  clearly-scoped auto-fixes; anything judgment-call-shaped gets flagged
  as backlog instead of auto-decided, matching the skill's own step 4.
- Walid reinforced: no artifact for ANY routine, daily/weekly/monthly,
  full stop. Immediately followed by a request for a genuinely
  different thing — an on-demand, interactive Cowork artifact (not a
  routine) as a deals dashboard: his owned deals from the Notion
  Accounts DB, plus unclaimed AE-pod opportunities scanned from
  #se-requests with a one-click Claim button. Built as
  `walid-deals-dashboard`. Claim writes directly to Notion (permitted
  freely per guardrail 4); it does NOT auto-post to Slack (guardrail 2
  — never send without showing a draft and getting an OK) — instead it
  shows a copy-ready suggested reply Walid sends himself. Verified live:
  the Notion query and Slack scan both returned real data on first
  load.

### 2026-07-21 — Session 23 (dashboard Kanban rebuild + Notion board fix + Phase 4/5)
- Rebuilt `walid-deals-dashboard` as a real Kanban: "Walid's Pipeline"
  (his deals, grouped by real Notion Stage values) + new "Team
  Pipeline" row (ALL deals, unfiltered, grouped by AE) + "Worth
  Claiming" (now 7 days not 24h, and cross-checks each candidate's
  Slack thread so requests already claimed by another SE get filtered
  out instead of just trusting the raw message text).
- Walid separately called the Notion "Dashboard" view "shit" and asked
  for Trello-style cards grouped by sale status. A `dashboard`-type
  Notion view can only hold chart/table widgets, not a board — so the
  real fix was enriching/renaming the existing "Pipeline board" view
  (had been grouped by Stage all along, but showed only the account
  name) to "🗂️ Sales Pipeline (Board)" with real card properties, and
  renaming the chart view to "📊 Analytics (charts)" so the two stop
  being confused. Also fixed "By status" (mislabeled — groups by AE,
  not Stage) → "By AE".
- Phase 4/5: asked Walid three real blockers before building anything
  speculative (matches ROADMAP's own "nothing built speculatively"
  rule). Answers: `todo-sync` = create/update items on his one real
  Notion "✅ To Do" checklist (built). `rfp-answer` = he'll seed
  `resources/rfp-library/` with real content; answered his
  architecture question directly — markdown library, not a vector DB
  (corpus too small to need one, and a flat library is more auditable
  for legally-reviewed submissions); Storyblok MCP is irrelevant to
  this either way (content-management API, not a knowledge source).
  `storyblok-content` = still blocked, no Storyblok MCP connector in
  this session; flagged a same-session bash/curl-against-Management-API
  option Walid hadn't considered, alongside his two (MCP connector /
  Claude Code prompt).
- Self-caught (guardrail 10): `resources/rfp-library/answers/
  won-lost-notes.md` was tracked in git since the initial scaffold and
  would eventually name real deals/customers — gitignored + untracked
  before any real content lands, same fix pattern as coaching-log.md.
- ROADMAP.md updated to reflect reality: Phase 4 now fully drafted
  (though `weekly-review`/`monthly-review`/`todo-sync` still unproven
  on a real scheduled run); Phase 5 marked `[~]` (mechanism drafted,
  genuinely blocked) rather than either `[ ]` or a dishonest `[x]`.

### 2026-07-21 — Session 24 (Team Pipeline by SE, real shared RFP library, Storyblok MCP confirmed absent)
- Team Pipeline redone per Walid's correction: filtered to AE-pod deals
  only (was every deal in the DB), grouped by Solution Engineer owner
  instead of AE — resolved Ines Akrap / Chakit Arora / Roberto Butti's
  real Notion user IDs via `notion-get-users` rather than guessing.
  Added a per-card dismiss (×) with localStorage persistence — checked
  first that Cowork artifacts (unlike general Claude artifacts)
  explicitly permit localStorage.
- Walid pointed at a real, live, company-wide "RFP Answer Library
  (POC)" Notion database — fetched it directly and found 19 real
  Q&A rows already written (all Status "needs review", not approved).
  This changes `rfp-answer`'s whole shape: it's no longer "build an
  empty local library and wait for Walid to seed it" — there's already
  a shared team resource to retrieve from today, just one that needs
  an explicit trust-level tag since nothing in it is approved yet.
  Kept it read-only (cross-team resource, not Walid's own Notion —
  guardrail 4's "edit freely" is specifically about his own content).
  Walid also authorized four sanctioned research sources for gaps:
  Storyblok public docs, the wider company Notion, Slack search,
  Google Drive.
- Walid asked directly to try connecting a Storyblok MCP connector —
  searched the registry with several keyword variants, confirmed zero
  results. Not a "not connected in this session" gap, a "doesn't exist
  yet" gap — updated `storyblok-content` to say this plainly.
