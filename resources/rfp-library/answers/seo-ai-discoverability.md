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

## Native SEO/quality tooling worth naming
- **Broken Links Checker** — a native app that scans and resolves broken
  links across every story in a space. The right answer to "regular scanning
  of redirects and 404s / don't waste crawl budget", and easy to miss because
  it isn't part of the SEO app. **Gate: Premium/Elite** per the official fact
  sheet's Apps table (an earlier note here said Growth Plus+, and a search
  snippet claimed Growth Plus — the fact sheet is the better source; verify
  on the live page if it's decision-relevant). Setup needs an admin to issue
  a preview token.
- **Field-level required validation** in content types — covers
  "enforceable rules before publication" (mandatory meta fields etc.).
  Heading structure (a single H1) stays authoring/template discipline; the
  CMS doesn't enforce it.

## Core Web Vitals levers that ARE platform-side
Most CWV requirements are honestly frontend responsibilities, but two are
genuinely Storyblok's and worth claiming rather than deferring:
- The **Image Service enforces width/height via URL parameters**, directly
  preventing image-driven layout shift (CLS).
- The **component schema can carry a `fetchpriority` field**, letting
  editors mark the hero/LCP element high priority.
Everything else (reserving space for dynamic injects, `font-display`,
below-the-fold lazy loading, preconnect hints, code-splitting/minification)
is frontend build work — say so plainly, then name these two.

## AI / LLM discoverability
Content is stored and delivered as structured, typed JSON — directly
consumable by LLMs and AI crawlers without HTML stripping. Plus a hosted
MCP Server for programmatic/agentic access, and semantic content
intelligence on the roadmap. Note that final rendered HTML quality still
depends on the front-end implementation; don't over-claim.
