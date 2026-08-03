# Plans, licensing & feature gating

**Source of truth:** the public pricing-page comparison matrix
(storyblok.com/pricing) + the current fact sheet linked from it
(`july-2026-pricing-plan-factsheet.pdf`) + the internal "Enterprise:
Premium vs. Elite" solutions guide in company Notion. Transcribed
2026-07-30. Trust: `official-docs` for the matrix, `validated` for the
internal guide.

⚠️ **Re-check the live page before quoting numbers in a real submission.**
This matrix changes; the fact-sheet filename is date-stamped for a reason.

⚠️ **Never copy customer pricing calculators, quotes or discount structures
into this file.** Those are per-deal confidential and live in Drive. This
file is public list price + feature gating only, and is tracked in git.

## Current tier names (get these right)
**Starter** (free) → **Growth** ($99/mo, $90.75/mo billed annually) →
**Growth Plus** ($349/mo, $319.91/mo billed annually) → **Premium**
(custom) → **Elite** (custom). Annual billing = one month free.
Subscriptions are **per space**, not per account. The 45-day free trial
runs on Growth Plus.

There is no "Community", "Entry", "Teams" or "Business" tier — don't
invent tier names from memory.

## Core quotas
| | Starter | Growth | Growth Plus | Premium | Elite |
|---|---|---|---|---|---|
| Spaces (projects) | 1 | 1 | 1 | Custom | Custom |
| Seats incl. / max | 1 / 2 | 5 / 10 | 15 / 20 | Custom | Custom |
| Extra seat | $15/mo | $15/mo | $15/mo | Custom | Custom |
| **Uptime SLA** | none | 97% | 97% | **99.9%** | **99.99%** |
| API requests/mo | 100k | 1M (max 5M) | 4M (max 15M) | Custom | Custom |
| Traffic incl. / max | 100GB | 400GB / 1TB | 1TB / 2TB | Custom | Custom |
| Stories | 20k | 25k | 100k | Unlimited | Unlimited |
| Assets no. / max size | 2k / 500MB | 2.5k / 1GB | 10k / 1GB | Custom / 5GB | Custom / 5GB |
| Locales incl. | 2 | 2 (unlimited avail.) | 10 (unlimited avail.) | Custom | Custom |
| Components | 200 | 600 | 600 | Unlimited | Unlimited |
| Content folders | 100 | 1,000 | 1,000 | 2,000 | 2,000 |
| Datasources | 10 | 20 | 20 | Unlimited | Unlimited |
| Webhooks | 3 | 5 | 5 | 15 | 15 |
| Preview URLs | 2 | 6 | 6 | Unlimited | Unlimited |
| **Version / activity / webhook log retention** | 1 day | 30 days | 30 days | **180 days** | **Unlimited** |
| Scheduled single stories | — | 2 | 2 | 100 | 100 |
| AI credits/mo | 25k | 100k (max 500k) | 200k (max 1M) | Custom | Custom |

Self-serve overage: $10 per 1M API requests · $75 per 250GB traffic · $20
per locale · $20 per 200k AI credits · $5 per 10k assets · $5 per 1k
stories (Growth Plus).

## Feature gating — this is the part that gets answers wrong
**Premium + Elite only** (i.e. on *no* self-service plan):
A/B testing · GraphQL Content Delivery API · Pipelines / content staging ·
Releases · Release merging · Single Sign-On · SCIM provisioning ·
Forced 2FA · Custom roles · Custom workflows & workflow stages ·
Conditional fields · Advanced paths · Shared components · Custom metadata
fields · History comparison view · Historical visual preview · Language
export/import · Translatable asset metadata · Organization analytics ·
Dedicated CSM · Space Plugins & Tool Plugins · AI SEO · Basic AI branding ·
Visualize unpublished relations · Payment by wire transfer.

**Elite only:** Additional Access Controls (folder / block / datasource /
asset-folder scoping) · Access Token Scopes · Restricted IP address range ·
Security Audit · Bring-your-own AI model · Advanced AI branding ·
Enterprise Assets Library (a paid add-on on Premium).

**Both have it, but not equally:**
| | Premium | Elite |
|---|---|---|
| Releases | 20 | Unlimited |
| Custom workflows | 2 | Unlimited |
| Custom roles | 10 | Unlimited |
| Pipeline stages | 5 | Unlimited |
| S3 backup frequency | Weekly | Daily |
| Managed backup (paid add-on) | Weekly, 180 days | Daily, 7 years |
| Preferred data centre / region | **paid add-on** | **included** |
| Log & version retention | 180 days | Unlimited |
| Support | Advanced | Priority |

**Paid add-ons, not bundled** — marked `$`/`€` on the matrix: Managed
Backup Services (both tiers), Preferred Data Centers (Premium only),
Enterprise Assets Library (Premium only).

**From Growth up:** Dimensions (multi-tree localisation) · translatable
slugs · content locking · SEO meta tags · single-story scheduling · task
manager · duplicate spaces · webhook secrets · replace assets · AI alt text.

**All tiers incl. Starter:** Visual Editor · Content Delivery API ·
Management API · internationalisation · image optimisation service · asset
manager · activity log · default workflows · 2FA · custom field types ·
Field Plugins · responsive preview · blueprints · autosave · AI content
generation · AI translations · Storyblok Labs.

## Support tiers (consistent with the internal Premium-vs-Elite guide)
- **Core** — Starter / Growth / Growth Plus.
- **Advanced** — Premium: all severities next business day or later.
- **Priority** — Elite: Critical 2h · High 12h · Medium 24h · Low 1
  business day, plus emergency protocols, alerting and dedicated
  monitoring involving senior leadership.
- Dedicated CSM on Premium and Elite.
- **Extended Support Package** (add-on, from a validated grid): SLA-defined
  response times plus **24/7 critical incident management, 2h response for
  critical incidents**, and post-incident reporting. Support-hour carryover
  terms are **contract-negotiable**. When a prospect specifies P1/P2/P3
  bands "to market-conform specs", the honest answer is *partial* — the
  bands don't map one-to-one.

## Integration gating (matters on almost every RFP)
**All tiers:** Figma, Netlify, Vercel.
**Growth up:** Bynder, Cloudinary, **Optimizely**, Semrush, Shopify, Slack.
**Premium/Elite only:** Akeneo, Algolia, BigCommerce, Centra,
CommerceLayer, commercetools, inRiver, Nacelle, Saleor, Salesforce Commerce
Cloud, SAP, Shopware, Spryker, Sylius, Vendure, **VWO**.
(Note: the official fact sheet places **Smartling** from Growth up, while
the live pricing page shows it Premium/Elite — check the live page for a
real submission.)

Standard third-party disclaimer on the fact sheet worth mirroring in
responses: Storyblok isn't liable for third-party service performance,
access must be purchased from the provider directly, and the integration
list can change.

→ Nearly every PIM / commerce / TMS connector is Premium/Elite-gated. If an
integration is central to the deal it sets a **pricing floor** — say so
early rather than discovering it in negotiation.

## FlowMotion is a separately priced product line
Not merely a roadmap item. Three tiers, all "contact sales": **Core** (40k
workflow executions/mo, 10 concurrent runs) · **Advanced** (120k, 30) ·
**Pro** (180k+, 50+, customisable). 365-day workflow history on all three.
Add-ons on each: source control & environments, AI Assistant, custom
execution data, external secrets, audit & log streaming, insights.

## ✅ SSO conflict RESOLVED (2026-08-03, official public fact sheet)
The official fact sheet uses an explicit legend — **✔ = included in user
licence, $ = additional fee or terms apply** — and shows:
- **Single Sign-On: ✔ on Premium and Elite** (included, not an add-on).
- Its "Security Access" summary row states Premium = "Standard roles +SSO";
  Elite = "Standard roles, +SSO, access token scopes, IP restrictions,
  additional access control".

So the validated submission's "SSO is an add-on on Enterprise plans" was
**imprecise** — SSO is bundled on both enterprise tiers. Genuine paid
add-ons are the `$` rows: Preferred Data Center (Premium), Additional Data
Center China (both), Managed Backup Service (both).

## Commercial terms found on the official fact sheet
- **Maximum Usage Breach Fee: $/€1,000** — an overage surcharge that applies
  when a customer exceeds the API & traffic limit in **two consecutive
  months**. Worth surfacing in any TCO/pricing-predictability answer; a
  prospect asking about cost overrun protection will care.
- Self-managed **S3 backups are Premium/Elite** (weekly on Premium, daily on
  Elite) to the customer's own bucket; **Managed Backup Service** is a paid
  add-on on both (Premium weekly/180 days, Elite daily/up to 7 years).
- **Custom AI Tokens ✔ on both Premium and Elite** — note this sits in mild
  tension with the pricing page's "Bring your own AI = Elite only" row; two
  differently-named rows for adjacent things. Confirm which the prospect
  actually needs before answering.
- Legal entities confirmed: Storyblok GmbH (Linz), Storyblok Solutions GmbH
  (Vienna), Storyblok Inc. (Wilmington, Delaware), Storyblok LTD (London).

## Licensing breakdown structure (what to itemise in a response)
Platform: annual licensing (tier, seats, locales, spaces, API allowance,
traffic, storage, AI credits) · any paid add-ons actually needed (region,
managed backup, enterprise assets) · support tier · optional professional
services. Then **implementation services** (partner, companion proposal)
and **third-party estimated costs** (front-end hosting; external TMS;
advanced personalisation tooling). RFPs usually want 3-year TCO — year 1
model plus recurring years 2–3.

## Standing rules for RFP answers
1. Never write "supported out of the box" without naming the plan. Roughly
   a third of the feature matrix is Premium+/Elite-gated.
2. "Unlimited" is usually an Elite word. Premium often has a real numeric
   cap (2 workflows, 10 roles, 20 releases, 5 pipeline stages, 180-day
   retention).
3. Dev/staging/prod as separate spaces → **self-service includes 1 space**,
   so any environment requirement implies Premium/Elite.
4. Quote the SLA against the tier being proposed (97 / 99.9 / 99.99), never
   as a flat number.
5. AI credits are a metered consumption unit with monthly caps — factor
   them in if the prospect plans heavy AI authoring.
6. When procurement asks for financial-stability or insurance detail,
   "we'll cover this during negotiation as the information is confidential"
   is an acceptable, validated answer — not a gap.
