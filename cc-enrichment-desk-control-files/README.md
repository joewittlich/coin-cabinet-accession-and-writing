# Coin Cabinet Companion Enrichment Desk

Generic control files for a companion-site enrichment desk.

These files define the schema and workflow for writing phone-length enrichment JSON for an already-accessioned coin catalog. Facts come from `catalog.json` plus the two cabinet photos. The desk never opens the accession register and never assigns or changes catalog numbers.

## Layout

```
AGENTS.md                      project instructions
CATALOG-EXPORT-SPEC.md         schema authority (cc-catalog-export/v1, cc-enrichment/v1)
ENRICHMENT-PROJECT-BRIEF.md    stand-up brief and paste prompts
historical-sources-for-enrichment.md
research-guide.md
research-sources.json
```

The skill folders are stored in the sibling `../skills/` directory, not in this control-files folder. Add the companion enrichment skill and the applicable coin-research skill from there to the AI project.

`research-guide.md` is a standalone, optional workflow for evidence-backed enrichment. It uses ordinary web browsing and the project files; no local scripts, research repository, or facts ledger are required. `research-sources.json` is an optional directory of source leads to consult alongside it.

## Status

This desk writes `Draft`, `Ready`, or `Stale` only. A downstream review may later set `complete`.

## Topical blocks

`cc-enrichment/v1` (amended F4–F6) includes optional blocks:

- `christian_history` (relevance: direct / period / background)
- `economics` (evidence: priced / period / thin)
- `culture`

Each carries its own `paragraphs` and `sources` arrays. Core `sources` stays the flat catalog stack.
