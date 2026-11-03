# Companion enrichment project — stand-up brief

This is a separate AI project for companion enrichment. It is not the accession desk and it is not the Astro repository.

| Project | Job | Writes |
|---|---|---|
| Coin ID and catalog (this book) | Identify, flip, accession | `CC-Accession-Register.xlsx` |
| Companion enrichment (this brief) | Phone-length writeups | `enrichment/CC-00xx.json` only |
| Astro website | Merge and publish | `dist/` |

Authority for facts is `export/catalog.json` from the book. If enrichment and the book disagree on mint, catalog, weight, or series, the book wins. Re-run enrichment. Do not edit the xlsx from this project.

Add `CATALOG-EXPORT-SPEC.md` to the project as the schema authority. Install the `companion-catalog-enrichment` skill and the same six coin skills the desk uses from the sibling `skills/` folder.

## Skills to attach

Required:

- `companion-catalog-enrichment` — workflow, schema, confirm gate (this packet)
- `ancient-coin-research-helper-general` — series unclear
- `ancient-coin-research-helper-roman`
- `ancient-coin-research-helper-greek`
- `ancient-coin-research-helper-byzantine`
- `medieval-coin-research-helper`
- `ancient-coin-research-helper-misc`

Route by the `series` field on the catalog coin. Do not re-identify unless the photos contradict the book. Then stop and send the coin back to the desk.

## What the user drops on this desk

1. The catalog object for one `id` (or the whole `catalog.json` plus the id).
2. `CC-00xx-obv.jpg` and `CC-00xx-rev.jpg`.
3. Optional note: more mint, less biography, reading-level, stale rewrite.

Paste prompt (new chat):

```
Enrich CC-00xx for the companion site.

Catalog facts (do not change these):
[paste the catalog.json object]

Photos attached: CC-00xx-obv.jpg / CC-00xx-rev.jpg.

Load the skill that matches series.
Draft enrichment/CC-00xx.json.
Do not write the file until I say to proceed with enrichment.
```

After the draft is right:

```
Proceed with enrichment of CC-00xx.
Write enrichment/CC-00xx.json with status Ready.
```

## Workflow

1. Announce the id. One open enrichment at a time unless the user splits work.
2. Read the catalog object. Treat every fact field as given.
3. Load `companion-catalog-enrichment`, then the series skill.
4. Describe what the photos actually show.
5. Draft all required blocks (facts pass), rewrite prose for a visitor (voice pass), then show the JSON in a fenced block.
6. Confidence High / Medium / Low / ? for the writeup, not for the attribution.
7. Ask: `Proceed with enrichment of CC-00xx?`
8. User may ask for more or less. Revise the draft.
9. On yes, write `enrichment/CC-00xx.json` with `"status": "Ready"` and `"stale": false`.
10. Do not write Draft files unless the user says to park it.

## Sections to write

Phone length. Default is one or two dense paragraphs for a life block, less for mint, struck, and a house-only authority. User may ask for more or less. After the facts pass, rewrite prose for a visitor: spoken English, one new idea at a time, no desk language in the file. State connections plainly. Do not use hedging disclaimers (“the coin does not state the link,” “though that is a reading of the type rather than a meaning the evidence states”). Do not change locked catalog fields or fill gaps to sound finished.

Historical content is required whenever the named subject or issuing authority is historically identifiable. Keep uncertain claims qualified.

Catalog `name` is a person: write the life in `portrait.overview` — role and accession, major political or military events, death or later fate. CC-0004 and CC-0008 are the pattern for Byzantine emperors; Roman named portraits use the same seat. If the face is a monogram and not a bust, say that in the first sentence, then write the life in `portrait.overview` anyway.

Catalog `name` is Anonymous and the face is Christ: `portrait.who` is `Christ, Class X`. `portrait.overview` is the image and the class. Put the associated emperor’s life in `authority.overview`. CC-0002, CC-0003, and CC-0007 are the pattern.

Put issuer, co-ruler, dynasty, or issue context in `authority.overview` without duplicating the portrait biography.

| Block | Job |
|---|---|
| `obverse.description` | This flan. Wear, what is left of the bust and legend. |
| `obverse.legend` | Catalog `obv` unchanged (the book string). Not a photo reading. |
| `obverse.legend_expanded` | Expansion of that book string, when it is a legend. |
| `obverse.legend_translated` | Plain-English translation of that book string, when supported. |
| `obverse.legend_note` | Monogram, missing circular legend, or a reading note. Empty string if unused. |
| `reverse.description` | This flan. Type and mintmark. |
| `reverse.type_note` | Type name or mark note. Empty string if unused. |
| `mint` | Where the mint is and what it was doing in this issue. |
| `authority` | Who issued the coin. |
| `portrait` | Who is on the obverse, or the type if anonymous. |
| `struck` | The year or range and why it matters. |
| `also` | Optional extra cards. Empty array if nothing. |
| `sources` | Spec stack: catalog corpus, matching OCRE/RPC line, primary only if used, Wikipedia page title for the life. No URLs. No dealers. |
| `christian_history` | One or two dense paragraphs when a Christian-historical link exists. `relevance`: `direct` / `period` / `background`. Own `sources` array. |
| `economics` | One or two dense paragraphs on the monetary setting. `evidence`: `priced` / `period` / `thin`. Own `sources` array. |
| `culture` | One or two dense paragraphs on type, symbol, or setting. Own `sources` array. |

Gifted or Sold: you may mention the coin left the cabinet in one clause. Do not name the recipient, dealer, or price. Those are not in the catalog object and must not be invented from memory.

## Invention rule

Same as the desk. Do not invent catalog numbers, weights, axes, provenances, or prices. Do not copy Register notes or market comps. Historical overview is standard narrative and must flag uncertainty.

## File written on confirm

Path: `enrichment/CC-0001.json`

Schema: `cc-enrichment/v1` in `CATALOG-EXPORT-SPEC.md` as amended F1–F6. Do not add keys the Astro merge does not know. Do not omit `spec`, `id`, `status`, `stale`, `series`, or the six section objects. Status on this desk is `Draft` / `Ready` / `Stale` only; never write `complete`.

## Stale rewrite

If the user says the book changed (new RIC, new mint, new series):

1. Read the new catalog object.
2. Draft a replacement file.
3. Confirm again.
4. Overwrite `enrichment/CC-00xx.json`.

Do not keep a second file for the same id.

## What this project must not do

- Open or write `CC-Accession-Register.xlsx`
- Assign or change a CC number
- Change `series`, catalog, mint, or flip facts
- Export `catalog.json`
- Build or deploy the Astro site
- Put dealer, price, recipient, or desk notes into enrichment

## Project instructions one-liner for the new AI project

> This project writes companion-site enrichment files. The accession book is a different project. Facts come from catalog.json. Load companion-catalog-enrichment and the series skill. Draft, then ask to proceed. Write enrichment/CC-00xx.json only after confirm.
