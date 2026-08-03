# Integrations

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## ⚠️ First question, every single time
**Does our product have a native/first-party integration with the
prospect's own product?** If the prospect is a software vendor, check the
app directory and changelog for *their company name* before anything else.

Real case (2026-07): the prospect was itself a software vendor in a
category we already integrate with. A first-party plugin connecting their
own platform to our editor existed, shipped, and was documented publicly —
letting content teams browse, filter and embed the prospect's own product
data by channel/locale/category into any story, exposed through the
delivery API for front-end rendering. **No competitor in that evaluation
had a native integration with the prospect's own platform.**
It was the strongest differentiator in the entire response and the
first-pass research missed it completely, because it wasn't a requirement
line item. Requirement-driven research structurally cannot find this.

## Forms / CRM / MAP — the "no native form builder" answer
There is no dedicated form-builder app. That is **not** a dead end, and
should not be answered as "requires custom development" without naming
the route. Two proven paths:
- **(a) Lowest effort:** embed the prospect's existing marketing-automation
  forms directly in the front end via their JS embed. Existing forms keep
  working as-is, zero CMS dependency, zero re-plumbing of lead routing.
- **(b) More editor control:** model form fields as reusable components
  (text input, dropdown, checkbox) that editors assemble visually;
  front-end handles validation and submission to the MAP/CRM/any endpoint.

Answer as **Supported with configuration**, presenting both paths.

## Analytics / tag management / consent
GA4, GTM, Hotjar, Cookiebot and equivalents are front-end script/tag
embeds in the app shell — identical to any headless front end, no CMS-side
blocker. Existing GTM containers are retained. Answer: Supported with
configuration.

Known third-party gotcha worth flagging proactively: Hotjar's own GA4
integration does not work when GA4 is deployed via GTM. Unrelated to us,
but flagging it earns credibility.

## Commerce / PIM / TMS ecosystem — and the plan floor it sets
First-party plugins exist for a long list of PIM, commerce and TMS
platforms, plus a documented e-commerce integration-plugin pattern for
anything else. **Always check the live app directory, the changelog AND the
pricing-page integration matrix** rather than relying on a remembered list.

Two things the pricing matrix tells you that the app directory doesn't:
1. Nearly every PIM/commerce/TMS connector is **Premium/Elite gated**
   (only Figma, Netlify, Vercel are on all tiers; Bynder, Cloudinary,
   Optimizely, Semrush, Shopify, Slack from Growth up). If the prospect's
   integration is central to the deal, it sets a **pricing floor** — raise
   that early, not in negotiation.
2. It is a fast way to check whether an integration with the prospect's own
   platform exists at all — the 2026-07 miss was listed right there in the
   integrations section of the public pricing page.
See `pricing-licensing.md` for the full gating list.

## Extensibility fallback
Official App Directory + first-party Plugin SDK (custom field types, Space
Plugins, OAuth-based) + webhooks + Management API. Middleware (Zapier,
n8n, Pipedream) covers anything without a native app.
