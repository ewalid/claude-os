# Security & compliance

Sources: three official pages checked directly on **2026-08-03** —
`/trust-center`, `/trust-center/security`, `/enterprise-security` — plus the
official public fact sheet (all `official-docs`), and two validated RFP
submissions (`validated`). Where they disagree the official pages win, and
note that `/trust-center/security` is visibly the **oldest** of the three
(see the retention and TLS discrepancies below) — prefer `/trust-center`.

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

### ⛔ SOC 2 — RESOLVED 2026-08-03: it is **AWS's** certification, not Storyblok's
Three official pages checked directly:
- **`/trust-center`** — ISMS section lists **ISO 27001 and TISAX only**. No
  SOC 2.
- **`/enterprise-security`** — titled "ISO 27001 Certified CMS"; its
  "Security standards & certificates" section lists exactly ISO 27001,
  OWASP, GDPR, WAF, AWS GuardDuty. No SOC 2.
- **`/trust-center/security`** — **this is where the confusion comes from.**
  Its Data Protection section reads: *"The solution is hosted on Amazon AWS
  in Frankfurt/Germany **which has** various security certificates like: SOC
  1/SSAE 16/ISAE 3402, **SOC 2**, SOC 3, FISMA/DIACAP/FedRAMP, DOD CSM 1-5,
  PCI DSS Level 1, ISO 9001/ISO 27001, ITAR, FIPS 140-2, MTCS Level 3."*

That list belongs to **AWS**, the hosting provider — the sentence subject is
Amazon AWS, not Storyblok. Storyblok has **no SOC 2 certification of its
own** on any official page.

**This almost certainly explains the bad answer in a validated submission**
("Storyblok holds ISO 27001, SOC 2 Type II certifications") — someone read
AWS's certificate list as Storyblok's. The other submission's "SOC 2 Type I"
looks like the same misread, softened.

**Standing rule:** answer certification questions with **ISO 27001 + TISAX +
GDPR + EU-U.S. Data Privacy Framework** as *Storyblok's own*, and, if useful,
cite AWS's certifications separately and explicitly as **the hosting layer's**
(that framing is legitimate and often reassuring in a security review).
**Never present SOC 2 — of any type — as Storyblok's own certification.**
If a prospect specifically requires a vendor SOC 2 report, that is a real
question for the security team, not something to answer from the website.

## SSO / provisioning / token scoping
SSO: Auth0, Okta, Microsoft Entra ID, Google Workspace, OneLogin,
JumpCloud, Salesforce, SAML 2.0 and SAML 1.0. SCIM 2.0 provisioning via
Okta and Entra ID (auto-provision, role sync, de-provision).

**Scoped Personal Access Tokens (PATs)** — apply least-privilege to
integrations, automation workflows and developer tooling by limiting a token
to only the permissions it needs. Worth naming in any API-security answer.

Internal network access is public/private key pair over SSH only; access is
monitored by automated tooling with brute-force mitigation, and every content
change is logged to the user activity event stream.

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

## Change management (useful in security questionnaires)
Documented process for customer-impacting changes: **peer-reviewed**
(technical review required), **tested** (verified not to adversely impact
security), and **approved** (all changes authorised for oversight of business
impact). Unusual/malicious traffic is detected by CloudWatch alarms with
notification to a responsible employee.

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
replica with automatic failover; point-in-time restore within the
transaction-log window (see the retention caveat below); regular backup
testing. **Self-managed S3 backups to
the customer's own bucket are Premium/Elite** (weekly on Premium, daily on
Elite); a **Managed Backup Service** is a paid add-on on both (Premium
weekly/180 days, Elite daily/up to 7 years). Full programmatic export via
the APIs at any time (data-portability answers).

⚠️ **Backup retention: the two official pages disagree — 14 vs 30 days.**
- `/trust-center` (newer): **"14-Day Transaction Log Retention** — restore to
  any point in time within the last 14 days."
- `/trust-center/security` (older): daily S3 backup "with a changelog for a
  **retention period of 30 days**. Restoring is possible at any point in time
  within 30 days."

`/trust-center/security` shows clear staleness tells — it still says APIs use
**TLS 1.2** and carries a note about TLS 1.1 being disabled "after
01.12.2021", while the main Trust Center states **TLS 1.3**. So the 14-day
figure is probably current and the 30-day page probably out of date — but
**don't state either as fact in a security review without confirming**;
retention windows are exactly the kind of number a regulated prospect will
hold you to. Also documented: **monthly recovery tests** covering
point-in-time database recovery and asset recovery via S3 versioning.

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
