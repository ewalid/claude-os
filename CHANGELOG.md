# CHANGELOG

All notable changes Darwin makes to itself, this repo, or Walid's connected
tools (Notion, scheduled tasks, artifacts) are logged here — newest first.
Session-level narrative detail still lives in `memory.md`; this file is the
short, skimmable "what changed and when" record. Update it alongside any
`improve:` commit.

## 2026-07-21 (Team Pipeline redo by SE, real RFP library found, Storyblok MCP confirmed nonexistent)
- Team Pipeline row rebuilt: filtered to deals from the AE pod only
  (was unfiltered), grouped by Solution Engineer owner (Walid, Ines
  Akrap, Chakit Arora, Roberto Butti, Unassigned — real Notion user
  IDs resolved via `notion-get-users`, not guessed) instead of by AE.
  Cards now have a dismiss (×) button, persisted via localStorage
  (Cowork artifacts explicitly support this, unlike general Claude
  artifacts) with a "show hidden" undo link.
- Found a real, live, company-wide "RFP Answer Library (POC)" Notion
  database (19 real Q&A rows, all Status "needs review") that someone
  else already started. `rfp-answer` rewritten to use it as the
  primary shared source (read-only — it's a cross-team resource, not
  Walid's own Notion) layered under Walid's own local library (still
  empty, still highest-trust once seeded) and above four newly
  -sanctioned research sources: Storyblok public docs, the wider
  company Notion, Slack search, Google Drive. Every answer now carries
  an explicit trust-level tag (validated / needs review / research /
  needs SME input) — never smoothed over.
- Searched the MCP registry directly for a Storyblok connector — none
  exists, in this session or at all. `storyblok-content` updated to
  say so plainly rather than "not connected yet."

## 2026-07-21 (Phase 4 finished; Phase 5 drafted and honestly flagged as blocked)
- `todo-sync` skill built: reconciles personal action items onto the
  single real "✅ To Do" checklist block on Walid's Notion space page.
  Deliberately doesn't touch account-specific next-steps.
- Dashboard artifact gained a "Team Pipeline" row (all deals, grouped
  by AE, unfiltered) alongside the renamed "Walid's Pipeline" row.
- Worth Claiming widened from 24h to 7 days (with pagination), and now
  reads each candidate's Slack thread to filter out requests already
  claimed by another SE instead of trusting the raw message text alone.
- `rfp-answer` mechanism drafted (RETRIEVE + HARVEST against a markdown
  library under `resources/rfp-library/`, explicitly not a vector DB —
  documented why). Not live: the library is still empty, blocked on
  Walid seeding real content. `won-lost-notes.md` gitignored + untracked
  (guardrail 10 — it will name real deals/customers).
- `storyblok-content` mechanism drafted (dry-run → confirm → write →
  verify). Blocked on an execution path — no Storyblok MCP connector
  in this session. Flagged three options, including a same-session
  bash/curl path against Storyblok's Management API if Walid has a
  personal access token.
- Notion "Dashboard" (chart view) renamed "📊 Analytics (charts)" and
  "Pipeline board" renamed + enriched to "🗂️ Sales Pipeline (Board)"
  after Walid called the existing Notion dashboard "shit" — a
  `dashboard`-type Notion view can only hold chart/table widgets, not
  a Kanban board, so the real fix lived in the board view, not the
  dashboard view. Also fixed "By status" (was mislabeled — groups by
  AE, not Stage) → "By AE".

## 2026-07-20 (Kanban rebuild — Cowork artifact + Notion board view)
- `walid-deals-dashboard` artifact rebuilt as an actual Kanban board:
  deals grouped into columns by real Notion Stage values (Not started /
  Discovery / Demo / Deep Dive-Demo / Proposal / In progress /
  Contracting / Done / Closed Won). Any owned deal with no Stage set is
  flagged in an error banner rather than silently dropped.
- Notion "Pipeline board" view renamed to "🗂️ Sales Pipeline (Board)"
  and enriched with real card properties (Priority, Type, AE, Next
  call, Deadline) — previously showed only the account name.
- Notion "Dashboard" (chart-based) view renamed to "📊 Analytics
  (charts)" to stop it being confused with the new card board — a
  `dashboard`-type Notion view can only hold chart/table widgets, not a
  Kanban board, so the two stay separate views.
- Notion "By status" view renamed to "By AE" — it groups by AE, not
  Stage; the old name was actively misleading.

## 2026-07-20 (deals dashboard artifact: 24h Slack window + visual redesign)
- `walid-deals-dashboard` artifact: "Worth Claiming" Slack scan now windowed
  to the last 24h (`oldest` param) instead of a flat 40-message pull.
- Same artifact: full visual redesign — gradient hero with live stat tiles,
  priority-colored card accents, pill-style tags, hover/shadow polish.
  Functional behavior unchanged (Notion write on Claim, Slack reply is
  copy-only, never auto-sent).

## 2026-07-20 (build-deck first run: Jouet club deck; gitignore gap fixed)

- **First real `build-deck` run** — French-language demo deck for Joué
  Club / La Grande Récré, adapted from the Cellbes AB example, saved to
  `accounts/jouet-club/2026-07-20_Demo_JouetClub.pptx`. 3 demo stations
  picked from `style-guide.md`'s real theme vocabulary (not invented),
  translated to French; leftover scaffolding slides excluded.
- **`improve: broaden accounts/ gitignore to all files, not just .md`**
  — `.gitignore` rule `accounts/*/*.md` didn't cover the new `.pptx`
  deck. Changed to `accounts/*/*` + `!accounts/*/.gitkeep` so the whole
  per-account directory stays local-only, matching CLAUDE.md guardrail
  6's actual intent.

## 2026-07-20 (feat: deals dashboard artifact — Notion + Slack, claim-in-one-click)

- New Cowork artifact `walid-deals-dashboard`: "My Deals" (live Notion
  Accounts DB query) + "Worth Claiming" (Slack #se-requests scanned and
  AI-triaged for unclaimed AE-pod opportunities). Claim button creates
  the Notion row directly; shows a copy-ready Slack reply rather than
  auto-sending (guardrail 2). Explicitly NOT a routine — an on-demand
  tool, per Walid's own distinction.

## 2026-07-20 (feat: weekly-review built; weekly + monthly routines scheduled; no-artifact preference)

- **Standing preference**: no artifacts from routines — plain chat
  text, blank line between sections. `daily-briefing/SKILL.md` fixed
  (its scheduled run was already compliant; its docs weren't).
- **New skill: `weekly-review`** — pipeline/account-health pulse,
  distinct from `monthly-review`'s self-audit scope.
- **Both `weekly-review` and `monthly-review` are now real scheduled
  tasks** (`darwin-weekly-review` Mondays 08:30, `darwin-monthly-review`
  1st of month 08:00, Paris time) — not just skill files.

## 2026-07-20 (improve: root-cause fix for the recurring gitignore pattern, new monthly-review skill, demo-setup reframed)

- **New CLAUDE.md guardrail 10**: check `.gitignore` at file creation
  for anything under `resources/`/`accounts/` that could hold real
  customer/personal data — fixes the root cause behind three separate
  same-day incidents (`accounts/*/*.md`, a `.pptx`, `coaching-log.md`).
- **New CLAUDE.md Voice rule**: bullets vs. prose, generalized from the
  `call-coach` fix so the preference doesn't need relearning per skill.
- **New skill: `monthly-review`** — formalizes pattern-mining across
  `memory.md`/`CHANGELOG.md` (previously only theorized in
  `darwin-improve`'s "Compounding" section, never actually built).
- **`demo-setup/SKILL.md` + `ROADMAP.md` reframed**: dropped the
  "execution-blocked" framing entirely — the written setup script is
  the deliverable, connector execution is a future bonus, not a
  dependency. Correction from Walid.

## 2026-07-20 (post-call-update: Implid backfilled from real transcript)

- Notion Debriefs DB: new row for Implid's July 9 call. Notion Accounts
  DB: MEDDPICC + Notes refreshed with what the call itself confirmed.
  `accounts/implid/debriefs.md` created (local-only). Full evidence
  stays in Notion/local file per guardrail 6 — not repeated here.

## 2026-07-20 (improve: call-coach chat output always mirrors bulleted log format)

- **`call-coach/SKILL.md`** new step 7: the chat reply always uses the
  same two-bullet-list structure ("did well" / "to improve") as the
  `coaching-log.md` entry — first real run (Implid) went out as prose
  in chat despite the log entry already being bulleted.

## 2026-07-20 (improve: untrack coaching-log.md from git)

- **`resources/coaching-log.md`** was tracked in git since the initial
  scaffold (as an empty stub) — once `call-coach` wrote a real entry
  into it (Implid), that would have pushed customer quotes/names to
  GitHub, the same risk guardrail 6 exists to prevent for
  `accounts/<customer>/`. Added to `.gitignore`, untracked via
  `git rm --cached` before any commit went out. Content on disk
  unaffected.

## 2026-07-20 (improve: darwin-improve triggers more reliably, not just from memory)

- **CLAUDE.md "How I improve"** rewritten: concrete trigger signals
  (correction, stated preference, self-caught mistake, a reflection
  question whose honest answer reveals a gap) instead of relying on
  the literal "Darwin, learn this" phrase; explicit instruction to
  reread `darwin-improve/SKILL.md` before acting instead of running it
  from memory; no exceptions to the `improve:` commit prefix.
- **`darwin-improve/SKILL.md`**: new step 1 ("notice the trigger — no
  auto-routing exists, I have to catch it myself"), step 2 now says to
  reread the file's own steps before executing.
- Root cause being fixed: this session alone had a real gap between
  "applying the substance" and "actually running the named procedure
  correctly" (two commits went out as `fix:` instead of `improve:`).

## 2026-07-20 (improve: permanent guard against the overwrite mistake recurring)

- **New CLAUDE.md guardrail 9**: before any bash/python write to a
  path the Edit tool refuses (currently `.claude/skills/`), manually
  `cat`/`git log --oneline -- <path>` first — Edit enforces
  read-before-write automatically, bash/python doesn't.
- `darwin-improve/SKILL.md` step 3 now documents this concrete failure
  mode and procedure directly, with the real example that caused it.
- Also self-noting: this is the first fix today actually committed
  with the skill's own specified `improve:` prefix — the two before it
  (headshots, the restore itself) went out as `fix:` instead.

## 2026-07-20 (fix: restored overwritten demo-script/demo-setup content)

- **Self-caught mistake**: Session 14's "formalize demo-script/demo-
  setup as skills" actually overwrote real pre-existing Phase 3
  content (commit `80ee6e8`, 2026-07-15) without reading it first —
  lost the battle-cards-sourced objections section + verify-before-call
  checklist in `demo-script`, and replaced `demo-setup`'s guarded
  dry-run-preview-OK-execute-verify automation identity (built around
  CLAUDE.md guardrail 3) with a plain manual checklist.
- **`improve: merge restored content with Session 14's real
  improvement`** — both skills now have their original
  structure/identity back, plus the one thing Session 14 actually got
  right: explicit confirm-before-finalizing gates.
- Open: the Jouet club account's `demo-script.md`/`demo-setup.md`
  (delivered in Session 14) don't match the restored structure —
  not regenerated yet, Walid's call.

## 2026-07-20 (fix: title-slide AE/SE headshots; new photo resource)

- **New resource**: `resources/AEs & SEs/<Full Name>.jpeg` — headshots
  for use on deck title slides. Walid added his own; others still
  missing.
- **Bug fix**: all 5 example decks carry 2 headshot PICTURE shapes on
  slide 1 (AE + SE), invisible to a text-only shape scan. The Jouet
  club deck had shipped with the original Cellbes AE/SE's photos still
  in place. Fixed Walid's photo in place; Thibault's is still wrong
  (no headshot on file for him yet) — flagged, not silently left.
- `style-guide.md` and `build-deck/SKILL.md` updated to call out the
  headshot swap explicitly going forward.

## 2026-07-20 (improve: confirmation checkpoints added to demo-prep skills)

- **`improve: build-deck confirms demo station shortlist before building`**
  — added an explicit gate to `build-deck/SKILL.md`, mirroring the
  existing Commercial-section confirmation.
- **New skills: `demo-script/SKILL.md`, `demo-setup/SKILL.md`** —
  formalized from this session's first real runs (Jouet club). Each
  has a built-in confirmation checkpoint before finalizing (script:
  confirm outline/framing; setup: flag space-structure decisions).
- Reasoning logged in `memory.md` Session 14: fix applied at the skill
  level, not CLAUDE.md, to avoid contradicting the existing Notion
  guardrail (no proposal step there — that's evidence-based, this is
  judgment-based).

## 2026-07-20 (Gmail added to daily-briefing; Implid + Winfarm Group updated from email; Akeneo deadline confirmed)

- **New connector wired in: Gmail** (walid.elmselmi@storyblok.com).
  `daily-briefing/SKILL.md` now scans the inbox as a fourth source
  alongside Calendar/Notion/Slack, same last-working-day window as
  Slack, read-only (CLAUDE.md guardrail 1 — never draft/send — restated
  in the skill itself). Output format gains a "📧 EMAIL" section. Not
  yet wired into the live `darwin-morning-brief` artifact.
- **Akeneo RFP**: Walid confirmed the submission deadline — **2026-07-31**
  (same date as Cera RFP). Set on the Notion row.
- **Implid**: Stage "Proposal" → **"Contracting"**, AE confirmed Thibault
  de Maison Rouge, Deadline cleared (the old date was for sending the
  proposal, which is done — deal is now signature-pending, not overdue).
  Real detail in `accounts/implid/notes.md` (local-only).
- **Winfarm Group**: previously a near-empty stub row. Email surfaced
  Walid as the confirmed SE with a custom demo on 2026-07-29. Notion row
  updated: AE Thibault de Maison Rouge, Owner(s) Walid, Stage "Demo",
  Next call 2026-07-29. Real detail in `accounts/winfarm-group/notes.md`
  (local-only).

## 2026-07-20 (process-customer: renamed Phase A-E to explicit step names)

- **`process-customer/SKILL.md` phases renamed** for clarity: Phase A → Step 1 (Self-research), B → Step 2 (Ask Walid, hard stop), C → Step 3 (Analyze), D → Step 4 (Write the brief), E → Step 5 (Update Notion and announce). No behavior change, just clearer naming.

## 2026-07-20 (Akeneo RFP claimed — Notion row from a parallel session)

- **New Notion row: "Akeneo RFP"** — created in a different session
  (same repo) after Walid claimed it live off a briefing flag. Repurposed
  a blank template stub row. Deadline **2026-07-31** — same date as Cera
  RFP, plus Implid still overdue since July 17 — three accounts stacking
  up end-of-month. Real evidence moved to `accounts/akeneo/notes.md`
  (local-only); that session's memory.md entry had customer specifics
  inline, trimmed retroactively to meta-level here.

## 2026-07-20 (process-customer: added a "Demo — what to show" section)

- **`process-customer/SKILL.md` template gains a "Demo — what to show"
  section** in Phase D, between "Their priorities" and "Stakeholders" —
  concrete use cases tied explicitly to real pain points/decision
  criteria/MEDDPICC gaps (not a generic feature tour), split by occasion
  when there are calls at different depths, with mock-data caveats
  flagged rather than assumed. Applied to the Jouet club brief
  (local-only) and pasted into its Notion page too.

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

## 2026-07-21 (two corrections: RFP trust order, Team Pipeline silent drop)
- `rfp-answer`: Walid confirmed the shared "RFP Answer Library (POC)"
  Notion DB is unofficial (a coworker built it on their own) — official
  Storyblok docs now rank above it in the trust order, and the two
  disagreeing means the docs win, not the POC library.
- Team Pipeline was silently dropping any deal with a blank/non-pod AE
  (Cera RFP's AE is genuinely null in Notion — confirmed by query, not
  a fetch bug) because the SQL filter excluded it outright. Fixed:
  fetches all deals, filters in JS, and flags excluded deals by name +
  their actual AE value in an error banner instead of vanishing them.
