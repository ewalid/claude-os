# Platform architecture & developer experience

Sources: validated RFP submissions, 2026-07-30 and 2025-11-19
(automotive/AEM-migration, multi-region enterprise). Trust: `validated`.

## Core
API-first/headless: REST Content Delivery API (primary), Management API,
official SDKs and webhooks on **all** tiers. The **GraphQL** Content
Delivery API is **Premium/Elite only** — don't present it as universally
available. Multi-tenant SaaS is the default, and there is still **no
mechanism for hosting customer application code** (see `integrations.md`
for the plugin-hosting limit and what it means for "move our app into the
CMS" requirements) — strong separation needs are normally met via separate
spaces + folder structure + fine-grained access control, not dedicated
infrastructure.

⚠️ **Correction (2025-11-19 validated submission): a BYOC option exists.**
An earlier note here said flatly "no dedicated single-tenant or
private-cloud option, no customer-controlled server." A later validated
submission offers two deployment models: **Public Cloud (SaaS)** —
standard, fully Storyblok-hosted — and **Customer-Controlled Cloud (BYOC)**
— full backend hosting inside the *customer's own* AWS account, still
managed by Storyblok; GCP or Azure hosting can be offered as an option too.
For a prospect with sovereignty/infrastructure-ownership requirements this
is a real answer, not a flat no — but treat it as an enterprise/custom-deal
capability to confirm with the account team before quoting, not a
self-service option.

## Multi-region / multi-brand architecture pattern (enterprise)
Recommended pattern for a large multi-market rollout: **one regional Space
per geography** (e.g. one Space per major region, shared across the
markets in it), with individual markets organized as **folders within that
Space**. A separate **dedicated dev/test Space** hosts centrally-built
components and structural changes, which are then deployed to the regional
Spaces via the CLI. The **Dimensions app** lets a Story's content be cloned
into other market folders while retaining market-specific overrides —
this is the mechanism for "global consistency, regional autonomy."
Component/feature availability can be scoped per-Space and per-market via
roles and permissions. Cite this whenever an RFP asks how the platform
supports multiple regions, brands or country-specific styling at once.

## Scaling — visibility answer
Backend systems run on redundant EC2 instances across multiple AWS regions
and availability zones via AWS Elastic Container Service, with load
balancing inside the VPC and auto-scaling for traffic spikes. Use this when
an RFP specifically asks for "visibility of scaling capability" rather than
just asserting "it scales."

## Transactional vs. non-transactional content — the architecture answer
Storyblok's content model can reference (not duplicate) external
transactional data: a component defines the API call/parameters needed, and
at request time the frontend unifies Storyblok's content/config with
live data from the customer's own systems (pricing, availability, user
info) via API calls — avoiding data duplication and keeping the CMS as the
single source of truth for the *marketing* content only. For pages that are
almost entirely transactional (e.g. a configurator or checkout flow), the
CMS may not be involved at all past a hand-off point in the user journey.
Frame the split as "CMS content vs. transactional data" rather than
implying the CMS stores transactional state.

## Token scoping for multi-market access control
Access tokens can be scoped to a region or folder (not just a whole Space),
letting a central team grant a market or agency editor access only to their
own folder/Story while enforcing global consistency centrally. Cite
alongside the RBAC/custom-roles material in `security-compliance.md` for
"how do you restrict who edits what by country/brand/market" questions.

## Framework integration
Official Next.js SDK with dedicated docs and Visual Editor support; large
production deployment base. Framework-agnostic beyond that — any front end
can consume the delivery API.

## Environments & dev workflow
Environments are modelled as **separate spaces** (e.g. dev / staging /
production), promoted via CLI/Management API. **Self-service plans include
exactly 1 space**, so any real environment requirement implies
Premium/Elite — state this rather than describing environments as freely
available. **Pipelines / content staging is also Premium/Elite only**
(5 stages on Premium, unlimited on Elite). Page History gives content
versioning out of the box. Schema-as-code (TypeScript, version-controlled)
covers content-model-in-CI.

**Be precise about the CI/CD boundary:** app-code CI/CD lives in the
customer's own repo and hosting pipeline, not in the CMS; the CMS provides
webhooks to trigger external builds. Answering "Supported out of the box"
for CI/CD overstates it — "Supported with configuration" plus this
boundary sentence is the accurate answer.

## Hosting
- **Vercel** — official integration, available on all plan tiers,
  dedicated docs, production customers to cite. High confidence.
- **Containerised hosting (GCP Cloud Run, etc.)** — architecturally
  compatible since the CMS is API-first and hosting-agnostic, but **no
  official partnership or dedicated integration.** State the confidence
  difference between these two plainly rather than blurring them; a
  prospect naming both will notice.

## Extensibility
Official App Directory + first-party Plugin SDK, webhooks, Management API.
Hosted MCP Server shipped for agentic/programmatic workflows.
**Plugin gating: Field Plugins on all tiers; Space Plugins and Tool
Plugins are Premium/Elite only.**
