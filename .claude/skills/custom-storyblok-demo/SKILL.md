---
name: custom-storyblok-demo
description: >
  Trigger: "let's create a custom [demo] for [customer]" — the word "custom"
  matters, it's what distinguishes this from `demo-setup` (writes the planning
  script) and `storyblok-content` (ongoing content writes into an existing
  space). End-to-end pipeline for standing up a brand-new customer's
  Storyblok demo: Path A (Storyblok-internal operators) duplicates the
  private REDACTED-TEMPLATE ecommerce template; Path B (non-Storyblok
  operators, who can't access that private repo) scaffolds their chosen
  framework via the Storyblok CLI. Then, both paths: clone+own a private
  repo, fix known bugs (Path A), deploy to Vercel via CLI, set env vars via
  CLI, wire both preview URLs into the CMS. Runs mostly hands-off.
---

# custom-storyblok-demo

## What it does

Turns "let's create a custom for [customer]" into a deployed, CMS-wired demo,
end to end and mostly hands-off. Distinct from `demo-setup` (planning/script)
and `storyblok-content` (ongoing content writes into a working space). First
real run 2026-07-24 (Winfarm, by hand); reworked leaner 2026-07-24 after that
run — see "Efficiency notes" for what changed and why.

The shape: **one human-gated step up front** (the Storyblok space — Darwin
can't create it on Path A), and everything after runs without hand-holding —
clone+own the repo, fix the known bugs, deploy to Vercel via CLI, set env
vars via CLI, wire both preview URLs into the CMS.

## Two template paths — decide this FIRST

`storyblok/REDACTED-TEMPLATE` is a **PRIVATE, Storyblok-internal
repo.** A plain `git clone` of it 403s for anyone outside Storyblok. So which
path applies depends on who the operator is:

- **Path A — operator has access to the private template** (Storyblok
  employees, e.g. Walid). Use `REDACTED-TEMPLATE` as written below.
  It's the purpose-built ecommerce demo, and the known-bugs list in step 4
  and the Shopify product-picker specifics are all properties of *this
  template specifically*. This is the default, fully-proven path.
- **Path B — operator is NOT from Storyblok / has no access to that repo.**
  Do NOT try to clone the private template (it will fail). Instead, ask which
  **framework/language** they want (Next.js, Nuxt, Astro, SvelteKit, etc.),
  then scaffold with the **Storyblok CLI** — see step 3, Path B. This
  produces a *different* boilerplate, so the step-4 bug list does NOT apply
  verbatim — verify what actually reproduces in that boilerplate rather than
  blindly editing files that may not exist.

Confirm the operator's situation before step 3 if it isn't already obvious
(Darwin's own operator identity in `memory.md` usually settles it — Walid is
Storyblok-internal → Path A). When unsure, ask.

## Prerequisites

- GitHub CLI (`gh`) authenticated as the operator's own account.
- Vercel CLI authenticated (`npx vercel whoami`) — the deploy is done via CLI,
  not the dashboard.
- **Path A only:** access to the private `storyblok/REDACTED-TEMPLATE`
  repo (Storyblok-internal). **Path B only:** the Storyblok CLI — npm package
  `storyblok` (`npm i -g storyblok`, or `npx storyblok`); `storyblok create`
  scaffolds a framework project and can seed a matching space.
- Storyblok Management API token in `storyblok.env` (`STORYBLOK_MANAGEMENT_TOKEN`),
  and/or the official Storyblok MCP connector (now permanently configured —
  HTTP server at `https://mcp.labs.storyblok.com/mcp`, registered via
  `claude mcp add --transport http --scope user`; note it needs a **Personal
  Access Token**, not the space Management token, in the Bearer header, and
  its tools only load after a session restart following the add). The MCP has
  `getSpace` (read) but **no space-settings write op** — environments/preview
  URLs must be set via Management API curl regardless (see step 7).

## Steps

The one human-gated step (1) goes first, so the space is being created while
Darwin does the rest — by the time the token is needed (step 6), it's ready.

1. **FIRST, get the human-gated answers up front** — in one batched turn,
   before touching code, so they resolve while Darwin does steps 2-5.
   - **Path A: ask the operator to create the Storyblok space themselves.**
     Darwin must **never** create a space on this path (see guardrails): a
     blank API-created space has none of `default-se`'s component schemas and
     renders nothing, silently. Point them at Storyblok's "Solutions Demo
     Environment" flow (`app.storyblok.com/#/me/spaces/new?tab=experience-demo`,
     select "Solutions Demo Environment"), named after the customer, and have
     them disable **Settings → Maintenance Mode → Example Mode** while there
     (needed later for the Visual Editor to accept a custom preview URL).
     Don't block on their confirmation until step 6.
   - **Path B: ask which framework/language** instead — the Storyblok CLI's
     `storyblok create` scaffolds the space itself as part of step 3, so
     there's no separate manual space-creation ask here.
   - **Both paths: ask which Shopify store** — reuse the shared SE-demo
     sandbox used elsewhere, or a customer-specific one? (2026-07-24: the
     shared sandbox is a generic multi-tenant store —
     watches/F1-merch/smart-home/homeware — nothing agri-relevant; fine as a
     placeholder, but confirm it suits the demo.) Batch every applicable ask
     into one turn, don't drip them out.

2. **Resolve the customer's local working directory.** Check `~/dev/accounts/`
   for a folder that's already a variant of the customer's name — different
   casing, spacing, or an abbreviation (that directory already has
   inconsistent naming: `joué club`, `Group`, `implid-demo`/`implid-careers`
   as separate folders for one account). If something matches, use it. Only
   create `~/dev/accounts/<Customer>/` if genuinely nothing matches — **ask
   the operator to confirm** on any ambiguity. Never silently create a second
   folder for the same customer under a different spelling.

3. **Get the code into an owned private repo** — path-dependent:

   **Path A (private `default-se` template).** Clone straight into the
   working dir, drop the template remote, create+push the private repo in one
   `gh` call — one clone, no scratchpad, no re-clone:
   ```
   git clone https://github.com/storyblok/REDACTED-TEMPLATE.git \
     ~/dev/accounts/<Customer>/<customer>-storyblok-demo
   cd <that dir>
   git remote remove origin
   gh repo create <customer>-storyblok-demo --private --source=. \
     --remote=origin --push
   ```
   `--push` creates AND pushes in one step, and the working dir is already
   the clone — the first run's scratchpad + re-clone was pure waste.

   **Path B (Storyblok CLI scaffold).** Don't clone the private repo — it
   403s. Scaffold with the CLI in the framework the operator chose (step 1),
   then own it the same way:
   ```
   npx storyblok create ~/dev/accounts/<Customer>/<customer>-storyblok-demo \
     --template <framework>          # verify current flags: storyblok create --help
   cd <that dir>
   git init && git add -A && git commit -m "Initial scaffold"   # if create didn't
   gh repo create <customer>-storyblok-demo --private --source=. \
     --remote=origin --push
   ```
   `storyblok create` also scaffolds/links a Storyblok space for that
   boilerplate — so on Path B the space is handled here, not via the manual
   Solutions Demo Environment flow. CLI flags change across versions (the
   `--token`/`--skip-space` flags are recent) — check `storyblok create
   --help` rather than trusting the syntax above verbatim.

   **Both paths:** never use GitHub's native Fork — it defaults to public and
   carries visible "forked from …" lineage, wrong for a client demo repo
   (operator decision, 2026-07-24: "a private repo, so I can work on it
   without touching the team's template"). Repo name: lowercase-kebab,
   `<customer>-storyblok-demo`.

4. **Apply the known template bugs — Path A / `default-se` only, fresh on
   every new customer repo.** These five are all specific to
   `REDACTED-TEMPLATE`; a Path B CLI-scaffold (Next/Nuxt/Astro/etc.)
   won't have these files, so **skip this step on Path B** and instead verify
   that boilerplate boots cleanly and fix whatever actually reproduces there.
   Operator decision (2026-07-24): don't maintain a pre-fixed base fork,
   since it would silently drift from the upstream template. Read the target
   regions in ONE batched command (a single `cat`/`grep` across all five
   files, not five separate reads) to confirm each still reproduces before
   editing:
   - `plugins/storyblok.js` unconditionally calls the VWO SDK's `init()`
     using `VITE_VWO_ACCOUNT_ID`/`VITE_VWO_SDK_KEY`. Unset (the normal case
     until a personalization station actually wires up VWO), `init()`
     returns `null`. Guard: only call `init()` when both env vars are
     present, default `vwoClient` to `null` otherwise.
   - `storyblok/custom/ExperimentationVwo.vue` then calls `.getFlag()` on
     that client unconditionally — an unconditional "Cannot read properties
     of null" 500 on *every* route, not just pages using that block. Guard
     the `.getFlag()` call behind `if (vwoClient)`, falling back to the
     default/forced variation when it's absent.
   - `nuxt.config.js` has no `compatibilityDate` set (`NUXT_B5001` warning)
     — add one (today's date at setup time).
   - Any stray `import { defineProps } from 'vue'` — it's a compiler macro,
     doesn't need importing (found at least once, in `VideoPlayer.vue`).
   - `composables/getVersion.js` defaults to `'draft'` for every visit, with
     no working mechanism to switch to `'published'` — meaning Save and
     Publish look identical on the live deployed site, which undercuts any
     account whose pitch includes a draft/publish governance story. Flip it:
     real visits get `'published'`; only the Storyblok Visual Editor
     (detected via `route.query._storyblok`, the same convention already used
     in ~10 other components in this template — `Header.vue`, `Button.vue`,
     `ProductCard.vue`, etc. — reuse that, don't invent a new marker) gets
     `'draft'`. **Tell the operator afterward that every story they want
     visible on the live URL now needs an explicit Publish, not just Save.**
   Commit and push the fixes.

5. **`npm install`, then check `git diff` before committing.** On the first
   run this only produced benign lockfile metadata churn (stale
   `peer`/`extraneous` flags, the package-name field) — but earlier in the
   session an install had silently bumped `swiper` to a new major version.
   Revert `package.json`/`package-lock.json` if the diff includes dependency
   *version* changes that weren't the intent; commit if it's only metadata.

6. **Once the operator confirms the space exists, retrieve its Preview token
   yourself** — don't make them hunt for it. Via Management API:
   `GET /v1/spaces` to find the space by name → `GET /v1/spaces/<id>/api_keys/`
   → the token whose `access` is `"private"` is the draft-capable Preview
   token (verify if unsure: it can fetch `?version=draft`; the `public` one
   returns Unauthorized on draft). Then write `.env` in the project root:
   - `STORYBLOK_TOKEN` = that private/Preview token.
   - `SHOPIFY_DOMAIN` / `SHOPIFY_TOKEN` = per the step-1 answer.
   - Leave `VITE_VWO_*` / `AKENEO_*` commented out (step 4's guards cover it).
   Confirm `.env` is gitignored (`git check-ignore -v .env`).

7. **Deploy to Vercel via CLI — not the dashboard.** The CLI path is fully
   automatable and is the default now (operator, 2026-07-24: "could have
   deployed yourself and add the variable yourself"). Netlify remains an
   untried fallback — only offer it if the operator asks. Sequence:
   ```
   npx vercel link --yes --project <customer>-storyblok-demo --scope <team>
   # add all three vars to all three environments, non-interactively:
   for ENV in production preview development; do
     npx vercel env add STORYBLOK_TOKEN  "$ENV" --value "<token>"  --yes
     npx vercel env add SHOPIFY_DOMAIN   "$ENV" --value "<domain>" --yes
     npx vercel env add SHOPIFY_TOKEN    "$ENV" --value "<token>"  --yes
   done
   npx vercel --prod --yes    # builds WITH the env vars already set
   ```
   Doing env-vars-before-first-`--prod` avoids the dashboard flow's
   "redeploy needed after adding vars" gotcha entirely. If a project already
   exists, `vercel link` finds it; if `link` prints a `next[]` hint asking
   for `--scope`, re-run with the scope it names. The final `--prod` output
   gives both the deployment URL and the aliased
   `https://<customer>-storyblok-demo.vercel.app`.

8. **Verify the DEPLOYED URL renders real content — cheaply. Skip localhost
   entirely** (operator, 2026-07-24: "don't run it... you don't have to
   check that it works"). The deployed check is the real gate and it also
   confirms the space isn't empty. Do it with ONE lightweight check:
   `get_page_text`, or a single `innerText.includes('...')` via the JS tool.
   **Never use `read_network_requests` on this template** — it inlines fonts
   as base64 `data:` URIs and the dump is enormous (the big token sink on
   the first run). Avoid screenshots unless a text check is ambiguous; if the
   first check looks blank, it's usually a paint-timing race — re-check text,
   don't reach for a screenshot.

9. **Wire both preview URLs into the CMS** via Management API (the MCP has no
   space-settings write op). Re-fetch environments first so you don't clobber
   anything, then PUT the full list:
   `PUT /v1/spaces/<id>` with
   `{"space":{"environments":[
     {"name":"dev","location":"https://localhost:3000/"},
     {"name":"Vercel","location":"https://<customer>-storyblok-demo.vercel.app/"}]}}`.
   Plain URLs, nothing appended — the `?token=...&path=...` scheme is only
   for Storyblok's multi-tenant demo hosting, and a query-param token
   *overrides* the env-var fallback (pasting the wrong one there — e.g. a
   Shopify token — silently breaks auth, which happened 2026-07-24). Set the
   Vercel entry as the one to use as default. localhost is wired as "dev" for
   convenience but is NOT launched or verified by this skill.

## Efficiency notes (what changed after the first run, and why)

- **Space-ask goes first** so the human-gated bottleneck overlaps the code
  work instead of blocking at the end.
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

- Tokens (Storyblok Preview, Shopify Storefront) live only in that
  project's own local `.env`, gitignored by the template itself — never in
  the darwin repo, never in any tracked file. A Storefront token is
  public/read-only by design and safe to receive in chat; nothing else is.
- **Path A: never create the Storyblok space via a raw API call, not even
  "just in case."** A blank space created that way has none of `default-se`'s
  component schemas and fails silently (renders nothing, no error). Ask the
  operator to create it via the Solutions Demo Environment flow and wait for
  confirmation. (Path B is different: the Storyblok CLI's `storyblok create`
  legitimately scaffolds/seeds a matching space for its own boilerplate —
  that's fine, because it's seeded, not blank.) On either path, once the
  space exists, retrieving its Preview token IS Darwin's job (step 6) — don't
  make the operator fetch and paste it.
- **`storyblok/REDACTED-TEMPLATE` is a private, Storyblok-internal
  repo.** Don't attempt to clone it for an operator without access — it 403s.
  For non-Storyblok operators, use Path B (Storyblok CLI scaffold in their
  chosen framework) instead. See "Two template paths."
- The render-verification gate is the **deployed URL**, not localhost — and
  it must be a real content check (text actually present), not just a 200 or
  "no crash," because an empty space renders cleanly with zero content. Skip
  launching/verifying localhost entirely (operator preference, 2026-07-24).
  Keep the verification cheap: `get_page_text`/`innerText`, never
  `read_network_requests` on this template (inlined base64 fonts).
- **Read each message for which mode is active before assuming a default.**
  The operator has, in the same session, asked to type every command
  themselves *and* asked for direct edits/commits ("do it yourself") at
  different points — don't assume one mode holds for an entire session
  (2026-07-24, Winfarm).
- Treat "abort" / "NO NO" as an immediate hard stop — revert whatever's
  in-progress without a clarifying question first, then ask what to do
  next (2026-07-24, Winfarm).
- A Shopify Storefront token can read products/collections and drive a mock
  cart, but cannot create/edit them — that needs a separate Admin API token
  (or just creating products by hand in the Shopify admin UI, which needs no
  token and is fast enough for a dozen demo products). Don't conflate the two
  credentials or assume one covers the other.
- If the demo uses `FeaturedProducts`/`ImageTextSectionProduct` blocks, don't
  mock product data in code — those two block types use a separate custom
  Storyblok field plugin (`shopify-demo-product-picker`) that searches the
  *live* Shopify catalog for editors to pick from, and has no awareness of
  anything mocked in the frontend's own composables. Create real
  products/a real collection in Shopify itself instead.
