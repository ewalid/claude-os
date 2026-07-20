# CHANGELOG

All notable changes Darwin makes to itself, this repo, or Walid's connected
tools (Notion, scheduled tasks, artifacts) are logged here — newest first.
Session-level narrative detail still lives in `memory.md`; this file is the
short, skimmable "what changed and when" record. Update it alongside any
`improve:` commit.

## 2026-07-20 (process-customer: first real run + quality upgrade, Jouet club)

- **First real `process-customer` run** — `accounts/jouet-club/brief.md`
  written from real Salesforce + Gong extracts, Notion row rewritten
  (Stage, Notes, MEDDPICC). Customer specifics live only in that
  local-only file — not detailed here, per the guardrail 6 change below.
- **Policy fix (Walid's correction): customer data must be git-ignored,
  not just under-shared in chat.** `accounts/jouet-club/brief.md` was
  untracked before ever being pushed; `accounts/*/*.md` added to
  `.gitignore`. CLAUDE.md guardrail 6 updated to codify: real evidence
  lives only in `accounts/<customer>/` (local-only), tracked files
  (this one included) stay meta-level.
- **`process-customer/SKILL.md` upgraded**: Phase B now explicitly asks
  for Salesforce paste + Gong paste + a customer-specific Slack channel
  ID (three concrete asks, not "whatever you have"). Phase D's brief
  template restructured: prospect intro up front, an explicit meeting-
  attendees section, a much bigger MEDDPICC section (what wins/loses the
  deal + Storyblok's current positioning, not just tags), and a concrete
  next-steps action checklist (deck/demo-script/space setup) instead of a
  vague verify-before-call list.
- **Notion — "Pipeline board" view added** on the Accounts DB (Kanban
  grouped by Stage), alongside the existing table. Jouet club's own
  Notion page now also has the (upgraded) brief pasted into its page
  content plus a next-steps checklist, not just the property fields.

## 2026-07-20 (account-switch recovery: git, scheduler, artifact, GitHub connector)

- **Git repo reconciled with the real GitHub history.** `~/dev/claude-os`
  had no local `.git` despite this file/`memory.md` claiming a pushed
  repo — turned out the remote (`ewalid/claude-os`, branch `main`, 11
  commits) is real, just orphaned from a different Claude account on this
  same Mac. Linked local `main` to `origin/main` via `git reset --soft`
  after clearing a stale `HEAD.lock` the sandbox couldn't self-delete
  (fixed with the `allow_cowork_file_delete` permission tool). Restored
  `README.md` from origin, took origin's slightly newer wording for two
  `memory.md`/`CHANGELOG.md` recap bullets, and — per Walid's decision —
  gitignored `resources/deck-examples/*.pptx` (~120MB, kept local-only,
  never pushed). Committed as `7f611e5`, one commit ahead of `origin/main`,
  pending push once the GitHub connector's tools are confirmed reachable.
- **Scheduled task and live artifact recreated** under this Claude account
  — both existed for real under a different account on the same machine
  (confirmed via `~/Claude/Scheduled` existing as a protected folder, and
  Cowork refusing to reuse the `darwin-daily-briefing` artifact id) but
  were invisible/unusable from this login. New scheduled task:
  `darwin-daily-briefing` (weekdays ~9am Paris). New artifact id (the old
  one is locked): `darwin-morning-brief` — verified live against Calendar,
  the Notion Accounts DB, and both Slack channels. Updated
  `daily-briefing/SKILL.md` to point at the new id.
- **GitHub MCP connector connected** (`https://api.githubcopilot.com/mcp/`,
  OAuth, no PAT) via Settings → Connectors — shows Connected, but tools
  hadn't loaded into the running session yet as of this entry (known
  Cowork limitation: needs a fresh session). Confirmed no GitHub MCP tool
  exists anywhere else in this workspace.

## 2026-07-15 (process-customer: full-row rewrite)

- **`process-customer` now rewrites the ENTIRE Notion Accounts DB row**
  (Stage, Priority, Notes, AE, Next call, Deadline, MEDDPICC), not just
  MEDDPICC — from real Gong/Salesforce/Slack evidence Walid pastes in.
  Walid's own instruction: his manual notes aren't the source of truth,
  the agent's analysis of real evidence is. Removed the "propose diff,
  wait for OK" gate for Stage/Priority/AE/dates and the downgrade-gate
  on MEDDPICC — all of it now writes directly and gets announced after
  the fact (old value → new value → evidence), per CLAUDE.md guardrail
  4, which already granted free Notion editing; the skill had been
  over-restricting itself relative to that standing rule. Clarified
  guardrail 4 itself to spell out "the whole row, not just one column."
  The only limits that still apply: never write a field with no real
  evidence behind it (stays as-is / need validation), and never touch
  human-only columns.

## 2026-07-15 (process-customer upgrade)

- **`process-customer` now does a real MEDDPICC analysis and writes it
  to Notion.** New Phase C: works through all 8 MEDDPICC elements
  against whatever Salesforce/Gong extracts Walid pastes in (plus
  Slack/Debriefs/calendar context), tagging each Confirmed (quoted
  evidence + source) / Partial (signal but incomplete) / Gap (nothing).
  Only Confirmed elements get written to the Notion MEDDPICC
  multi-select — done automatically per CLAUDE.md guardrail 4 (edit
  Notion freely, announce at the end), except a *downgrade* of a
  previously-confirmed element, which still needs Walid's OK first.
  The brief's MEDDPICC section now shows the full picture (Confirmed/
  Partial/Gap for all 8), not just a checklist of what's in Notion.

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

## 2026-07-15 (Phase 2)

- **`darwin-improve` skill drafted** — formalizes CLAUDE.md's "how I
  improve" loop into a concrete procedure: identify friction ->
  classify (CLAUDE.md / a skill's SKILL.md / resources/ / accounts/ /
  a live artifact) -> apply everywhere it touches in one pass -> commit
  as `improve: ...` -> log in CHANGELOG.md -> update memory.md. Uses
  today's people-model correction as its worked example.
- **`post-call-update` skill drafted** — trigger "debrief [account]":
  writes a new row to the Debriefs DB, updates the Accounts DB row's
  Stage/Next call/Deadline/MEDDPICC, appends to
  `accounts/<customer>/debriefs.md`, updates memory.md. Never fabricates
  outcomes — asks Walid if his trigger message doesn't already state
  what happened.
- **`call-coach` skill drafted** — coaches from a pasted Gong transcript
  or notes (Gong not connected, never claims otherwise): 3-5 things
  done well + 3-5 to improve, each quoting an actual moment, critiqued
  against the call's stated goal (from `accounts/<customer>/brief.md`),
  one focus for next call. Feeds private `resources/coaching-log.md` —
  never surfaced anywhere customer-facing.
- **Phase 2 of ROADMAP.md now complete** (process-customer, call-coach,
  post-call-update, darwin-improve all drafted). Phase 3 (demo pack) is
  next, but blocked on Storyblok MCP reachability and example decks in
  `resources/deck-examples/`.

## 2026-07-15 (Phase 3)

- **`resources/deck-examples/` populated** — Walid dropped 5 real demo
  decks (Stokke, fashioncheque, Payabl, Cellbes AB, Orange Cyberdefense).
  Reverse-engineered the shared template across all 5 and documented it
  in `style-guide.md`: title → "What we know so far" discovery recap →
  themed demo-stations overview → per-station transition/blank-Demo/
  key-takeaways triptych → optional Technical Topics → optional
  commercial/pricing section → Thank You close.
- **`build-deck` skill drafted** — adapts the closest-fitting example
  deck per account brief; never invents a slide type outside the
  documented template; demo slides always stay blank (that's
  `demo-script`'s job, in a separate doc).
- **`demo-script` skill drafted** — Tell-Show-Tell per demo station,
  objections sourced from `resources/battle-cards/` (currently empty —
  flagged), verify-before-call checklist. Chains off `build-deck` and
  the account brief.
- **`demo-setup` skill drafted but blocked** — fully specced (dry-run
  preview → OK → write → verify, per CLAUDE.md guardrail 3), but the
  Storyblok MCP connector is still absent from this Cowork session as
  of 2026-07-15 (re-verified, same result as the original HANDOFF.md
  flag). Will not run live until that connector appears.
- **Phase 3 of ROADMAP.md is now mostly complete** — only `demo-setup`'s
  live execution remains blocked on the Storyblok connector. Phase 4
  (`weekly-review`, `monthly-review`, `todo-sync`, `dashboard`) is next.

## Known gaps / carried forward

- Notion "Dashboard" view has no charts yet (API limitation hit today).
- Implid and Cera RFP have no confirmed AE — need validation from Walid.
- MEDDPICC not yet populated for any account — needs Walid's input per
  deal, or for `process-customer` to gather real Gong/SF extracts first.
- `call-coach`, `post-call-update`, `build-deck`, `demo-script` are all
  drafted but unused — first real runs will surface whatever the specs
  got wrong.
- `demo-setup` cannot run live — Storyblok MCP connector still absent.
- `resources/battle-cards/` is empty — `demo-script`'s objection-sourcing
  step has nothing to draw on yet.
- No `accounts/<customer>/brief.md` exists yet for any account —
  `process-customer` hasn't been run for real; `build-deck`/`demo-script`
  need one to chain off.
- `process-customer`'s full-row rewrite (Stage/Priority/Notes/AE/dates/
  MEDDPICC) hasn't had a real run yet — no account has real Gong/SF
  extracts pasted in.
- Scheduled `darwin-daily-briefing` task's prompt still describes chat-
  text output; it hasn't been updated to point at the live artifact.
