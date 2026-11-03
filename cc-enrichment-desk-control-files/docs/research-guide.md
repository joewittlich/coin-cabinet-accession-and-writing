# Research Guide for the Companion Enrichment Desk

Use this guide to research an already-catalogued coin while drafting its companion-site enrichment. It is designed to work inside the enrichment project with the catalog object, coin photographs, this project's reference files, and ordinary web search or browsing. It does not require access to the accession repository, a local research repository, scripts, a source registry, or a facts ledger.

## What to have in the project

- `../AGENTS.md`, `ENRICHMENT-PROJECT-BRIEF.md`, and `CATALOG-EXPORT-SPEC.md`
- The `companion-catalog-enrichment` skill and the coin-research skill matching the coin's `series`
- `historical-sources-for-enrichment.md` as a general source directory
- `../research/research-sources.json` as an optional set of topic lanes, source leads, and access cautions
- For each task: the coin's catalog object, obverse and reverse photos, and optionally an existing enrichment draft

The source directory and JSON index are starting points, not required software or evidence. If either is unavailable, follow the source priorities below and search and open sources directly.

## Non-negotiable boundaries

- Treat catalog facts as locked. Research may explain them but must not alter or re-identify them. If photos appear to contradict the catalog, stop and refer the coin to the Accession Desk.
- Do not access or modify the accession workbook, assign an ID, or invent missing measurements, provenance, prices, or catalog references.
- Research only what is needed to support the enrichment. Do not turn the task into a general literature review.
- Never treat a search result, snippet, source index entry, or a source's listed status as proof. Open the relevant source page, record, text, or scan and verify the passage.
- Exclude dealer listings, auction descriptions, prices, acquisition details, recipients, and private desk notes.
- Do not create a separate ledger, research JSON, script output, or other file. Keep temporary claim-to-source notes in the chat or working draft; the final enrichment file is the only task output.

## Research workflow

### 1. Lock the scope

Read the supplied catalog object and identify its `id`, `series`, date, mint, denomination, catalog reference, and legend/type descriptions. Load `companion-catalog-enrichment` and the matching coin specialist skill. Use the photos to describe the actual coin, not to reopen the attribution.

Note which enrichment blocks need evidence. Common questions include:

- **Core identification:** Does the named catalog reference support the supplied type, date, mint, denomination, or legend expansion?
- **Mint:** Where was the ancient mint, what role did it play in this issue, and what is its present-day location?
- **Authority and portrait:** Who issued the coin, and what concise life or political context helps a visitor understand the named person?
- **Date struck:** Why does the supplied issue date or range matter?
- **Christian history, economics, or culture:** Is there a specific, well-supported connection worth adding? Omit a topical block when there is no useful evidence.

Do not research a catalog fact merely to replace it. Verify it only as needed to explain the coin or to identify a genuine conflict that requires stopping.

### 2. Choose a source

Start with the coin's named standard catalogue or corpus, then consult the matching online catalogue or specialist reference when available. Choose sources that directly support the particular claim:

1. **Coin type and catalog facts:** the applicable standard catalogue, followed by its matching online type record (for example RIC/OCRE, RPC, DOC, MEC, North, Album, or a relevant specialist corpus).
2. **Historical events and people:** primary texts for claims that depend on testimony; reputable specialist reference works for concise narrative context.
3. **Mint geography and institutions:** archaeological, museum, scholarly, or authoritative geographic references.
4. **Economics and monetary context:** scholarly studies, primary price or wage evidence, and specialist numismatic work. Record date, place, and unit for any figure.
5. **Iconography and culture:** specialist numismatic or art-historical studies, primary texts where relevant, and reputable museum catalogues.

Use `../research/research-sources.json` to find candidate sources by lane or topic. Its source titles, URLs, `use_for` descriptions, and `access_note` fields are discovery aids only. A listed source may be unavailable, may cover only a landing page, or may not support the proposed claim. Verify the exact relevant page or passage yourself. Do not assume its period routes are exhaustive.

Use secondary or general reference sources to orient a question, not as substitutes for a relevant coin corpus or primary evidence. Wikipedia may be used for the person's life when the enrichment schema calls for it, but open the article and do not rely on it alone for disputed or specialist claims.

### 3. Verify claims, not just sources

For each claim you plan to use:

- Open the source itself and identify the exact page, entry, section, book/chapter, or scan page that supports it.
- Cross-check important or disputed attribution and chronology claims against a second appropriate source when possible.
- Treat OCR, search snippets, undated summaries, and generic landing pages cautiously. Verify important readings against a scan or another source.
- If a source cannot be opened, do not cite it as read. Try a more direct route or another source. After two failed attempts at the same source or claim, stop repeating the attempt; qualify the claim from evidence you did obtain, or omit it.
- Keep any temporary research note concise: **claim | source title | exact locator | confidence or limitation**. No quotations are needed in the enrichment JSON.

For monetary claims, keep comparisons historical and relative. Never convert an ancient or medieval amount into modern currency. Do not give a number without the evidence's date, place, and unit, and explain any calculation or uncertainty.

### 4. Draft for the visitor

Write only evidence-supported claims that serve the coin's story. Put the named person's life in `portrait.overview`; avoid repeating that biography under `authority`. Use the separate topical blocks only when relevant, with the evidence labels required by the schema:

- `christian_history.relevance`: `direct`, `period`, or `background`
- `economics.evidence`: `priced`, `period`, or `thin`

Keep prose phone-length, direct, and readable. Preserve uncertainty when the evidence is uncertain. Do not insert research-process notes, URLs, or unsupported caveats into visitor-facing prose.

### 5. Cite and hand off

Follow `CATALOG-EXPORT-SPEC.md` for the exact JSON shape. The core `sources` array is the required catalog/source stack; each topical block has its own `sources` array. List only works actually opened and used, using concise work or database titles and the specific type/entry where useful. Do not put URLs, dealer houses, or sources merely suggested by the index in those arrays.

Show the complete JSON draft in chat and ask for confirmation as required by the enrichment workflow. On confirmation, write only `enrichment/CC-00xx.json` with `status: "Ready"` and `stale: false`. Do not create a research ledger or write before confirmation.
