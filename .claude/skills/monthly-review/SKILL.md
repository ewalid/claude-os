---
name: monthly-review
description: >
  Trigger: "run a monthly review", "audit what we've done", "how are
  you doing overall", or a natural monthly cadence. Mines memory.md and
  CHANGELOG.md for recurring patterns across corrections/fixes, not
  just a list of what happened — the thing `darwin-improve`'s
  "Compounding" section says should exist, formalized as a real skill
  (2026-07-20: first real run was a manual, ad hoc audit requested by
  Walid — this skill exists so that doesn't have to be manually asked
  for every time).
---

# monthly-review

## What it does

Steps back from individual sessions and looks for patterns across many
of them: corrections that keep recurring in slightly different forms,
categories of mistake that show up more than once, skills that get
used constantly vs. never, gaps that keep getting flagged but never
fixed. A single correction is a `darwin-improve` job. A *pattern* of
corrections is this skill's job — and per `darwin-improve`'s own
Compounding note, a repeating pattern is a signal CLAUDE.md itself
needs a clearer rule, not just another one-off patch.

## Steps

1. **Read `memory.md` in full** (all session entries since the last
   review, or the whole file if this is the first run) **and
   `CHANGELOG.md` in full.** Don't sample — a pattern hiding in an
   entry you skipped is exactly what this skill exists to catch.

2. **Group friction by category**, not by date. Look specifically for:
   - The same *kind* of mistake recurring under different names (e.g.
     2026-07-20 had the same "new file holds real data, wasn't
     gitignored" mistake three separate times before it was named as
     one pattern and fixed at the root).
   - Skills drafted but never actually run for real (check against
     `ROADMAP.md`'s checkmarks vs. actual usage in `memory.md`).
   - Resources referenced by skills but still empty/thin (e.g.
     `resources/battle-cards/`).
   - Guardrails or fixes that got added once but may have quietly
     drifted since (e.g. a commit-prefix convention not actually
     followed in later commits — check `git log` against what
     CLAUDE.md says the convention should be).

3. **For each real pattern found**, don't just report it — run
   `darwin-improve` on it (name that it's running, reread that skill's
   steps, classify where the fix belongs, apply, commit as `improve:`,
   log in `CHANGELOG.md`/`memory.md`). A pattern spotted but not fixed
   isn't a review, it's just a list.

4. **Report back**: what patterns were found, what got fixed as a
   result, and what's flagged as backlog rather than fixed now (ask
   Walid to prioritize backlog items rather than silently deciding for
   him — this is exactly the kind of judgment call that needs a real
   checkpoint, not an assumption, per the same lesson `demo-script`
   and `build-deck` already learned this session).

5. **Never fabricate a pattern to have something to report.** If the
   month was genuinely clean, say so — this skill exists to catch real
   compounding friction, not to manufacture busywork.

## Guardrails

- This skill never overrides CLAUDE.md's hard guardrails — a pattern
  found here still routes through `darwin-improve`'s normal
  classification, including the confirm-before-acting checkpoints
  those fixes have already earned.
- Stay at the meta level when describing patterns that touch customer
  accounts (CLAUDE.md guardrail 6) — "the same gitignore gap recurred
  across three resource types" is fine to say; repeating the actual
  customer specifics that were at risk is not.
