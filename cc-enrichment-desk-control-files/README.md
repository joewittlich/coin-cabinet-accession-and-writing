# Coin Cabinet Companion Enrichment Desk

Use this desk to write concise, visitor-facing enrichment JSON for coins that have already been accessioned. The supplied catalog object is authoritative for coin facts; the desk never opens the accession register or assigns or changes coin IDs.

## Layout

```
AGENTS.md                      project instructions
README.md                      project setup and file map
docs/
  CATALOG-EXPORT-SPEC.md       catalog and enrichment schema authority
  ENRICHMENT-PROJECT-BRIEF.md  workflow and paste prompts
  historical-sources-for-enrichment.md
  research-guide.md
research/
  research-sources.json        optional source directory
```

The skill folders are in the repository-level `skills/` directory. Add `companion-catalog-enrichment` and the coin-research skill matching the catalog's `series` to the AI project.

## Set up the AI project

1. Create a private AI project for the Companion Enrichment Desk.
2. Add `AGENTS.md` as the project instructions. Add `docs/ENRICHMENT-PROJECT-BRIEF.md` and `docs/CATALOG-EXPORT-SPEC.md` as reference files.
3. Install or attach the `companion-catalog-enrichment` skill and the six coin-research skills from the repository-level `skills/` directory.
4. Enable web search or browsing for source verification. Add `docs/research-guide.md` and `research/research-sources.json` as optional research aids.
5. Confirm that the AI platform can create the requested enrichment file, or plan to save its confirmed JSON output yourself.

## Enrich a coin

1. Start a new chat. Provide one catalog object (or `data/cc-catalog.json` and the target ID) and the matching obverse and reverse photos.
2. The assistant follows `AGENTS.md`, the enrichment brief, and the specialist skill matching `series`. It drafts the JSON and asks “Proceed with enrichment of CC-00xx?”
3. Review the JSON and confirm before any write. On confirmation, save only `enrichment/CC-00xx.json` with `status: "Ready"` and `stale: false`.

The sample `data/cc-catalog.json` currently contains no coin records; use the live exported catalog object for actual enrichment. If the photos contradict the supplied catalog, stop and refer the coin to the Accession Desk rather than changing its attribution. Never provide this project with the accession workbook.

This desk writes `Draft`, `Ready`, or `Stale` only; `complete` is reserved for downstream review. Do not enrich CC-0018 unless its schema-test hold has been lifted. Do not include dealer names, prices, recipients, or private desk notes in enrichment.

## Enrichment schema notes

`cc-enrichment/v1` includes optional topical blocks:

- `christian_history` (relevance: direct / period / background)
- `economics` (evidence: priced / period / thin)
- `culture`

Each topical block carries its own `paragraphs` and `sources` arrays. The core `sources` array remains the flat catalog stack. See `docs/CATALOG-EXPORT-SPEC.md` for the required shape and field rules.
