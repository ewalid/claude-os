---
name: custom-storyblok-demo
description: >
  Trigger: "let's create a custom [demo] for [customer]" — the word "custom"
  matters, it's what distinguishes this from `demo-setup` (writes the planning
  script) and `storyblok-content` (ongoing content writes into an existing
  space). End-to-end pipeline for standing up a brand-new customer's
  Storyblok ecommerce demo from the REDACTED-TEMPLATE template: own
  repo, local working directory, Storyblok space, env vars, localhost check,
  then a guided deploy.
---

# custom-storyblok-demo

## What it does

Turns "let's create a custom for [customer]" into a working local dev
environment plus a guided deploy, end to end. This is the concrete,
repeatable pipeline — distinct from `demo-setup` (the planning/script-writing
skill) and `storyblok-content` (ongoing content writes into a space that
already exists and works). First real run: 2026-07-24, Winfarm Group,
retroactively — this skill formalizes what actually happened by hand,
message by message, that session.

## Prerequisites

- GitHub CLI (`gh`) authenticated as the operator's own account.
- A Storyblok MCP connector, or a Management API token in `storyblok.env`
  (`STORYBLOK_MANAGEMENT_TOKEN`) — same dual-path rule as `storyblok-content`,
  check both every time, never assume which is live.

## Steps

1. **Resolve the customer's local working directory.** Check `~/dev/accounts/`
   for a folder that's already a variant of the customer's name — different
   casing, spacing, or an abbreviation (that directory already has
   inconsistent naming: `joué club`, `Group`, `implid-demo`/`implid-careers`
   as separate folders for one account). If something matches, use it. Only
   create `~/dev/accounts/<Customer>/` if genuinely nothing matches — **ask
   the operator to confirm** on any ambiguity. Never silently create a second
   folder for the same customer under a different spelling.

2. **Create the repo — a private duplicate, not a GitHub Fork.** From a
   fresh local clone of `storyblok/REDACTED-TEMPLATE`:
   `gh repo create <customer>-storyblok-demo --private --source=. --remote=<tmp>`,
   then swap the new repo in as `origin` and push. Never use GitHub's native
   Fork feature for this — it defaults to public and carries visible
   "forked from storyblok/..." lineage, wrong for a client demo repo
   (explicit operator decision, 2026-07-24: "duplicating the template repo
   in my personal repo as a private repo, so I can work on it without
   touching the team's template"). Repo name convention: lowercase-kebab,
   `<customer>-storyblok-demo` (matches `winfarm-storyblok-demo`).

3. **Clone it into the working directory resolved in step 1.**

4. **Apply the known template bugs — fresh, on every new customer repo.**
   Explicit operator decision (2026-07-24): don't maintain a pre-fixed base
   fork to duplicate from instead, since that would silently drift from
   Storyblok's actual current template over time. Before applying each fix,
   verify it still reproduces — the upstream template may have been patched
   since this was last written:
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

5. **Check whether a Storyblok space for this customer already exists**
   (MCP connector, or Management API space list). If yes, skip straight to
   step 6.

   **If no space exists yet, never attempt to create it — not even as an
   API call to try.** A blank space created via a plain "create space" API
   call has none of this template's component schemas (Header,
   ProductPageOverview, etc.) — nothing will render even with a valid token,
   and it fails silently, not with an error. Confirmed 2026-07-24: this
   specific template (`REDACTED-TEMPLATE`) ships no
   `components.*.json` export and no Storyblok CLI dependency to push
   schemas programmatically — unlike the older
   `REDACTED-TEMPLATE-2` template, which did ship one.
   Instead: **ask the operator to create it themselves**, via Storyblok's
   own "Solutions Demo Environment" flow
   (`app.storyblok.com/#/me/spaces/new?tab=experience-demo`, select
   "Solutions Demo Environment"), named after the customer. Wait for their
   explicit confirmation it's done (operator decision, 2026-07-24: "don't
   create the space yourself ask me to do it") — don't proceed to step 6 on
   an assumption.

6. **Once the operator confirms the space is ready, retrieve its Preview
   token yourself** (MCP connector, or Management API — Settings → Access
   Tokens → **Preview**, not Public) rather than asking them to copy/paste
   it. This is the one part of space setup that IS on Darwin: fetch it,
   don't make the operator hunt for it.

7. **Create `.env`** in the project root:
   - `STORYBLOK_TOKEN` = the Preview token from step 6.
   - `SHOPIFY_DOMAIN` / `SHOPIFY_TOKEN` — **ask the operator** whether this
     customer reuses the same shared SE-demo Shopify sandbox used elsewhere,
     or needs its own; never assume silently. (2026-07-24: the shared
     sandbox turned out to be a generic multi-tenant store — watches,
     F1-team merch, smart-home, homeware — with nothing relevant to an
     agri-wholesale account. Confirm the shared store is actually usable for
     *this* customer's demo before reusing it as-is.)
   - Leave `VITE_VWO_*`/`AKENEO_*` commented out — safe now that step 4's
     guards are in place; only fill them in if that station is actually
     part of this customer's demo.

8. **`npm install`, then check `git diff` before ever committing.**
   `npm install` on this template has silently bumped `package.json`'s
   `swiper` range to a new major version purely from lockfile regeneration,
   with no explicit ask for that upgrade — revert
   `package.json`/`package-lock.json` if the diff includes dependency
   changes that weren't the actual intent of the install.

9. **`npm run dev`, then confirm localhost actually renders real content —
   not just "no crash."** A guarded VWO client and a valid token still
   render nothing useful if step 5's space doesn't have matching
   components/stories yet. This is the real verification gate.

10. **Ask the operator: Netlify or Vercel?** before deploying — this
    template's own README defaults to Netlify examples; only the Vercel path
    is proven end-to-end so far (2026-07-24, Winfarm). Guide whichever is
    chosen:
    - **Vercel**: Git-import via vercel.com/new (not the CLI), so pushes
      auto-deploy. Double-check the **Team dropdown on the New Project
      screen itself** — it can default to a personal Hobby team even when
      the account-level dashboard was showing a different team. Skip env
      vars on the first pass, deploy, then add
      `STORYBLOK_TOKEN`/`SHOPIFY_DOMAIN`/`SHOPIFY_TOKEN` in Settings →
      Environment Variables and manually trigger a **Redeploy** — Vercel
      does not redeploy automatically just because env vars changed.
    - **Netlify**: not yet run end-to-end by this skill. Treat the first
      real run as a chance to capture the equivalent gotchas afterward —
      don't assume the Vercel steps translate directly.

11. **Set the Storyblok space's Visual Editor Location** to the plain
    deployed URL, nothing appended (`https://<deployment-url>`) — the
    `?token=...&path=...` scheme in this template's README is only for
    Storyblok's own multi-tenant demo-hosting setup, not a dedicated
    single-customer deploy where `STORYBLOK_TOKEN` is already an env var.
    A query-param token also *overrides* the env var fallback, so pasting
    the wrong value there (e.g. a Shopify token by mistake, which happened
    2026-07-24) breaks auth in a confusing way — leave it out entirely.
    Also disable **Settings → Maintenance Mode → Example Mode**, required
    for any custom preview URL to work in the Visual Editor at all.

## Guardrails

- Tokens (Storyblok Preview, Shopify Storefront) live only in that
  project's own local `.env`, gitignored by the template itself — never in
  the darwin repo, never in any tracked file. A Storefront token is
  public/read-only by design and safe to receive in chat; nothing else is.
- **Never attempt to create a Storyblok space, not even as an API call to
  try "just in case."** Always ask the operator to create it themselves via
  the Solutions Demo Environment flow, and wait for their explicit
  confirmation before proceeding — a space Darwin created itself (or
  assumed existed) is exactly the silent-failure case step 5 warns about.
  Once confirmed, retrieving that space's Preview token IS Darwin's job
  (step 6) — don't ask the operator to fetch and paste it.
- Never treat "localhost didn't crash" as "localhost works" — confirm
  actual content renders (step 9), not just an absence of errors.
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
