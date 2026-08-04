# RFP answer library — index

Two layers:

**`answers/<category>.md`** — tracked in git. Reusable product-positioning
answers, scrubbed of every client/stakeholder/deal specific (guardrail 6).
This is the moat: each RFP should leave this library better than it found it.

**`validated-submissions/`** — **gitignored** (2026-07-30, guardrail 10).
The real, as-submitted documents. They name real clients, stakeholders,
partner firms and commercial specifics, so they never go to git — but they're
the highest-trust source there is, because they're what the operator actually
put their name on. Read the most recent one for a category before drafting
anything new.

## Trust ranking for anything in here
1. **The operator / the owning internal team** — the only way to settle an
   internal fact the public web doesn't cover (see lesson 8).
2. **`official-docs`** — the public pages and fact sheet. Best for plan
   gating, quotas, SLA, published capabilities.
3. **`validated`** — survived the operator's review and went out the door.
   Excellent for *framing* and commercial judgement, and it carries real
   nuance the docs don't. But **not infallible on facts**: of the four
   validated submissions harvested so far, three contained a wrong or
   misattributed certification claim and one mis-stated a licensing detail.
   Trust the framing; verify the figures — and see standing lesson 9 below,
   the certification error specifically has now recurred twice with the
   same root cause.

## Categories
| File | Covers |
|---|---|
| `platform-architecture.md` | API/headless architecture, environments, dev workflow, hosting, extensibility |
| `editorial-experience.md` | Visual Editor, components, workflows, versioning, scheduling, AI authoring |
| `localisation.md` | i18n model, fallbacks, TMS/AI translation |
| `seo-ai-discoverability.md` | Meta/canonical/hreflang, structured data, sitemaps, redirects, LLM/AI readiness |
| `personalisation-experimentation.md` | A/B testing, audience targeting, variants |
| `integrations.md` | Forms/CRM/MAP, analytics, consent, PIM/commerce, middleware patterns |
| `security-compliance.md` | Certifications, SSO/SCIM, RBAC, residency, SLA, backups, DR |
| `migration.md` | Migration tooling/methodology, SEO preservation, freeze/rollback |
| `dam-assets.md` | Asset Manager, Image Service, asset governance/distribution, DAM partials |
| `company-credentials.md` | Company facts, funding, partnerships, case studies, references |
| `pricing-licensing.md` | Licensing structure, plan gating, TCO framing |

## Standing lessons (learned the expensive way — read before drafting)
1. **Plan tier gates features — read `pricing-licensing.md` FIRST, before
   answering any capability question.** It holds the full transcribed
   public plan matrix (tier names, quotas, and the ~third of the feature
   set that is Premium+/Elite-only). Never answer "supported out of the
   box" without naming the plan; a capability answer without its tier is
   half an answer and creates commercial risk downstream. "Unlimited" is
   usually an Elite word — Premium often has a real numeric cap. The
   pricing page's own integration matrix is also the fastest way to check
   whether an integration with the prospect's platform exists at all.
2. **Check whether the product natively integrates with the prospect's own
   product.** See `integrations.md` — this was missed once on an RFP where a
   first-party integration with the prospect's own platform existed and was
   the single strongest differentiator available. Requirement-driven research
   will never surface it, because it's never a line item.
3. **A "gap" is usually a path.** Before writing "Requires custom
   development," ask what the *actual delivery route* is on the target stack.
   Naming the route ("embed the existing forms directly — zero CMS
   dependency") is both more accurate and more useful than naming the
   absence.
4. **Answer the metric, not just the feature.** If the RFP states success
   metrics, tie capabilities back to them explicitly.
5. **A well-written "partial" beats a padded "yes".** The most reusable
   content in this library came from the *partial* rows of a validated
   requirements grid — each one names the precise limit and then the bridge
   ("no native X; achievable via Y, which is additional implementation").
   Copy that shape. Never inflate a partial into a yes; never leave a
   partial as a bare no.
6. **Some answers are legitimately "not ours".** Frontend/implementation-
   partner responsibilities (PWA shell, responsive build, WCAG at site
   level, lazy loading, code splitting) should be stated as such, paired
   with what the platform *does* contribute. A validated grid used this
   framing consistently across ~250 requirements without losing the deal.
7. **Cross-reference instead of repeating.** In a large grid, answering
   "same pattern as TB0024" is normal, accepted practice and keeps the
   response readable. Write the canonical answer once.
8. **"Not on the website" ≠ "not true" — and over-correcting is its own
   error.** Public pages are the right way to *check* a claim, never the way
   to *settle* one about our own company. Compliance reports, internal
   roadmap status and commercial terms are routinely unpublished by design.
   Real case (2026-08-03): three official pages didn't mention a
   certification, so this library briefly asserted we didn't hold it — the
   operator corrected it (we hold Type I, Type II in progress). That
   understatement would have damaged us in a security review just as much as
   the overstatement it was "fixing". When the web is silent on an internal
   fact, write "confirm with <team>" and ask a human — don't infer absence.
9. **A wrong certification claim isn't a one-off — it recurred, verbatim,
   in a second validated submission.** The exact "hosted on AWS which has
   [SOC 2, FedRAMP, PCI DSS L1...]" paragraph — whose actual subject is
   AWS's certificates, not Storyblok's own — was copied into a validated
   RFP response a second time (2025-11-19), still attributing AWS's
   certifications to Storyblok. Treat this specific paragraph as
   contaminated wherever it's found in source material, and rewrite it
   explicitly as "our infrastructure provider AWS holds..." every time,
   rather than assuming a previously-validated document already fixed it.
10. **Hosting-model answers can go stale fast.** One validated submission
    stated flatly "no dedicated/private-cloud option, no customer-controlled
    server." A later validated submission (2025-11-19) offered a genuine
    **BYOC (customer-controlled cloud)** option — full backend hosting in
    the customer's own AWS/GCP/Azure account, still Storyblok-managed.
    Re-check this specific claim against the latest validated source or the
    account team before repeating an older "no such option" answer.
11. **A validated submission can be prose, not a grid — match what's asked.**
    The Nissan submission (2025-11-19) was a narrative response organised
    around the RFP's own "five key dimensions" (migration, integration,
    operating model, product vision, AI strategy) rather than a line-item
    requirements grid. Confirms standing lesson from `rfp-answer`'s own
    guidance: read the incoming document's own structure and vocabulary
    before drafting, rather than defaulting to a grid format.
