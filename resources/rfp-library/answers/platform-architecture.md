# Platform architecture & developer experience

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## Core
API-first/headless: REST Content Delivery API (primary), read-only GraphQL
endpoint, Management API, official SDKs, webhooks. Multi-tenant SaaS —
**no on-premise, dedicated single-tenant or private-cloud option**; strong
separation needs are met via separate spaces + folder structure +
fine-grained access control, not dedicated infrastructure. State this
plainly when asked; don't imply otherwise.

## Framework integration
Official Next.js SDK with dedicated docs and Visual Editor support; large
production deployment base. Framework-agnostic beyond that — any front end
can consume the delivery API.

## Environments & dev workflow
Environments are modelled as **separate spaces** (e.g. dev / staging /
production), promoted via CLI/Management API; a Pipelines app also supports
stage-based promotion within a single space. Page History gives content
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
Official App Directory + first-party Plugin SDK (custom field types, Space
Plugins, OAuth-based), webhooks, Management API. Hosted MCP Server shipped
for agentic/programmatic workflows.
