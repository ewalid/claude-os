---
name: call-coach
description: >
  Trigger: "coach me on [account] call", "how'd I do on [X]", or the operator
  pastes a Gong transcript/notes and asks for feedback. Pulls the
  transcript from Gong MCP if that connector is available this session;
  otherwise works only from what the operator pastes in.
---

# call-coach

## What it does

Turns a call transcript or notes into concrete, quotable feedback —
not generic sales-coaching platitudes. Every strength and every
improvement point must trace back to an actual line in the transcript.
Feeds `resources/coaching-log.md` (private, never customer-facing —
CLAUDE.md guardrail 7) so patterns compound over time instead of
resetting every call.

## Steps

1. **Get the material — Gong first if it's connected.**
   - **If a Gong MCP connector is available in this session**, use it
     to pull the call transcript directly: find the call by
     account/date, retrieve its transcript, and confirm with the operator
     that it's the right call before coaching. Do NOT assume specific
     Gong tool names — discover what's actually exposed this session
     (they vary by connector version) and use those. If the Gong
     lookup returns nothing or is ambiguous, fall back to asking the operator
     to paste, rather than guessing which call they meant.
   - **If Gong is NOT connected** (the fallback, and how this ran
     historically): The operator pastes a Gong transcript or their own notes and
     names the account/call. If they only name the call with no content
     and Gong isn't available, ask them to paste — never coach from a
     guess about what was probably said.
   - Either way, the rule downstream is identical: every point quotes a
     real line. Pulling from Gong doesn't lower the evidence bar, it
     just removes the copy-paste step.

2. **Get the call's goal.** Check `accounts/<customer>/brief.md` (from
   `process-customer`) for what this call was supposed to accomplish.
   If there's no brief or no stated goal, ask the operator what the goal was
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
   Goal: <call goal from brief, or "not stated — asked the operator">
   Did well:
   - ...
   To improve:
   - ...
   Focus for next call: <the one thing>
   ```

7. **The chat reply mirrors that same bullet structure — always.**
   Two clearly labeled bullet lists ("Things I did well" / "Things I
   did wrong" or similar), not prose paragraphs, then the one focus.
   (Learned 2026-07-20, first real run: it went out as prose
   paragraphs in chat even though the log entry itself was already
   bulleted — the operator asked for bullets explicitly.)

8. **Never surface this outside chat + coaching-log.md.** Not in
   account briefs, not in Notion, not in anything the operator might later
   paste into a customer-facing doc.

## Guardrails

- Only ever coach from a real transcript — whether pulled from Gong (if
  connected) or pasted by the operator. If Gong isn't connected and nothing is
  pasted, ask; never claim to have "pulled the transcript" when you
  haven't, and never coach from an imagined version of the call.
- `resources/coaching-log.md` is private (CLAUDE.md guardrail 7) —
  never quote it in account briefs, RFPs, or anything customer-facing.
- If the call's goal isn't documented anywhere, say so and ask, rather
  than inventing a plausible-sounding one.
- Directness over comfort — vague encouragement isn't coaching.
