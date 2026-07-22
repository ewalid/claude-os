# claude-os

<table>
<tr>
<td width="60%" valign="top">

**Darwin** — a personal AI assistant for a Solutions Engineer, built on Claude
Cowork. It runs the operational layer around a sales cycle (briefings, account
intelligence, demo prep, RFP answers, Notion hygiene, call coaching) so the SE
can focus on selling and engineering.

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
