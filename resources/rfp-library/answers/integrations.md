# Integrations

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## ⚠️ First question, every single time
**Does our product have a native/first-party integration with the
prospect's own product?** If the prospect is a software vendor, check the
app directory and changelog for *their company name* before anything else.

Real case (2026-07): the prospect was a PIM vendor. A first-party plugin
connecting their PIM to our editor existed, shipped, and was documented
publicly — letting content teams browse, filter and embed the prospect's
own product data by channel/locale/category into any story, exposed
through the delivery API for front-end rendering. **No competitor in that
evaluation had a native integration with the prospect's own platform.**
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

## Commerce / PIM ecosystem
First-party plugins exist for several PIM and commerce platforms, plus a
documented e-commerce integration-plugin pattern for anything else.
**Always check the live app directory and changelog rather than relying on
a remembered list** — the list grows, and a plugin for the prospect's own
platform may exist even if it isn't in your recollection.

## Extensibility fallback
Official App Directory + first-party Plugin SDK (custom field types, Space
Plugins, OAuth-based) + webhooks + Management API. Middleware (Zapier,
n8n, Pipedream) covers anything without a native app.
