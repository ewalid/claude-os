# claude-os

<table>
<tr>
<td width="60%" valign="top">

**Darwin** — a personal AI assistant for a Solutions Engineer, built on Claude
Cowork. It runs the operational layer around a sales cycle (briefings, account
intelligence, demo prep, RFP answers, Notion hygiene, call coaching) so the SE
can focus on selling and engineering.

It shows up on its own cadence, not just on request: a **daily briefing** every
morning (demos first, then deadlines, Slack worth claiming, relevant email, a
one-liner), a **weekly review** of the whole pipeline (what moved, what
stalled, what's due, RFP deadlines stacking up), and a **monthly review** that
audits Darwin's own behavior for recurring friction and fixes it at the root
instead of patching the same problem twice.

Underneath that cadence sits the actual work: `process-customer` builds and
refreshes account briefs from real evidence and keeps the Notion CRM row
honest end to end; `build-deck`/`demo-script`/`demo-setup` turn a brief into a
brand-consistent deck, a talk track, and a Storyblok space setup; `rfp-answer`
retrieves and harvests answers against a trust-tagged library so nothing
customer-facing is ever guessed; `call-coach` and `post-call-update` turn every
call into quotable feedback and a closed-loop debrief; and a live dashboard
artifact keeps the whole pipeline and what the team is working in one place.
Every skill is evidence-first — if there's no real source for a claim, Darwin
says "need validation" rather than inventing one.

Darwin is the sweetest cat ever, he's very good boy. A seasoned Solution Engineer.

</td>
<td width="40%">

<img src="resources/darwin.jpg" alt="Darwin">

</td>
</tr>
</table>

## Start here
- **[SKILLS.md](SKILLS.md)** — what Darwin can do: every skill, when it fires,
  what it needs. Read this first.
- **[CLAUDE.md](CLAUDE.md)** — Darwin's identity and hard guardrails (always
  loaded).
- **[HANDOFF.md](HANDOFF.md)** — design history and rationale.
- **[ROADMAP.md](ROADMAP.md)** — phased build plan and skill roster.
- **[CHANGELOG.md](CHANGELOG.md)** — what changed and when.

## Using Darwin on your own machine/account
On a fresh clone there's no `memory.md`, so Darwin runs **`darwin-setup`**
automatically the first time — it introduces itself and interviews you to make
the repo yours (identity, team, connectors, Notion/Slack IDs). Everything is
tuned to you from there.

## Privacy
This repo contains **zero real client or account names** and is safe to share.
Customer evidence, per-account files, rolling memory, and private coaching
notes are all git-ignored and stay local to each person's machine. See
`CLAUDE.md` guardrail 6.
