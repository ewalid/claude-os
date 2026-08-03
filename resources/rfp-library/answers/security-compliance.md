# Security & compliance

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## Certifications — the settled answer
- **ISO 27001** certified (TÜV Rheinland, valid to 2028-03-03).
- **SOC 2 Type I** certified. **Never claim Type II** — third parties
  repeatedly get this wrong; do not repeat the error.
- **TISAX** certified.
- Regular internal and third-party penetration testing; dedicated incident
  response team with severity-based escalation and root cause analysis.
- Public trust centre is the citable source.

### ⚠️ SOC 2: two validated submissions contradict each other — do NOT pick a side
- One validated submission (2026-07-30) states **ISO 27001 + SOC 2 Type I +
  TISAX**, and explicitly warns that third parties keep wrongly claiming
  Type II.
- Another validated submission (2026-07, a requirements grid) states
  **"ISO 27001, SOC 2 Type II certifications."**
- Earlier research found the *public* trust-centre page listing only ISO
  27001 + TISAX, with no SOC 2 mention at all.

Three sources, three answers, all "validated" or official. Overclaiming a
SOC 2 type in a regulated prospect's security review is a serious problem,
so **confirm with the security team before any submission repeats either
figure.** Until then, cite ISO 27001 (which all sources agree on) and say
the SOC 2 report status will be confirmed in writing.

## SSO / provisioning
SSO: Auth0, Okta, Microsoft Entra ID, Google Workspace, OneLogin,
JumpCloud, Salesforce, SAML 2.0 and SAML 1.0. SCIM 2.0 provisioning via
Okta and Entra ID (auto-provision, role sync, de-provision).

**Plan gating (corrected 2026-07-30):** SSO, SCIM and **Forced** 2FA are
all **Premium/Elite only** — not on any self-service tier. Plain 2FA is on
all tiers. ⚠️ A validated submission called SSO an Enterprise *add-on*
while the public matrix lists it as included on Premium/Elite; unresolved,
see `pricing-licensing.md`. Ask the AE before restating either way.

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

## Honest limit: no customer-facing infrastructure monitoring
**Server/infra-level metrics, tracing and alerting dashboards are not
exposed to customers** — that is Storyblok's internal ops concern. Content-
level auditing is covered (Activity Log on all plans; longer retention on
enterprise tiers), and Storyblok monitors its own infrastructure, but a
prospect asking for a self-service CPU/memory/response-time dashboard gets
a *partial*, not a yes. The public status page plus the uptime SLA are the
customer-visible surface. Validated answer, twice.

## Residency / DR / SLA
EU residency by default (AWS Frankfurt). **Region choice ("Preferred Data
Centers": EU/US/Canada/Australia, China on isolated infra) is a paid add-on
on Premium and included on Elite** — not a free universal setting. DPA is
published publicly and also downloadable in-app. Hot-standby read replica
with automatic failover; daily backups to S3 with 14-day transaction log
retention and point-in-time restore. Enterprise customers can run
self-controlled daily backups to their own bucket. Full programmatic export
via the APIs at any time (data-portability answers).

⚠️ Backup retention: the validated submission says **14-day** transaction
log retention; earlier research had 30-day. Confirm against the trust
centre before restating, don't average them.

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
