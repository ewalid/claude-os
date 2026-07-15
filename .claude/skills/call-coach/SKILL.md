---
name: call-coach
description: >
  Trigger: "coach me on [account] call", "how'd I do on [X]", or Walid
  pastes a Gong transcript/notes and asks for feedback. Gong itself is
  not connected — this only ever works from what Walid pastes in.
---

# call-coach

## What it does

Turns a call transcript or notes into concrete, quotable feedback —
not generic sales-coaching platitudes. Every strength and every
improvement point must trace back to an actual line in what Walid
pasted. Feeds `resources/coaching-log.md` (private, never customer-
facing — CLAUDE.md guardrail 7) so patterns compound over time instead
of resetting every call.

## Steps

1. **Get the material.** Walid pastes a Gong transcript or his own
   notes, and names the account/call. If he only names the call with
   no content, ask him to paste the transcript/notes — never coach
   from a guess about what was probably said.

2. **Get the call's goal.** Check `accounts/<customer>/brief.md` (from
   `process-customer`) for what this call was supposed to accomplish.
   If there's no brief or no stated goal, ask Walid what the goal was
   before critiquing — "did well" is meaningless without knowing what
   the call was for.

3. **Identify 3-5 things done well**, each one quoting or closely
   paraphrasing an actual moment from the transcript — not "good
   rapport-building" in the abstract, but the specific exchange that
   showed it.

4. **Identify 3-5 things to improve**, same rule — quote the moment,
   explain what a stronger version would have looked like. Be direct;
   softening this to be nice defeats the purpose (CLAUDE.md voice: no
   flattery, no padding).

5. **Pick ONE focus for the next call.** Not three, not five — the
   single highest-leverage thing. If multiple issues showed up, pick
   the one blocking the deal's progress, not just the easiest to fix.

6. **Append to `resources/coaching-log.md`**:
   ```
   ### <date> — <account> call
   Goal: <call goal from brief, or "not stated — asked Walid">
   Did well:
   - ...
   To improve:
   - ...
   Focus for next call: <the one thing>
   ```

7. **Never surface this outside chat + coaching-log.md.** Not in
   account briefs, not in Notion, not in anything Walid might later
   paste into a customer-facing doc.

## Guardrails

- Gong is not connected — never claim to have "pulled the transcript,"
  only ever work from what Walid pasted.
- `resources/coaching-log.md` is private (CLAUDE.md guardrail 7) —
  never quote it in account briefs, RFPs, or anything customer-facing.
- If the call's goal isn't documented anywhere, say so and ask, rather
  than inventing a plausible-sounding one.
- Directness over comfort — vague encouragement isn't coaching.
