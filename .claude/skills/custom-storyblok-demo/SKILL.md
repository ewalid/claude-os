---
name: custom-storyblok-demo
description: >
  Trigger: "let's create a custom [demo] for [customer]" — the word "custom"
  matters, it's what distinguishes this from `demo-setup` (writes the planning
  script) and `storyblok-content` (ongoing content writes into an existing
  space). End-to-end pipeline for standing up a brand-new customer's Storyblok
  demo: Path A duplicates a starter template repo the operator already uses;
  Path B (no such template / non-Storyblok operators) scaffolds their chosen
  framework via the Storyblok CLI. Then, both paths: clone+own a private repo,
  apply any template-specific fixes the operator tracks locally, deploy to
  Vercel via CLI, set env vars via CLI, wire both preview URLs into the CMS.
  Runs mostly hands-off. Template-specific specifics live in local memory,
  never in this file.
---

# custom-storyblok-demo

## What it does

Turns "let's create a custom for [customer]" into a deployed, CMS-wired demo,
end to end and mostly hands-off. Distinct from `demo-setup` (planning/script)
and `storyblok-content` (ongoing content writes into a working space). First
real run 2026-07-24; reworked leaner the same day — see "Efficiency notes".

The shape: **the human-gated bits go up front** (a starter template choice
and, if that template needs a pre-seeded CMS space, its creation — Darwin
can't seed one that renders), then everything after runs without hand-holding:
clone+own the repo, apply any known fixes, deploy to Vercel via CLI, set env
vars via CLI, wire both preview URLs into the CMS.

> **Template-specific details are local-only.** Which starter repo the
> operator uses, its known bugs/fixes, and any CMS-seeding quirks are
> confidential to the operator's setup and live in `memory.md` (gitignored) —
> **never** name a specific private repo, its files, or its fix list in this
> tracked skill. This file stays a generic process; memory supplies the
> specifics at run time.

## Two template paths — decide this FIRST

- **Path A — the operator has a starter/template repo they duplicate for
  demos.** Ask which one (the operator's usual default may be recorded in
  local `memory.md` — check there first; if found, confirm rather than
  re-ask). Use it as the base in step 3. Any template-specific fixes or a
  required pre-seeded CMS space are properties of *that* template — pull those
  from local memory, don't hardcode them here.
- **Path B — no such template, or the operator can't access one.** Ask which
  **framework/language** they want (Next.js, Nuxt, Astro, SvelteKit, etc.),
  then scaffold with the **Storyblok CLI** (see step 3, Path B). This produces
  a different boilerplate each time, so any Path-A fix list does NOT apply —
  verify what actually reproduces rather than editing files that may not exist.

Confirm which path applies before step 3. If the operator's memory records a
default template, that settles it (Path A); otherwise ask.

## Prerequisites

- GitHub CLI (`gh`) authenticated as the operator's own account.
- Vercel CLI authenticated (`npx vercel whoami`) — the deploy is done via CLI,
  not the dashboard.
- **Path A:** access to whatever starter template repo the operator names.
  **Path B:** the Storyblok CLI — npm package `storyblok` (`npm i -g storyblok`,
  or `npx storyblok`); `storyblok create` scaffolds a framework project and can
  seed a matching space.
- Storyblok Management API token in `storyblok.env` (`STORYBLOK_MANAGEMENT_TOKEN`),
  and/or the official Storyblok MCP connector (now permanently configured —
  HTTP server at `https://mcp.labs.storyblok.com/mcp`, registered via
  `claude mcp add --transport http --scope user`; it needs a **Personal Access
  Token**, not the space Management token, in the Bearer header, and its tools
  only load after a session restart following the add). The MCP has `getSpace`
  (read) but **no space-settings write op** — environments/preview URLs must be
  set via Management API curl regardless (see step 9).

## Steps

The human-gated step (1) goes first, so anything the operator must do resolves
while Darwin works — by the time the token is needed (step 6), it's ready.

1. **FIRST, get the human-gated answers up front** — in one batched turn,
   before touching code.
   - **Path A: ask which starter template repo** to duplicate (check
     `memory.md` for a recorded default first and just confirm it). If that
     template needs a pre-seeded CMS space that Darwin can't create itself
     (see the space guardrail), **ask the operator to create it** and to do
     any one-time space setup their process requires; don't block on their
     confirmation until step 6.
   - **Path B: ask which framework/language** instead — the Storyblok CLI's
     `storyblok create` scaffolds and seeds the space itself in step 3, so no
     separate manual space-creation ask is needed.
   - **Both paths: ask which Shopify store** — reuse the shared SE-demo
     sandbox used elsewhere, or a customer-specific one? (The shared sandbox
     is a generic multi-tenant store with no vertical-specific catalog — fine
     as a placeholder, but confirm it suits the demo.) Batch every applicable
     ask into one turn, don't drip them out.

2. **Resolve the customer's local working directory.** Check `~/dev/accounts/`
   for a folder that's already a variant of the customer's name — different
   casing, spacing, or an abbreviation (that directory already has
   inconsistent naming: `joué club`, `Group`, `implid-demo`/`implid-careers`
   as separate folders for one account). If something matches, use it. Only
   create `~/dev/accounts/<Customer>/` if genuinely nothing matches — **ask
   the operator to confirm** on any ambiguity. Never silently create a second
   folder for the same customer under a different spelling.

3. **Get the code into an owned private repo** — path-dependent:

   **Path A (duplicate the operator's starter template).** Clone it straight
   into the working dir, drop the template remote, create+push the private
   repo in one `gh` call — one clone, no scratchpad, no re-clone:
   ```
   git clone <template-repo-the-operator-named> \
     ~/dev/accounts/<Customer>/<customer>-storyblok-demo
   cd <that dir>
   git remote remove origin
   gh repo create <customer>-storyblok-demo --private --source=. \
     --remote=origin --push
   ```
   `--push` creates AND pushes in one step, and the working dir is already the
   clone — the first run's scratchpad + re-clone was pure waste.

   **Path B (Storyblok CLI scaffold).** Scaffold with the CLI in the framework
   the operator chose (step 1), then own it the same way:
   ```
   npx storyblok create ~/dev/accounts/<Customer>/<customer>-storyblok-demo \
     --template <framework>          # verify current flags: storyblok create --help
   cd <that dir>
   git init && git add -A && git commit -m "Initial scaffold"   # if create didn't
   gh repo create <customer>-storyblok-demo --private --source=. \
     --remote=origin --push
   ```
   `storyblok create` also scaffolds/links a Storyblok space for that
   boilerplate — so on Path B the space is handled here. CLI flags change
   across versions (the `--token`/`--skip-space` flags are recent) — check
   `storyblok create --help` rather than trusting the syntax above verbatim.

   **Both paths:** never use GitHub's native Fork — it defaults to public and
   carries visible "forked from …" lineage, wrong for a client demo repo
   (operator decision, 2026-07-24: "a private repo, so I can work on it
   without touching the team's template"). Repo name: lowercase-kebab,
   `<customer>-storyblok-demo`.

4. **Apply any known template fixes — Path A only.** If the operator's starter
   template has recurring boot-breaking bugs, the fix list lives in local
   `memory.md` (Darwin loads it each session) — apply those fixes fresh on
   every new customer repo, and **re-verify each still reproduces** before
   editing (the upstream template may have been patched). Skip this step
   entirely on Path B — a freshly-scaffolded boilerplate won't have the same
   files; instead just confirm it boots cleanly and fix whatever actually
   breaks there. Commit and push the fixes.

5. **`npm install`, then check `git diff` before committing.** Benign lockfile
   metadata churn (stale `peer`/`extraneous` flags, the package-name field) is
   fine to commit — but watch for silent dependency *version* bumps (an
   install once bumped `swiper` to a new major). Revert
   `package.json`/`package-lock.json` if the diff includes version changes that
   weren't the intent; commit if it's only metadata.

6. **Once the space exists, retrieve its Preview token yourself** — don't make
   the operator hunt for it. Via Management API:
   `GET /v1/spaces` to find the space by name → `GET /v1/spaces/<id>/api_keys/`
   → the token whose `access` is `"private"` is the draft-capable Preview
   token (verify if unsure: it can fetch `?version=draft`; the `public` one
   returns Unauthorized on draft). Then write `.env` in the project root:
   - `STORYBLOK_TOKEN` = that private/Preview token.
   - `SHOPIFY_DOMAIN` / `SHOPIFY_TOKEN` = per the step-1 answer.
   - Leave any optional integration vars (analytics/personalization SDKs, PIM
     connectors, etc.) commented out unless that station is part of the demo.
   Confirm `.env` is gitignored (`git check-ignore -v .env`).

7. **Deploy to Vercel via CLI — not the dashboard.** The CLI path is fully
   automatable and is the default now (operator, 2026-07-24: "could have
   deployed yourself and add the variable yourself"). Netlify remains an
   untried fallback — only offer it if the operator asks. Sequence:
   ```
   npx vercel link --yes --project <customer>-storyblok-demo --scope <team>
   # add all vars to all three environments, non-interactively:
   for ENV in production preview development; do
     npx vercel env add STORYBLOK_TOKEN  "$ENV" --value "<token>"  --yes
     npx vercel env add SHOPIFY_DOMAIN   "$ENV" --value "<domain>" --yes
     npx vercel env add SHOPIFY_TOKEN    "$ENV" --value "<token>"  --yes
   done
   npx vercel --prod --yes    # builds WITH the env vars already set
   ```
   Doing env-vars-before-first-`--prod` avoids the dashboard flow's
   "redeploy needed after adding vars" gotcha entirely. If a project already
   exists, `vercel link` finds it; if `link` prints a `next[]` hint asking for
   `--scope`, re-run with the scope it names. The final `--prod` output gives
   both the deployment URL and the aliased
   `https://<customer>-storyblok-demo.vercel.app`.

8. **Verify the DEPLOYED URL renders real content — cheaply. Skip localhost
   entirely** (operator, 2026-07-24: "don't run it... you don't have to check
   that it works"). The deployed check is the real gate and it also confirms
   the space isn't empty. Do it with ONE lightweight check: `get_page_text`,
   or a single `innerText.includes('...')` via the JS tool. **Never use
   `read_network_requests`** on a Nuxt/Vite frontend like this — it inlines
   fonts as base64 `data:` URIs and the dump is enormous (the big token sink
   on the first run). Avoid screenshots unless a text check is ambiguous; a
   first "blank" check is usually a paint-timing race — re-check text.

9. **Wire both preview URLs into the CMS** via Management API (the MCP has no
   space-settings write op). Re-fetch environments first so you don't clobber
   anything, then PUT the full list:
   `PUT /v1/spaces/<id>` with
   `{"space":{"environments":[
     {"name":"dev","location":"https://localhost:3000/"},
     {"name":"Vercel","location":"https://<customer>-storyblok-demo.vercel.app/"}]}}`.
   Plain URLs, nothing appended — a `?token=...&path=...` query-param token
   *overrides* the env-var fallback, so pasting the wrong one there (e.g. a
   Shopify token by mistake) silently breaks auth (happened 2026-07-24). Set
   the Vercel entry as the default. localhost is wired as "dev" for
   convenience but is NOT launched or verified by this skill.

## Efficiency notes (what changed after the first run, and why)

- **Human-gated asks go first** so they overlap the code work instead of
  blocking at the end.
- **One clone, no scratchpad** (`gh repo create --source=. --push`) — the
  first run cloned twice.
- **Vercel via CLI**, env-vars-before-first-deploy — no dashboard, no
  Netlify-vs-Vercel question, no post-hoc redeploy.
- **No localhost run/verify** — the deployed URL is the single render gate.
- **Cheap verification only**: `get_page_text`/`innerText`, never
  `read_network_requests` (base64 fonts) and rarely a screenshot.
- **Don't churn MCP `search`** for space-settings writes — they aren't in the
  connector; go straight to Management API curl for token + environments.

## Guardrails

- Tokens (Storyblok Preview, Shopify Storefront) live only in that project's
  own local `.env`, gitignored by the template itself — never in the darwin
  repo, never in any tracked file. A Storefront token is public/read-only by
  design and safe to receive in chat; nothing else is.
- **Template-specific specifics stay in local memory, never in this tracked
  file** — which starter repo the operator uses, its exact files/bugs/fixes,
  and any CMS-seeding quirks are confidential to the operator's setup. This
  skill names none of them; it reads them from `memory.md` at run time.
- **Never create a CMS space that would render blank.** If the operator's
  template needs a space pre-seeded with matching component schemas, Darwin
  cannot produce that via a raw "create space" API call — a blank space fails
  silently (renders nothing, no error). Ask the operator to create/seed it and
  wait for confirmation. (Path B is different: the Storyblok CLI legitimately
  scaffolds a seeded space for its own boilerplate — fine, it's not blank.)
  On either path, once the space exists, retrieving its Preview token IS
  Darwin's job (step 6) — don't make the operator fetch and paste it.
- The render-verification gate is the **deployed URL**, not localhost — and it
  must be a real content check (text actually present), not just a 200 or "no
  crash," because an empty space renders cleanly with zero content. Skip
  launching/verifying localhost entirely (operator preference, 2026-07-24).
  Keep verification cheap: `get_page_text`/`innerText`, never
  `read_network_requests` (inlined base64 fonts).
- **Read each message for which mode is active before assuming a default.**
  The operator has, in the same session, asked to type every command
  themselves *and* asked for direct edits/commits ("do it yourself") at
  different points — don't assume one mode holds for an entire session
  (2026-07-24).
- Treat "abort" / "NO NO" as an immediate hard stop — revert whatever's
  in-progress without a clarifying question first, then ask what to do next
  (2026-07-24).
- A Shopify Storefront token can read products/collections and drive a mock
  cart, but cannot create/edit them — that needs a separate Admin API token
  (or just creating products by hand in the Shopify admin UI, which needs no
  token and is fast enough for a dozen demo products). Don't conflate the two.
- Some templates drive certain content blocks from a **live** Shopify catalog
  via a custom CMS field plugin (an editor picks real products in the Visual
  Editor). Mocking product data in code silently breaks those blocks. If the
  operator's template has such blocks (tracked in local memory), use real
  Shopify products rather than mocks. Generic caveat here; specifics in memory.
