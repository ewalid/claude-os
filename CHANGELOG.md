# CHANGELOG

All notable changes Darwin makes to itself, this repo, or Walid's connected
tools (Notion, scheduled tasks, artifacts) are logged here — newest first.
Session-level narrative detail lives in `memory.md` (local-only, git-ignored);
this file is the short, skimmable, **name-free** "what changed and when" record
that is safe to share with colleagues. Update it alongside any `improve:` commit.

Convention (guardrail 6): this file is tracked in git and MUST contain zero real
client/account names or deal figures — accounts are referred to generically
("an RFP deal", "an FR demo account"). Colleague names (the AE/SE pod) are fine.

## 2026-07-21 (Darwin's namesake, a signature)
- Walid: Darwin is named after his cat — a black cat with pointy ears
  and huge round eyes, "the sweetest cat ever." New CLAUDE.md
  "Signature" section: a small ASCII cat that opens `darwin-setup`'s
  introduction and every "routine" skill's chat output (`daily-briefing`,
  `weekly-review`, `monthly-review`), always as the very first thing.
  Walid dropped the real photo in at the repo root; moved it to
  `resources/darwin.jpg` and it's now shown at the top of `README.md`
  above the ASCII mascot.

## 2026-07-21 (repo made shareable: Gong MCP wired in conditionally, no client names in git)
- Gong support made conditional across the call-consuming skills
  (`call-coach`, `post-call-update`, `process-customer`) and CLAUDE.md
  guardrail 5: if a Gong MCP connector is present in the session, pull
  transcripts directly (discover the actual tool names at runtime — they
  vary by connector version, never assume); if not, fall back to Walid
  pasting transcripts, exactly as before. `darwin-setup` now tests/asks
  for Gong alongside the other connectors. (At the time of writing, no
  Gong MCP tools were actually resolvable in-session — the wiring is
  forward-compatible, same pattern as storyblok-content.)
- **Repo scrubbed of all real client/account names** so colleagues can
  use Darwin: `memory.md` is now git-ignored (local-only, regenerated
  fresh per person by `darwin-setup` — same model as `accounts/`), and
  every tracked file (this CHANGELOG, HANDOFF, ROADMAP, skills, resources,
  CLAUDE.md) was rewritten to generic account descriptors. Guardrail 6
  tightened to make "zero real client names in git" a hard rule, not a
  preference. (Git *history* still contains prior names — rewriting that
  is a separate manual step, noted for Walid.)

## 2026-07-21 (improve: process-customer chat reply must lead with company brief)
- Walid: "Never forget to add a brief about the company" — a real run
  wrote a full "Who they are" section into the local brief.md, but the
  chat reply itself skipped straight to the Notion diff, so Walid had
  to ask "what is this company?" afterward.
- Fixed in `process-customer/SKILL.md` Step 5: new explicit point 2
  (chat reply must lead with the 2-4 sentence company brief before any
  field-level diff), renumbered the rest. Applies to every future run.

## 2026-07-21 (process-customer: first real run)
- Ran `process-customer` on a live RFP deal for the first time — wrote
  its local-only `accounts/<account>/brief.md`. No Gong/Salesforce
  extracts existed (it's an RFP) — all evidence from Slack (a
  deal-specific channel + a bid-function channel + #se-requests + DMs)
  and Drive (the requirements matrices + a colleague's draft response).
- Notion row: Deadline corrected to the real submission deadline
  (the prior date was wrong). Notes rewritten to reflect real progress
  (large draft response already mostly resolved, in human-review).
  MEDDPICC left empty — all 8 elements landed Gap/Partial, none
  Confirmed (expected for a partner-mediated RFP with no direct contact).
- Surfaced two unresolved inconsistencies to Walid rather than
  auto-fixing (a deal-value conflict across two Slack sources, and a
  possible naming confusion in his own shorthand).

## 2026-07-21 (accounts/ folder was leaking customer names into git history)
- Walid asked whether the account folder scaffold was necessary. It
  wasn't: only 3 of the real account folders even had a tracked
  `.gitkeep`, and those paths put real customer names into git history
  via the directory name itself even though contents were empty — a
  guardrail-6 violation. Removed the customer-named paths, replaced with
  a single generic `accounts/.gitkeep`, and tightened `.gitignore` to
  `accounts/*` + `!accounts/.gitkeep` so no future account folder can be
  committed by name.

## 2026-07-21 (new: darwin-setup — portability to a new computer/account)
- Built `darwin-setup`: the interview that rebuilds this repo for
  whoever is actually running Darwin. Wired to fire automatically —
  CLAUDE.md has a "First run" check at the top ("does `memory.md` exist?
  if not, this is an uninitialized copy") that runs `darwin-setup`
  before responding to anything else, introduction first. Covers: who
  the person is, team structure (generalized — doesn't assume the
  AE-pod/SE-colleague shape), which connectors are live (tested, not
  assumed), where the real data lives (Notion DB, Slack channel IDs),
  which guardrails to keep vs. change, voice preferences. Rewrites
  CLAUDE.md Identity/Voice/guardrails, `resources/people.md`, and resets
  `ROADMAP.md`/`memory.md` — doesn't merge with the previous config.

## 2026-07-21 (storyblok-content: token in, blocked by sandbox network allowlist)
- Walid provided a Storyblok Management API personal access token,
  stored gitignored at `storyblok.env` (confirmed ignored before writing
  it in). Test call from this Cowork sandbox to `mapi.storyblok.com` was
  blocked by the sandbox's own network proxy allowlist — a sandbox
  network-access problem, not a token or connector one. Two documented
  ways around it: run from Walid's own terminal (no sandbox), or get
  `storyblok.com` added to the org's network allowlist.

## 2026-07-21 (Slack>Notion rule; Claim button removed; AE schema fixed; pipeline scan rebuilt)
- New standing rule (CLAUDE.md guardrail 8, broadened from a
  Notion-dates-only rule): for Deals, Slack outranks Notion. Notion is
  manually-maintained and can be stale, incomplete, or structurally
  wrong. When they conflict, Slack is evidence to fix Notion, never the
  reverse.
- Dashboard: removed the one-click "Claim" button (auto-wrote to Notion)
  from Worth Claiming cards, replaced with a "View Slack thread" link —
  Walid wants to read the actual thread before any action. Real trigger:
  a request looked claimable from the message text, but the thread showed
  the AE saying he didn't need SE support. Same link added to Team
  Pipeline cards. `claimDeal()` removed as dead code.
- Notion Accounts DB: the "AE" select property only had two configured
  options — most of the AE pod (Rob Scholte, Mine Heck, Kristoffer
  Strindevall) were never selectable values at all. Likely root cause of
  an earlier "why does everything show one AE" symptom. Added the missing
  three, then corrected one deal's AE from blank to the value Walid
  identified directly.
- Pipeline scan rebuilt: Team Pipeline and Worth Claiming were two
  separate batch-AI extractions over the whole #se-requests dump;
  replaced with one unified 7-day scan that parses each message
  deterministically (regex against the Slack bot's regular field format)
  and, only for messages with a thread, runs one small scoped AI call per
  thread asking whether Chakit/Roberto/Ines claimed it. Routing is binary
  per-message: claimed by one of those three -> Team Pipeline, else ->
  Worth Claiming. Claimed-but-untracked deals auto-add to Notion. Caught
  mid-build via `verify_artifact`: `update_artifact` had silently dropped
  `slack_read_thread` from the tool allowlist — always re-verify after
  updating an artifact.
- Team Pipeline row rebuilt: filtered to the AE pod, grouped by SE owner
  (real Notion user IDs resolved via `notion-get-users`) with dismiss (×)
  buttons persisted via localStorage.
- Found a real, live, company-wide "RFP Answer Library (POC)" Notion DB
  (unofficial — a coworker built it) and rewrote `rfp-answer` to use it
  as a read-only source, ranked below official Storyblok docs, above
  general research; every answer carries a trust-level tag.
- Confirmed via the MCP registry there is no Storyblok connector at all;
  `storyblok-content` updated to say so plainly.

## 2026-07-21 (two corrections: RFP trust order, Team Pipeline silent drop)
- `rfp-answer`: the shared POC library is unofficial — official Storyblok
  docs now rank above it, and if they disagree the docs win.
- Team Pipeline was silently dropping any deal with a blank/non-pod AE
  because the SQL filter excluded it. Fixed: fetch all deals, filter in
  JS, flag excluded deals by name + actual AE value in a banner.

## 2026-07-21 (Team Pipeline switched from Notion to Slack #se-requests)
- Per Walid: Team Pipeline no longer reads the Notion Accounts DB (too
  sparse) — it scans #se-requests directly, reads each request's thread
  to determine who claimed it, and groups by claimer.
- Re-added a `colorForName` helper accidentally deleted in an earlier edit.

## 2026-07-21 (fixed dashboard "still loading" hang)
- Root cause: a 30-day Slack scan + sequential per-thread claim-checks
  could take minutes, and a transient Claude API 529 overload was
  silently swallowed as "zero results." Fixed: window trimmed, thread
  checks parallelized and capped, and a `looksLikeApiError()` check now
  surfaces AI-call failures as a visible banner. (Later superseded by the
  full pipeline-scan rebuild above.)

## 2026-07-21 (Phase 4 finished; Phase 5 drafted and honestly flagged as blocked)
- `todo-sync` skill built: reconciles personal action items onto the
  single "✅ To Do" checklist block on Walid's Notion space page.
  Deliberately doesn't touch account-specific next-steps.
- Dashboard gained a "Team Pipeline" row alongside the renamed "Walid's
  Pipeline" row.
- Worth Claiming widened from 24h to 7 days, and now reads each
  candidate's Slack thread to filter out already-claimed requests.
- `rfp-answer` mechanism drafted (RETRIEVE + HARVEST against a markdown
  library, explicitly not a vector DB — documented why). Not live: the
  library is empty. `won-lost-notes.md` gitignored + untracked.
- `storyblok-content` mechanism drafted (dry-run → confirm → write →
  verify). Blocked on an execution path.
- Notion views renamed for clarity: chart view → "📊 Analytics (charts)",
  board view enriched + renamed "🗂️ Sales Pipeline (Board)", "By status"
  → "By AE" (it groups by AE, not Stage).

## 2026-07-20 (Kanban rebuild — Cowork artifact + Notion board view)
- `walid-deals-dashboard` rebuilt as an actual Kanban board: deals
  grouped into columns by real Notion Stage values. Any owned deal with
  no Stage set is flagged in a banner rather than silently dropped.
- Notion board/analytics/By-AE views renamed and enriched (see above).

## 2026-07-20 (deals dashboard artifact: 24h Slack window + visual redesign)
- "Worth Claiming" Slack scan windowed to the last 24h instead of a flat
  message-count pull.
- Full visual redesign — gradient hero with stat tiles, priority-colored
  card accents, pill tags. Behavior unchanged (Notion write on Claim,
  Slack reply copy-only, never auto-sent).

## 2026-07-20 (build-deck first run: FR demo deck; gitignore gap fixed)
- **First real `build-deck` run** — a French-language demo deck for an FR
  retail account, adapted from the closest example deck, saved local-only.
  3 demo stations picked from `style-guide.md`'s real theme vocabulary
  (not invented), translated to French; scaffolding slides excluded.
- **`improve: broaden accounts/ gitignore to all files, not just .md`** —
  the `.gitignore` rule didn't cover a new `.pptx` deck. Changed so the
  whole per-account directory stays local-only.

## 2026-07-20 (feat: deals dashboard artifact — Notion + Slack, claim-in-one-click)
- New Cowork artifact `walid-deals-dashboard`: "My Deals" (live Notion
  query) + "Worth Claiming" (Slack #se-requests AI-triaged for unclaimed
  AE-pod opportunities). Claim button created the Notion row and showed a
  copy-ready Slack reply rather than auto-sending. (Later replaced by the
  thread-link approach.)

## 2026-07-20 (feat: weekly-review built; weekly + monthly routines scheduled; no-artifact preference)
- **Standing preference**: no artifacts from routines — plain chat text.
  `daily-briefing/SKILL.md` docs fixed to match.
- **New skill: `weekly-review`** — pipeline/account-health pulse, distinct
  from `monthly-review`'s self-audit scope.
- **Both `weekly-review` and `monthly-review` scheduled** (Mondays 08:30,
  1st of month 08:00, Paris time).

## 2026-07-20 (improve: root-cause fix for the recurring gitignore pattern; monthly-review; demo-setup reframed)
- **New CLAUDE.md guardrail 10**: `.gitignore` check at file creation for
  anything under `resources/`/`accounts/` that could hold real customer/
  personal data — fixes the root cause behind three same-day incidents.
- **New CLAUDE.md Voice rule**: bullets vs. prose, generalized from the
  `call-coach` fix.
- **New skill: `monthly-review`** — pattern-mining across memory/CHANGELOG.
- **`demo-setup`/`ROADMAP` reframed**: the written setup script is the
  deliverable, connector execution is a future bonus, not a dependency.

## 2026-07-20 (post-call-update: a real deal backfilled from a transcript)
- Notion Debriefs DB: new row for a real call. Accounts DB MEDDPICC +
  Notes refreshed with what the call confirmed. The account's local-only
  `debriefs.md` created. Full evidence stays local per guardrail 6.

## 2026-07-20 (improve: call-coach chat output always mirrors bulleted log format)
- **`call-coach/SKILL.md`** new step 7: the chat reply always uses the
  same two-bullet-list structure as the `coaching-log.md` entry — the
  first real run went out as prose in chat despite the log being bulleted.

## 2026-07-20 (improve: untrack coaching-log.md from git)
- **`resources/coaching-log.md`** was tracked since the initial scaffold;
  once `call-coach` wrote a real entry it would have pushed customer
  quotes to GitHub. Added to `.gitignore`, untracked via `git rm
  --cached`. Content on disk unaffected.

## 2026-07-20 (improve: darwin-improve triggers more reliably)
- **CLAUDE.md "How I improve"** rewritten with concrete trigger signals
  (correction, stated preference, self-caught mistake, reflection
  question) instead of relying on the literal "Darwin, learn this"
  phrase; explicit instruction to reread the skill before acting; no
  exceptions to the `improve:` commit prefix.
- **`darwin-improve/SKILL.md`**: new step 1 (notice the trigger), step 2
  rereads the file's own steps before executing.

## 2026-07-20 (improve: permanent guard against the overwrite mistake recurring)
- **New CLAUDE.md guardrail 9**: before any bash/python write to a path
  the Edit tool refuses (currently `.claude/skills/`), manually
  `cat`/`git log --oneline -- <path>` first.
- `darwin-improve/SKILL.md` step 3 documents this failure mode directly.

## 2026-07-20 (fix: restored overwritten demo-script/demo-setup content)
- **Self-caught mistake**: an earlier "formalize as skills" pass
  overwrote real pre-existing Phase 3 content without reading it first.
  Restored and merged with the one thing the pass got right (explicit
  confirm-before-finalizing gates).

## 2026-07-20 (fix: title-slide AE/SE headshots; new photo resource)
- **New resource**: `resources/AEs & SEs/<Full Name>.jpeg` — headshots for
  deck title slides (gitignored). Walid added his own; others missing.
- **Bug fix**: all example decks carry 2 headshot PICTURE shapes on slide
  1, invisible to a text-only scan. The first FR deck had shipped with the
  source deck's original photos. `style-guide.md` + `build-deck/SKILL.md`
  updated to call out the headshot swap.

## 2026-07-20 (improve: confirmation checkpoints added to demo-prep skills)
- **`build-deck`** confirms the demo-station shortlist before building.
- **New skills `demo-script`/`demo-setup`** formalized from the first real
  runs, each with a confirmation checkpoint before finalizing.

## 2026-07-20 (Gmail added to daily-briefing; deals updated from email)
- **New connector wired in: Gmail** — `daily-briefing/SKILL.md` scans the
  inbox as a fourth source, read-only (guardrail 1). New "📧 EMAIL" section.
- Several live deals updated from email evidence (deadlines confirmed, a
  stage moved to Contracting, a stub-row account fleshed out with a
  confirmed SE + demo date). Real detail stays in local-only account files.

## 2026-07-20 (process-customer: renamed Phase A-E to explicit step names)
- Phases renamed for clarity (Self-research / Ask-hard-stop / Analyze /
  Write / Update-and-announce). No behavior change.

## 2026-07-20 (an RFP deal claimed — Notion row from a parallel session)
- A new RFP deal was claimed live off a briefing flag; its Notion row was
  created in a different session (repurposing a blank stub). Real evidence
  moved to the account's local-only notes; that session's memory entry had
  inline customer specifics, trimmed to meta-level.

## 2026-07-20 (process-customer: added a "Demo — what to show" section)
- Template gains a "Demo — what to show" section tying concrete use cases
  to real pain points/MEDDPICC gaps (not a generic feature tour). Applied
  to the first FR brief (local-only) and its Notion page.

## 2026-07-20 (process-customer: first real run + quality upgrade)
- **First real `process-customer` run** — an account brief written from
  real Salesforce + Gong extracts, Notion row rewritten. Customer
  specifics live only in the local-only file.
- **Policy fix (Walid's correction): customer data must be git-ignored.**
  `accounts/*/*.md` added to `.gitignore`; guardrail 6 codified.
- **`process-customer/SKILL.md` upgraded**: Step 2 asks for Salesforce +
  Gong + a deal-specific Slack channel ID; Step 4's brief template
  restructured (prospect intro, attendees, bigger MEDDPICC, concrete
  next-steps checklist).
- **Notion — "Pipeline board" view added** (Kanban by Stage); the FR
  account's Notion page got the brief + checklist pasted in.

## 2026-07-20 (account-switch recovery: git, scheduler, artifact, GitHub connector)
- **Git repo reconciled with the real GitHub history** (`ewalid/claude-os`,
  branch `main`) after the local repo was orphaned from a different Claude
  account on the same Mac. Restored `README.md`, gitignored the example
  decks (~120MB, local-only).
- **Scheduled task and live artifact recreated** under this account.
- **GitHub MCP connector connected** (OAuth); tools load on a fresh session.

## 2026-07-15 (process-customer: full-row rewrite)
- **`process-customer` now rewrites the ENTIRE Notion Accounts DB row**
  (Stage, Priority, Notes, AE, Next call, Deadline, MEDDPICC) from real
  Gong/Salesforce/Slack evidence — not just MEDDPICC. Removed the
  propose-diff-wait-for-OK gate per guardrail 4; writes directly and
  announces after. Never writes an unevidenced field, never touches
  human-only columns.

## 2026-07-15 (process-customer upgrade)
- **`process-customer` now does a real MEDDPICC analysis** against pasted
  extracts, tagging each of the 8 elements Confirmed/Partial/Gap; only
  Confirmed elements write to Notion (a downgrade still needs Walid's OK).

## 2026-07-15
- **Notion — Accounts DB restructured.** Split the ambiguous "Due date"
  (kept as legacy) into **Next call** and **Deadline**; added **AE** select
  and **MEDDPICC** multi-select properties. Populated the live rows'
  dates; AEs left unassigned where no source confirmed them.
- **Notion — new views**: "This week", "Demos upcoming", "Overdue", and a
  "Dashboard" tab (charts couldn't be scripted via the API — added by hand).
- **Notion — new "📞 Debriefs" database** created, related back to Accounts.
- **daily-briefing** updated to read the new fields; became a live Cowork
  artifact and was scheduled (weekdays 9am Paris).
- **Repo created**: private GitHub repo, scaffolded from HANDOFF.md.

## 2026-07-15 (later same day)
- **People model corrected**: `resources/people.md` distinguishes the AE
  pod from SE colleagues (Chakit Arora, Roberto Butti, Ines Akrap) and
  Walid's manager (Matthew Alberts). Wired into the artifact's Slack triage.
- **Slack scan window widened** to "start of the last working day through
  now".
- **Deck format decided**: .pptx exports, not .odp.

## 2026-07-15 (Phase 2)
- **`darwin-improve`, `post-call-update`, `call-coach` skills drafted.**
  Phase 2 complete. Phase 3 next, blocked on Storyblok MCP + example decks.

## 2026-07-15 (Phase 3)
- **`resources/deck-examples/` populated** — 5 real demo decks (gitignored).
  Reverse-engineered the shared template into `style-guide.md`.
- **`build-deck`, `demo-script`, `demo-setup` skills drafted** — `demo-setup`
  blocked on the Storyblok connector.

## Known gaps / carried forward
- Notion "Analytics" view has no charts yet (API limitation).
- MEDDPICC not yet populated for most accounts — needs Walid's input or
  real extracts.
- `rfp-answer` library is empty — blocked on Walid seeding real content.
- `storyblok-content` can't run in-sandbox — network allowlist / run locally.
- `resources/battle-cards/` is empty — `demo-script`'s objection-sourcing
  has nothing to draw on yet.
- Colleague (AE/SE pod) names are still in tracked files by design — they
  aren't clients. `darwin-setup` regenerates them per person.
- Git *history* still contains pre-scrub client names — rewriting history
  (e.g. `git filter-repo`) is a separate manual step if Walid wants it.
