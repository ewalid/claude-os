# DAM / asset management

Source: validated DAM requirements grid, 2026-07 (48 requirements).
Trust: `validated`.

Storyblok is positioned as a **CMS with a capable built-in Asset Manager**,
not as a full standalone DAM. That framing was accepted in a real validated
submission — lead with the native strengths, name the two genuine partials
honestly, and route the rest through automation.

## Native strengths (answer "Yes")
- **Asset Manager embedded in the Visual Editor** — this is the strongest
  card: no separate DAM to context-switch into, no round-trip. Answer
  "native integration" for any CMS↔DAM integration requirement.
- **Image Service** — on-the-fly resize/crop and format conversion to
  **WebP and AVIF**, per-platform and per-device dimensions generated from
  URL parameters. Covers "automated sizing by platform/device" and
  "resize/convert without quality loss".
- **CDN delivery** — all assets via AWS CloudFront natively, no extra
  config. Also makes assets referenceable from any external system
  (CRM, e-shop, ad network, brand portals) by CDN URL.
- **Asset folder permissions at root level** — restrict users/roles to
  specific folders, which is how external-partner/agency folder sharing
  gets answered. (Note the plan gate: fine-grained asset-folder scoping is
  top-tier — see `pricing-licensing.md`.)
- **Reference tracking** — shows where an asset is used across all content;
  answers asset governance, reuse and archiving requirements.
- **Search + filter by file type** in the Asset Manager.
- **Custom metadata fields** — text, required flags, regex validation, and
  the **Translatable flag natively**, which covers "automated translation
  of asset descriptions/metadata".
- **AI Alt Text** — automatic asset descriptions.
- **Format support** — all common media formats (jpg, png, gif, WebP, AVIF,
  mp3, mp4, pdf, doc/docx, xls/xlsx…).

## The two honest partials — state them, name the bridge
1. **Embedded file metadata (IPTC / Exif / XMP) is NOT natively extracted
   or preserved** on upload. Custom metadata fields exist, but they don't
   read what's baked into the file. Bridge: a FlowMotion workflow with a
   metadata-extraction step — real, but additional implementation.
2. **No native video transcript or subtitle generation.** Bridge: a
   speech-to-text API integrated via FlowMotion; the resulting
   transcript/subtitle files are then stored as assets and served like any
   other. Video assets themselves are stored and CDN-delivered fine;
   video *SEO* optimisation is a frontend concern.

## Automation / workflow asks → FlowMotion
Approval workflows on assets, AI-driven auto-tagging on upload, download
approval flows for external partners, and replacing email-based access
requests all route through **FlowMotion** (see `integrations.md`) rather
than being native Asset Manager features. This is a legitimate answer, not
a dodge — but say which product does the work.

## Positioning line that worked
A unified CMS + Asset Manager **reduces the expertise burden** compared
with running a separate DAM product alongside a CMS — useful when a
prospect worries about needing dedicated FTE headcount to operate two
systems.
