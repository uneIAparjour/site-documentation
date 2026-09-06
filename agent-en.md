# Agent.md — uneIAparjour.fr (English version)

This file documents the **English side** of uneIAparjour.fr specifically — how it differs from the French site, what's translated, what isn't, and how the translation pipelines work. For the full site documentation (editorial principles, French architecture, etc.), see `agent.md` in this same repo; facts that are identical in both languages aren't repeated here.

## Project identity

**Name**: Une IA par jour (kept untranslated everywhere — it's a brand name)
**French site**: https://www.uneiaparjour.fr
**English site**: https://www.uneiaparjour.fr/en/
**Author**: Bertrand Formet
**Contact**: contact@uneiaparjour.fr · @bertrandformet on social media
**License**: Creative Commons CC BY 4.0 (articles, categorization, selection, database)
**English site launched**: September 2026, via an automated FR→EN translation pipeline

## Why an English version exists

The site is French-first: one new generative-AI tool a day, tested and documented in French, since February 2023. The English version is a **translation layer on top of that same content**, not a separate editorial line — every EN tool post has a French original, and the newsletter itself (Substack) has no English edition. The goal is to make the daily tool reviews and a handful of static pages available to an English-reading audience, at as close to zero manual effort per day as possible.

---

## What's translated, and how

### Daily tool posts (the core content)
- **Mechanism**: fully automated. GitHub Actions workflow `translate.yml` (repo `uneiaparjour-en-translation`) runs nightly at 06:00 UTC, translates any new/changed FR tool post via Azure Translator, creates or updates the matching EN post via the WordPress REST API, links it to its FR original through Polylang (via a small companion plugin, "translation-bridge"), and publishes it automatically.
- **Scope**: only posts present in the `base` tools dataset (see `agent.md`) — this deliberately excludes newsletter, Focus, and Lectures-partagées-type content, which have their own separate handling (below).
- **Progress**: **1,280 tool posts translated** (as of 06/09/2026), all successful — the pipeline processes each day's new post plus a shrinking backlog of older ones, up to 200/run.
- **State file**: `script/state/translations.json` in the `uneiaparjour-en-translation` repo — the single source of truth for which FR post maps to which EN post. Never edit by hand.
- **Human-edit lock**: if a published EN post is manually edited in WordPress and its `_translation_locked` meta flag is set, the pipeline permanently skips re-translating it on future FR source changes.
- **Known limitation**: EN post slugs often end in `-2` (e.g. `/en/wisdomplan-2/`) when the translated title happens to match the FR slug — WordPress's own collision handling, not something the pipeline works around (tried and confirmed not fixable, see `uneiaparjour-en-translation`'s own incident log).

### Focus articles (weekly editorial pieces)
- **Separate pipeline**: `translate-focus.js` / `publish-focus.js` (same repo), running nightly at 07:00 UTC — deliberately split from the main pipeline because Focus posts (French category "Focus", id 50) are personal essays tied to each newsletter issue, not tool reviews, and must never enter the `base`/`base-en` tools dataset.
- **Own state file**: `script/state/focus-translations.json` — never shared with the main pipeline's state.
- **EN category**: "Focus" (slug `focus-en`), auto-created and linked to the FR "Focus" category via Polylang the first time the pipeline ran.
- **Excluded from the EN homepage**: same as FR (see the `exclure_focus_accueil` hook in the theme's `functions.php` — it excludes both the FR category id 50 and the EN category id 1763).
- **Backlog cleared**: 44/44 Focus articles translated and published (06/09/2026).

### Static pages (translated once, by hand)
| Page | EN URL | Notes |
|------|--------|-------|
| Home | `/en/` | The blog index — same mechanism as any WordPress home page, just filtered to EN posts by Polylang |
| Newsletter | `/en/newsletter/` | FR: "Lettre". The Substack subscribe embed (`uneiaparjour.substack.com/embed`) needs no translation — it localizes itself to the visitor's own browser language automatically |
| Readings | `/en/readings/` | FR: "Lectures partagées". Interface (repo `export-lectures-partagees`, file `index-en.html`) is translated; the actual 390 reading-list entries live in a one-time hand-translated `data-en.json` (see below) |
| About | `/en/about/` | FR: "À propos". Includes a translated infographic (`uneiaparjour-infographie-en.html`, this repo) |
| Site Database | not yet menu-placed (matches FR, which also doesn't link it from a menu) | Points to `base-en` (GitHub, Hugging Face) instead of `base` |
| Contact | `/en/contact-2/` | Slug collision with the FR original gave it the `-2` suffix, same limitation as tool posts |
| Legal Notice & Privacy | `/en/legal-notice-privacy/` | FR: "Mentions et confidentialité" |

**Not yet translated**: Sélection, Aide au choix — neither exists in English yet, and neither is linked from the EN menus.
**Deliberately never translated**: Écho (the narrative game about delegating to a machine) — an explicit editorial decision, not a backlog item.

### EN menu structure
Unlike the FR site's 8-entry header menu, the EN header menu (`Header Menu #1 (EN placeholder)`) currently has 5 entries, in the same relative order as their FR counterparts: **Home → Newsletter → Readings → Focus → About**. The EN footer menu (`Footer Menu (EN placeholder)`) has 3: **Contact → Sitemap → Legal Notice & Privacy** — same order as the FR footer. When Sélection and Aide au choix eventually get translated, their menu items should be inserted at the same relative position as in the FR menu (positions 2 and 3).

### `data-en.json` (Lectures / Readings dataset)
- A **one-time, hand-translated** copy of `data.json`'s 390 entries (repo `export-lectures-partagees`), covering only the `title` and `desc` fields — `url`/`date`/`letter`/`cat`/`source` are untouched.
- **Not auto-updated.** Unlike everything else on this page, there is no pipeline behind it: the site owner maintains it by hand going forward, the same way the FR `data.json` is already maintained by hand each week.
- Category labels are translated (e.g. `etudes`→Studies & Research, `securite`→Security & Disinformation); the app's own internal branding "Lectures partagées" is rendered as "Shared Reads" throughout the EN interface, including every export format's generated file content.

---

## The `base-en` dataset

Mirrors `base` (see `agent.md`) but in English, generated **only from articles already translated** — it can never get ahead of the translation pipeline. It's built by fetching directly from the site's own `/en/feed/` RSS feed, so it only ever contains English content by construction (no filtering needed on that side); the FR `base` dataset, in the other direction, has its own defensive filter explicitly excluding any `/en/` URL from its sync, so cross-contamination is guarded against both ways.

| Channel | URL | Format |
|---------|-----|--------|
| GitHub | `https://github.com/uneIAparjour/base-en` | ODS + CSV |
| Hugging Face | `https://huggingface.co/datasets/uneIAparjour/base-en` | ODS + CSV |

- **Workflow**: `nightly-update.yml`, 08:00 UTC (2h after the main FR `base` sync, 2h before the EN tool-translation pipeline's own run the following cycle — offset by convention, not a technical requirement).
- **`force_sync`**: a `workflow_dispatch` boolean input that forces a Hugging Face push even when 0 new rows were found — a standing troubleshooting toggle, not meant for routine use.
- Feeds the EN search overlay (see below) — never the FR one.

---

## Bilingual `recherche-outils` (search overlay)

The same overlay code serves both languages. `functions.php` resolves the visitor's Polylang language server-side and passes the right CSV URL to the frontend via `wp_localize_script`:
- FR pages → `uneIAparjour/base`
- EN pages → `uneIAparjour/base-en`

UI strings (category names, placeholders, buttons) are translated client-side via an `I18N` lookup table in `recherche-overlay.js`, keyed off the same server-resolved language.

---

## Rules for an AI agent working on the English site

- **Never hand-translate a tool post.** If an EN tool post is missing or stale, that's a pipeline issue (check `uneiaparjour-en-translation`'s Actions runs and `translations.json`), not something to fix by editing WordPress directly — a manual edit sets `_translation_locked` and permanently opts that post out of future automatic updates.
- **Never add Focus or newsletter content to `base-en`.** Both are explicitly out of scope for that dataset — see the exclusion logic in `translate.js`'s own source comments if in doubt.
- **`data-en.json` is manually maintained.** Don't expect it to reflect new FR `data.json` entries — that sync is intentionally not automated (see above).
- **Menu placement mirrors FR structure**, but only for pages that are actually translated — never add a menu item pointing at a page that doesn't have an EN version yet.
- **Internal links inside translated content**: point at the FR URL for any destination page not yet translated (graceful degradation), and get automatically rewritten to the EN URL once that destination is translated (see `rewriteInternalLinks` / the pipeline's slug map).
- **Category and tag names**: a fixed, hand-verified FR→EN lookup table (`config/category-translations.json` in `uneiaparjour-en-translation`) is used ahead of machine translation — short, context-free category names translate unreliably via Azure otherwise.

---

## Contacts

- **Author**: Bertrand Formet
- **Email**: contact@uneiaparjour.fr
- **Social**: @bertrandformet
- **Content license**: CC BY 4.0
- **Independence**: no commercial partnerships, no sponsorships
