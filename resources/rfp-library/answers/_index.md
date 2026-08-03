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
`validated` — it survived the operator's review and went out the door.
Higher trust than official docs, because it reflects both the facts *and*
how the operator chooses to frame them commercially.

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
