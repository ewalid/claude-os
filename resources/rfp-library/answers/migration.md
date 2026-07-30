# Migration

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## Real tooling — name it, don't describe it generically
- **Official WordPress importer** exists as a public repo starting point.
  Check for an official importer for the prospect's *current* CMS before
  describing migration as bespoke scripting.
- **CLI v4** + Management API: scripted, re-runnable jobs with dry runs,
  idempotency guarantees, and manifest-based reference mapping — designed
  exactly for the "multiple rehearsal passes before cutover" pattern most
  RFPs require.
- **Schema-as-code package**: content model defined in TypeScript,
  version-controlled, CI/CD-friendly, pushed repeatedly to a staging space
  for validation. Strong answer for "scripted and repeatable."

## Methodology skeleton
1. Discovery & content audit — full page/template/content-type inventory,
   localised variants, URL audit against current sitemap + analytics.
2. Content model design — translate the old flexible-content structures
   into components; proof-of-modelling on one representative page first
   (this usually maps directly to the RFP's own demo requirement).
3. Scripted migration — importer/CLI/API, multiple dry passes before
   cutover, sized against the stated freeze window.
4. Redirects & SEO preservation — 301 map every indexed URL, metadata and
   structured-data parity checks, CWV validation, analytics QA.
5. Launch & hypercare — DNS cutover, defined hypercare window, decommission
   the old platform.

## Content freeze — don't just call it "not a platform feature"
It *is* a process commitment, but the platform can enforce it operationally:
lock editors to a read-only workflow stage, or temporarily restrict
publishing permissions via role config for the freeze period. Answer
**Supported with configuration**, not "Not supported."

## Risk mitigation plan
Owned by the implementation partner as a methodology deliverable — but
enumerate the platform's contributions rather than deferring wholesale:
version history + content rollback, automated daily backups with
point-in-time restore, hot-standby replica for HA, published incident
response process.

## Consultative flag worth making
If a prospect's timeline puts content migration in a single late phase,
recommend beginning dry-run migrations during the *build* phase instead —
it surfaces content-model problems early. Flagging a realistic risk in
their own plan reads as expertise, not obstruction.
