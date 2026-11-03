# Optional Grok upstream research integration

This guide is an optional add-on, not part of the standard enrichment setup. It requires a separate research repository containing `grok-upstream-sources.json`, the `scripts/` commands, and the canonical source registry and facts ledger mentioned below. Those files are not included in this control-files package. Skip this guide unless that separate repository is available.

Use [grok-upstream-sources.json](./grok-upstream-sources.json) as a compact routing index when collecting evidence for the Coin Cabinet enrichment workflow. It groups existing research sources by task and period and defines the format for handing new evidence back to the project.

## Run order

1. Run `node scripts/research-brief.mjs CC-xxxx` and use the reported period, current text, existing facts, and source leads. Add `--period` only when broader period facts are needed.
2. Select sources from the package's relevant lane and period route. The source IDs point to the canonical `research/sources.json`; check that entry for current access notes and status before fetching.
3. Read the source itself. Record one fact per line in `research/facts/<period>.jsonl`, including a precise locator, direct URL, tier, verification date, coin ID, and a quotation of no more than 25 words.
4. If a source is new or its registry entry needs updating, write a JSON array to `research/sources-updates/grok-<period>.json`, then run `node scripts/research-registry.mjs --merge`.
5. Run `node scripts/research-registry.mjs --check` after changing registry or fact files. Follow the active research playbook and block-specific guide before writing or promoting enrichment.

## Evidence discipline

- Treat this package as a source selector, not evidence. A link, source title, or `verified` status does not establish a claim until the relevant page or passage has been read.
- Prefer the coin's named standard catalogue reference, primary texts, specialist coin catalogues, or scholarly tables. Use secondary and fallback material only with the project confidence tier and wording.
- Respect source-specific access failures and OCR limitations recorded in `research/sources.json`. Do not invent a passage locator from a search result or landing page.
- For monetary claims, record the source's date, place, and unit. Purchasing-power comparisons must stay relative; never convert to modern currency.
- Exclude dealer prices, auction copy, acquisition source, and provenance from research and enrichment.
- After two failed attempts on a claim, log the dead end; then qualify it at tier C with a recorded basis or omit it.

The package intentionally does not duplicate the 700+ entry source registry or the facts ledger. Keep those canonical files authoritative; this compact file is a Grok-friendly routing and handoff layer.
