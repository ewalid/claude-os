# Personalisation & experimentation

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## A/B testing — status corrected 2026-07-30
Native **A/B Testing is GA** with its own product landing page, included at
no extra cost on **Premium and Elite — and available on no self-service
tier at all** (confirmed against the public pricing matrix 2026-07-30). Editors create Experiments, define
Variants (multiple versions of a story), and surface Results from their
existing analytics stack inside the CMS. Marketer-operable end to end:
variant creation, experiment setup, results review, no developer involved.

⚠️ **History worth knowing:** a July 2026 research pass found this as a
"Labs" changelog entry and consequently hedged the answer down to
"Supported with configuration — new, don't over-claim." By the validated
submission it was GA with a dedicated LP. Lesson: a changelog/Labs entry is
a *point-in-time* signal — check for a product landing page and current
plan-inclusion before downgrading a feature's maturity in an answer.

## Audience targeting / personalisation — the honest split
There is **no native audience-segmentation engine**. Audience targeting is
delivered by integrating a specialised tool (VWO, Optimizely; also Segment,
Dynamic Yield for delivery-side variants). The division of labour to state:
**content variants are managed in the CMS; targeting logic lives in the
specialised tool.** Frame it as MACH best-of-breed — each tool doing what it
does best — rather than as a missing feature, but never claim native
segmentation.

Wiring up targeting keys and page instrumentation is genuine
implementation work; "Requires custom development" is the fair answer for a
full audience-variant requirement (F19-style), while marketer-led A/B
testing itself is now OOTB.

## Audience creation from a campaign ID (common demo ask)
Handled by the marketing-automation layer (HubSpot, Segment, etc.), not the
CMS. API + webhooks enable bi-directional flow so campaign data moves
between systems.

## Roadmap adjacency
Vector-based semantic content intelligence (Strata) is roadmap, not GA —
mention as direction of travel only, clearly labelled.
