# SEO & AI discoverability

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

The honest position: the CMS covers meta/OG natively; canonical, hreflang,
JSON-LD, sitemaps and redirects are **front-end implementation patterns**.
Say so plainly — but always name the delivery route on the prospect's
actual target stack, because "requires custom development" alone reads as
a gap when it's really framework-native work.

| Requirement | Answer | Route |
|---|---|---|
| Meta title/description, OG, per page + per locale | Supported OOTB | Native SEO app |
| Canonical + hreflang | Supported with configuration | Next.js Metadata API (`generateMetadata`) — standard, documented, framework-native |
| Structured data (JSON-LD) | Requires custom development | Front-end generated from the structured content fields |
| SSR/SSG for indexable pages | Supported OOTB | API-first + framework-agnostic; SSR/SSG/ISR is first-class |
| Redirect management for non-developers | Requires custom development | CMS story as redirect data store, queried via API — **plus** on Vercel, native redirect config + Edge Config with a dashboard non-developers can use. Present the combination. |
| XML sitemaps per locale | Requires custom development | `next-sitemap` or a script against the Links API |

## AI / LLM discoverability
Content is stored and delivered as structured, typed JSON — directly
consumable by LLMs and AI crawlers without HTML stripping. Plus a hosted
MCP Server for programmatic/agentic access, and semantic content
intelligence on the roadmap. Note that final rendered HTML quality still
depends on the front-end implementation; don't over-claim.
