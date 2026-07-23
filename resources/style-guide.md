# style-guide.md

Reference for anything customer-facing Darwin produces (decks, demo
scripts, RFP answers). Fill in as real examples accumulate — this is a
living document, not a one-time spec.

## Tone
- English primary; some accounts (FR-based) may need
  French — confirm with the operator before switching, never assume.
- Direct, technical-credibility-first. The operator is the technical expert in
  the room; copy should read that way — no marketing fluff.

## Slide conventions

- **File format: .pptx, not .odp.** Decks are authored in Google Slides;
  export as .pptx before dropping into `resources/deck-examples/`. Google
  Slides round-trips to .pptx natively with minimal formatting loss, and
  .pptx has far better tooling support for `build-deck` to actually parse
  and adapt than .odp does. Decided 2026-07-15.

### The template (learned from 5 real decks, 2026-07-15)

The operator dropped 5 real demo decks into `resources/deck-examples/` (real
customer decks — filenames are local-only, gitignored; not named here).
All 5 share one underlying template — `build-deck` adapts this template,
it never invents slide types that aren't in it.

The examples below come from a headless-CMS product's decks (the
operator's product — read it from `memory.md`). The **structure** is
the reusable part; product-specific slide content (feature names, the
website/case-study domain, pricing slide names) is drawn from the
operator's own example decks, not hardcoded here.

**1. Title** — "\<Product\> for \<Account\>" (the product name comes
from `memory.md`), with the AE's name/title and
the SE's name/title (The operator, "Solutions Engineer") on the slide, each
paired with a small headshot photo (confirmed present in all 5 example
decks — 2 PICTURE shapes on slide 1, one per person, easy to miss since
they don't show up in a text-only scan of the slide). Headshots live in
`resources/AEs & SEs/<Full Name>.jpeg` — swap in whichever exists for
the real AE/SE, matched by name (never guess/reuse a different
person's photo from the source deck). If a headshot is missing for
someone, flag it rather than leaving the source deck's wrong person in
place. Discovered 2026-07-20 (first real FR build): it left the
source deck's original AE/SE photos in place unnoticed.

**2. "What we know so far."** — the discovery recap, one row per line:
Key Business Result → Observed constraints → What is needed? → What
does success look like? → By when? This is pulled straight from
discovery notes/the account brief's MEDDPICC + priorities sections —
never invented. If a row can't be filled from real discovery, leave it
flagged "need validation," don't guess a plausible-sounding one.

**3. Demo stations overview** — a themed slide naming 2-4 "demo
stations," each tailored to what this account cares about. Real
examples of themes used: "Commerce & Integrations," "AI Capabilities,"
"Developer Experience & Integration," "Structured Content & Reuse,"
"Platform Architecture & Security," "System Architecture & DX,"
"Editorial User Experience," "Localisation and Internationalisation."
Pick themes based on the account's actual priorities/stack (from the
brief), not a default set.

**4. Per demo station** (repeated once per station named in step 3),
each a 3-slide block:
   - a transition slide: station name, a one-line value statement, a
     bullet list of what will be shown, the product's website in the footer
   - a **blank "Demo" slide** — literally just "Demo" + "CONFIDENTIAL",
     no content. This is the live-demo placeholder; build-deck never
     fills this in with fake demo content, it stays blank for the operator to
     drive live.
   - a "KEY TAKEAWAYS" slide: what we demonstrated (bullets) + key
     benefits (bullets) + one customer proof point/quote with a
     the product's public case-study URL as a source link (pick a public
     case study relevant to the account's vertical)

**5. Technical Topics** (optional — used for more technical/enterprise
accounts; skipped for smaller/less technical ones). Standing slides
reused near-verbatim across decks — the specific set is product-
specific and comes from the operator's example decks. For a
headless-CMS product they look like: a workflow/orchestration slide, a
CMS-MCP / AI-agent-access slide, a composable/headless reference
architecture, a performance & scalability slide, and a security slide
(ISO 27001/SOC 2, encryption, GDPR).

**6. Commercial section** (optional — larger/enterprise deals only,
seen in the enterprise example decks, not the smaller accounts):
   <Product> Pricing → Partnership Orchestration → Implementation
   Methodology → Regressive Multiplier → Premium vs Elite breakdown →
   package option slides ("Premium Smart Package," "Elite Smart
   Package").

**7. Closing**: "Start Your Story" / "Build Your Story" (only on decks
with the commercial section) → "Thank You for Joining!" — always the
last slide.

**Placeholder instructional text**: some source decks still carry
literal template instructions on unfilled slides (e.g. "List here your
talk track for the demo station... 1. The 'Tell' (Opening)... 2. The
'Show'..."). If `build-deck` ever encounters this leftover scaffolding
text in a source deck, it's a sign that slide was never actually
customized for that account — treat it as empty, not as real content to
copy forward.

## FR/EN rules
- Default to the customer's own language in decks; internal
  notes/briefs stay in English regardless.

## Status
Template structure confirmed against 5 real decks, 2026-07-15. Tone/
FR-EN rules still need the operator's real-world validation on an actual FR
deck (the first FR account is the test case).
