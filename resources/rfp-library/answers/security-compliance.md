# Security & compliance

Sources: official Trust Center + the dedicated ISO-27001/enterprise-security
page, both checked directly **2026-08-03** (trust level `official-docs`),
plus two validated RFP submissions (`validated`). Where they disagree, the
official pages win — see the SOC 2 note below.

## Certifications — what is safe to claim
Confirmed on Storyblok's own official pages:
- **ISO 27001 certified** — the certificate PDF is publicly downloadable
  from storyblok.com/enterprise-security, and the Trust Center links TÜV.
  (A validated submission adds: TÜV Rheinland, valid to 2028-03-03.)
- **TISAX** — listed under ISMS Policies & Certifications on the Trust
  Center.
- **GDPR compliant**, and **self-certified under the EU-U.S. Data Privacy
  Framework** (Trust Center, Data Protection & Privacy).
- OWASP secure-coding adherence, AWS WAF, AWS GuardDuty (AI-based intrusion
  detection), Detectify continuous automated security testing, regular
  internal *and* third-party penetration testing, dedicated incident
  response team with severity-based escalation and root cause analysis.

### ⚠️ SOC 2 — do NOT claim it. Verified 2026-08-03.
Checked both authoritative public pages directly:
- **Trust Center** (`/trust-center`) — ISMS section lists **ISO 27001 and
  TISAX only. No SOC 2 mention anywhere on the page.**
- **`/enterprise-security`** — the page is literally titled "ISO 27001
  Certified CMS"; its "Security standards & certificates" section lists
  exactly ISO 27001, OWASP, GDPR, WAF, AWS GuardDuty. **No SOC 2 in any
  form.**

So **neither** validated submission's SOC 2 claim is supported by official
material: one asserted **Type I**, the other asserted **Type II** (and the
first explicitly warned that third parties keep wrongly claiming Type II —
a warning the second submission then walked straight into).

**Standing rule until security says otherwise:** answer certification
questions with ISO 27001 + TISAX + GDPR/EU-U.S. DPF, and state that SOC 2
report availability will be confirmed in writing. It is entirely plausible
that a SOC 2 report exists but is NDA-only and therefore unmarketed — that
is exactly why it needs confirming rather than asserting. **Claiming Type II
in a regulated prospect's security review, unsupported, is a serious
problem.** Escalate to the security team; do not resolve it from documents.

## SSO / provisioning
SSO: Auth0, Okta, Microsoft Entra ID, Google Workspace, OneLogin,
JumpCloud, Salesforce, SAML 2.0 and SAML 1.0. SCIM 2.0 provisioning via
Okta and Entra ID (auto-provision, role sync, de-provision).

**Plan gating:** SSO, SCIM and **Forced** 2FA are all **Premium/Elite
only** — not on any self-service tier. Plain 2FA is on all tiers.

✅ **"Is SSO an add-on?" — resolved 2026-08-03.** It is **included in the
user licence** on Premium and Elite. The official fact sheet's legend
distinguishes ✔ (included) from $ (additional fee), and SSO carries ✔ on
both enterprise tiers; its Security Access row reads Premium = "Standard
roles +SSO". A validated submission had called SSO an Enterprise add-on —
that was imprecise. See `pricing-licensing.md` for the genuine paid add-ons.

## RBAC / audit
Admin + Editor roles on all plans. **Custom roles are Premium/Elite only
(10 on Premium, unlimited on Elite).** Scoping by space, language, content
type and workflow stage comes with custom roles.

**Folder / block / datasource / asset-folder scoping and Access Token
Scopes are Elite ONLY** (the "Additional Access Controls" row), as is
restricted IP range and security audit. An earlier draft presented
fine-grained scoping as generally available — it isn't. Activity Logs are
encrypted (AES-256 at rest, TLS 1.3 in transit) and access-restricted.
**Retention: 1 day Starter / 30 days Growth & Growth Plus / 180 days
Premium / unlimited Elite** — "retained indefinitely on Enterprise" is
wrong, only Elite is unlimited.

## Contracting detail worth knowing (Trust Center)
There are **two separate GTCs**: one for enterprise customers in the
**Americas** region, and one for global self-service customers plus
enterprise customers in **all other regions**. Point procurement at the
right one. Separate terms also exist for AI-powered features. DSA (EU
2022/2065) and DMCA compliance are both stated.

## Honest limit: no customer-facing infrastructure monitoring
**Server/infra-level metrics, tracing and alerting dashboards are not
exposed to customers** — that is Storyblok's internal ops concern. Content-
level auditing is covered (Activity Log on all plans; longer retention on
enterprise tiers), and Storyblok monitors its own infrastructure, but a
prospect asking for a self-service CPU/memory/response-time dashboard gets
a *partial*, not a yes. The public status page plus the uptime SLA are the
customer-visible surface. Validated answer, twice.

## Residency / DR / SLA
EU residency by default (AWS Frankfurt, Germany). **Standard selectable
regions: EU, US, Canada, Australia** — dedicated AWS infrastructure, per the
Trust Center and the official fact sheet.

**China IS officially offered — correction, 2026-08-03.** An earlier note
here said China wasn't available because the Trust Center lists only the
four standard regions. The official fact sheet is explicit: **"Additional
Data Center (China)"** is a **paid add-on on both Premium and Elite**, with
dedicated Mainland China infrastructure, **an ICP license already in place**
for international customers, and a custom domain (`app.storyblokchina.cn`).
This is a real, sellable answer for a prospect with a China requirement —
just priced separately. Lesson: the Trust Center lists the *standard*
regions; the fact sheet lists the *purchasable* ones.

**Region choice ("Preferred Data Center Selection") is a paid add-on on
Premium and included on Elite** — not a free universal setting.

DPA is published publicly and also downloadable in-app. Hot-standby read
replica with automatic failover; 14-day transaction log retention with
point-in-time restore; regular backup testing. **Self-managed S3 backups to
the customer's own bucket are Premium/Elite** (weekly on Premium, daily on
Elite); a **Managed Backup Service** is a paid add-on on both (Premium
weekly/180 days, Elite daily/up to 7 years). Full programmatic export via
the APIs at any time (data-portability answers).

✅ Backup retention **resolved 2026-08-03**: the Trust Center states
**"14-Day Transaction Log Retention — restore to any point in time within
the last 14 days"**, plus daily customer-managed backups in Amazon S3 and
regular backup testing. The validated submission's 14 days was right; the
earlier 30-day figure was wrong. Use 14.

## Accessibility (WCAG)
Two dimensions, and they must be separated:
1. **Delivered front-end** — conformance is fully achievable through the
   implementation; the CMS does not block it. It is the implementation
   team's responsibility, not a platform guarantee.
2. **The editor app itself** — targets WCAG 2.2 AA, independent audit by
   AxessLab completed, full-time Accessibility Engineer on staff, design
   system audited, a small number of edge cases remain. The official
   published statement uses the words "partially compliant"; the validated
   framing is "largely compliant, audit completed, edge cases remain."
   Use the validated framing, but never claim full conformance.
