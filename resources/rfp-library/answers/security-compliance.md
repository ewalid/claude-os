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

Note: an earlier draft flagged a conflict here because an unofficial
internal library said SOC 2 Type I while the public trust-centre page
listed only ISO 27001 + TISAX. Resolved in the validated submission as
ISO 27001 + SOC 2 Type I + TISAX. If the public page and internal
material disagree again, ask the security team rather than picking one.

## SSO / provisioning
SSO: Auth0, Okta, Microsoft Entra ID, Google Workspace, OneLogin,
JumpCloud, Salesforce, SAML 2.0 and SAML 1.0. SCIM 2.0 provisioning via
Okta and Entra ID (auto-provision, role sync, de-provision). Forced 2FA on
all plans. **SSO is an Enterprise add-on, not bundled** — price it.

## RBAC / audit
Permissions scope by space, language, content type, workflow stage, folder
and asset library. Admin + Editor on all plans; custom roles with
fine-grained config on Premium+. Activity Logs encrypted (AES-256 at rest,
TLS 1.3 in transit), access-restricted, exportable for external archiving;
Extended Activity Log on Enterprise.

## Residency / DR / SLA
EU residency by default (AWS Frankfurt); region configurable
(EU/US/Canada/Australia); DPA published publicly. Hot-standby read replica
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
