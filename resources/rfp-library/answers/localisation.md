# Localisation

Source: validated RFP submission, 2026-07-30. Trust: `validated`.

## Model
Native internationalisation with two approaches: **field-level
translation** and **multi-tree / folder-level (Dimensions)** for markets
needing structurally different content rather than translations. Locale-aware
preview in the Visual Editor with an in-editor language switcher;
locale URL structure (`/fr/`, `/de/`) is a standard front-end i18n pattern.
Translatable slugs supported.

## Fallback behaviour — precise answer
The Content Delivery API takes a `fallback_lang` parameter: an untranslated
field returns the fallback locale's value, then the default language if
that is also missing. Fallback is configurable **per API request at the
locale level**.

⚠️ **Per-field fallback granularity** (different fallback rules for
different fields on the same page) is **not** natively supported. Answer
"Supported with configuration" and state the limit honestly — an earlier
draft answered this vaguely ("should be re-confirmed") which is worse than
a precise partial yes.

## Translation workflows — two paths, both real
- **Native AI Translations** — 35+ languages directly in the editor. Fast,
  in-platform, no vendor contract needed. Often overlooked; lead with it.
- **External TMS** — official integrations with Lokalise (bidirectional
  sync), Smartling, Localazy, plus language export/import (XML/JSON)
  compatible with tools like Trados.

Present both. Enterprise localisation questions usually assume a TMS is
mandatory; showing a credible no-vendor path is a differentiator.
