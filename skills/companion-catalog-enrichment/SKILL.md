---
name: companion-catalog-enrichment
description: Write phone-length companion-site enrichment JSON for an already-accessioned CC coin. Use when the user says enrich, writeup, companion page, historical overview, or proceed with enrichment. Does not identify coins or write the accession register.
---

# Companion catalog enrichment

Write `enrichment/CC-00xx.json` for the static companion site. Facts come from `catalog.json`. Photos are `CC-00xx-obv.jpg` and `CC-00xx-rev.jpg`.

Schema authority is `CATALOG-EXPORT-SPEC.md`, spec `cc-enrichment/v1` as amended F1–F6. This skill is workflow. After you know `series`, also load the matching research skill:

| series | Skill |
|---|---|
| roman | ancient-coin-research-helper-roman |
| greek | ancient-coin-research-helper-greek |
| byzantine | ancient-coin-research-helper-byzantine |
| medieval | medieval-coin-research-helper |
| judaean | ancient-coin-research-helper-misc |
| misc | ancient-coin-research-helper-misc |
| modern | ancient-coin-research-helper-general |

If series is missing or wrong for the photos, stop. Send the coin back to the accession desk. Do not silently reattribute.

## Do not

- Open or write `CC-Accession-Register.xlsx`
- Assign or change CC numbers
- Change catalog facts (name, title, date, mint, mark, legends, module, denomination, catalog_ref, series, status)
- Invent catalog numbers, weights, axes, provenances, prices, or citations
- Copy desk notes, comps, dealer names, recipients, or sale prices into the file
- Write the JSON file before the user confirms
- Write `"status": "complete"`. File status is `Draft` / `Ready` / `Stale` only. `complete` is applied only downstream after review.
- Use hedging phrases that undercut the writeup, including “the coin does not state the link”, “though that is a reading of the type rather than a meaning the evidence states”, or similar disclaimers. State the connection plainly.
- Enrich CC-0018 while it is parked as the schema-test piece, unless the user lifts that hold

## Desk flow

1. Announce the id. One coin at a time unless the user splits work.
2. Restate the catalog facts in one short block so errors are visible.
3. Describe the two photographs as they are. Worn legend is worn.
4. Draft all required blocks (facts pass: specialist-auditable).
5. Rewrite only the prose fields for a visitor (voice pass), then show the JSON in a fenced `json` block.
6. Give writeup confidence High / Medium / Low / ?.
7. Ask `Proceed with enrichment of CC-00xx?`
8. On revise requests (more mint, less biography, simpler English), change only the prose. Facts stay.
9. On yes, write `enrichment/CC-00xx.json` with `"status": "Ready"` and `"stale": false`.
10. Do not tell the user the file is written until that path exists.

Park a `"status": "Draft"` file only if the user says to save a draft.

## Tone

- Phone screen. One or two dense paragraphs for a life block. Shorter for mint, struck, and a house-only authority.
- Standard narrative. State the connection plainly. Do not undercut a block with hedging disclaimers.
- Describe this flan when photos exist, not an ideal catalog plate. Missing photos: generic wear in the file; missing-file note only in the chat photo paragraph.
- After the facts are stable, rewrite `description`, `overview`, `also[]`, and the topical `paragraphs` into spoken English. Keep the term, then the plain phrase once. Do not repeat the same fact in every section.
- No desk language in the file: “the book,” “this draft,” “project folder,” “cabinet photo missing.”
- No hedging disclaimers in the file: “the coin does not state the link,” “though that is a reading of the type rather than a meaning the evidence states,” or close variants.
- Voice pass does not add catalog numbers, ornaments, dates, or certainty.
- Gifted or Sold may be one clause. No recipient, dealer, or price.

## Legend

`obverse.legend` is catalog `obv`, copied unchanged. That is the book string. Do not replace it with a photo reading.

If the flan is worn, say so in `obverse.description` or `legend_note`. If the book stored a type phrase (`Monogram 8`, `IC-XC Christ enthroned`), still copy that string. Expand and translate only when the string is a real legend. Otherwise leave those fields `""` and use `legend_note`.

## Biography seat

Historical content is required when the person or issuer can be named.

- Catalog `name` is a person: life goes in `portrait.overview` — role and accession, major events, death or fate. Byzantine emperors use the same seat as Roman ones. If the face is a monogram and not a bust, say that in the first sentence, then write the life in `portrait.overview`.
- Catalog `name` is Anonymous and the face is Christ: `portrait.who` is `Christ, Class X`. `portrait.overview` is the image and the class. The associated emperor’s life goes in `authority.overview`.

`authority.name` is the issuing house, not a copy of `portrait.who` when they differ. Do not put the same life in both blocks.

## Mint geography

`mint.name` is the catalog mint, unchanged.

`mint.overview` must place the mint in both the ancient world and the present. Default pattern, one or two sentences:

1. Ancient name and what the town was (province, coast, road, or role in this issue).
2. Modern city and country, in the same clause. Use the current official or widely used English form (Heraclea Thraciae → Marmara Ereğlisi, Turkey; Constantinople → Istanbul, Turkey; Siscia → Sisak, Croatia; Bostra → Bosra, Syria).

Rules:

- One modern place name. No lat/long, no tourist copy, no invented “near X.”
- If the catalog mint is `?` or the site is disputed, say so and skip a confident modern pin.
- Rome, London, and other names that did not move still get the country if a phone reader would need it.
- Do not put the modern name in `mint.name`, `also`, or `sources`.
- Do not treat a modern country as the issuing authority.

## File schema

`spec` is always `cc-enrichment/v1`. Filename matches `id`. Include every required key. Empty optional strings are `""`. Unused `also` is `[]`. Unused topical blocks may be omitted or given empty arrays.

```json
{
  "spec": "cc-enrichment/v1",
  "id": "CC-0001",
  "status": "Ready",
  "stale": false,
  "confidence": "High",
  "series": "roman",
  "obverse": {
    "description": "",
    "legend": "",
    "legend_expanded": "",
    "legend_translated": "",
    "legend_note": ""
  },
  "reverse": {
    "description": "",
    "type_note": ""
  },
  "mint": {
    "name": "",
    "overview": ""
  },
  "authority": {
    "name": "",
    "overview": ""
  },
  "portrait": {
    "who": "",
    "overview": ""
  },
  "struck": {
    "when": "",
    "overview": ""
  },
  "also": [],
  "sources": [],
  "christian_history": {
    "relevance": "period",
    "paragraphs": [],
    "sources": []
  },
  "economics": {
    "evidence": "thin",
    "paragraphs": [],
    "sources": []
  },
  "culture": {
    "paragraphs": [],
    "sources": []
  }
}
```

Copy `mint.name` from the catalog mint. Copy `struck.when` from `date_struck`. Copy `series` from the catalog object. Copy `obverse.legend` from catalog `obv`. Copy `portrait.who` from name and title, or `Christ, Class X` when the catalog name is Anonymous.

`christian_history.relevance`: `direct` (named in a primary Christian text), `period` (same generation or closely linked), `background` (broader context).  
`economics.evidence`: `priced` (price, wage, tax, or reform tied to this piece), `period` (same monetary regime), `thin` (setting only).

## Sources

Core `sources` array, works actually opened, in this order when they apply:

1. Catalog corpus for the series (RIC volume, Sear, RPC, DOC, MIBE).
2. Matching type line in OCRE, RPC Online, or the equivalent live corpus. Same number as the catalog. Not a neighbor type.
3. A named primary source only when a sentence in the core sections depends on it.
4. Wikipedia page title for the person whose life is sketched (`Wikipedia, "Crispus"`).

Topical blocks carry their own `sources` arrays. List only works actually opened for that block.

No padded bibliography. No URLs. No auction houses, dealer lists, or market comps. Research skills may look at CoinArchives; this file must not.

## Stale

If the user brings a new catalog object for an id that already has a file, draft a full replacement and confirm again. Overwrite the same path. Set `"stale": false` on the new Ready file.

## Output in chat before confirm

1. Photo reading (short)
2. Draft sections as headings, including the legend triad
3. Fenced JSON
4. Confidence
5. The proceed question
