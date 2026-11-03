# Catalog export and enrichment spec

Authority remains `CC-Accession-Register.xlsx`. This file is the contract for a derived mobile companion site. It does not assign CC numbers and it does not store essays in the book.

Locked 2026-09-05. Amended 2026-09-09 (F1–F3). Amended 2026-11-03 (F4–F6). No other open pipeline decisions.

## Locked decisions

| ID | Decision |
|---|---|
| A1 | Export the Register on demand. Not on every accession. |
| A2 | Export master photo filenames only. Resize in the website repo. |
| A3 | Register column `series` is live. Set when the skill is chosen. Allowed: roman / greek / byzantine / medieval / misc / modern. |
| B1 | `catalog.json` + `enrichment/CC-0001.json`. Merge only in the generator. |
| B2 | Ingest lives in the website repo. This project only emits `export/`. |
| C1 | One enrichment project. It loads the matching coin skill. |
| C2 | Same invention rule as the desk. Historical overview is standard narrative. Flag uncertainty. Do not invent catalog numbers, weights, axes, provenances, or prices. |
| C3 | Inlined mint / authority / portrait / year sections. No gazetteer in v1. |
| C4 | Confirm gate on enrichment. User may ask for more or less detail before Ready. |
| D1 | Astro. Static `dist/` to Blob. |
| D2 | Client-side search. No Azure Function. |
| D3 | CSS grid pair for obverse and reverse. |
| D4 | Dark mode default. Mobile first. Search is primary navigation. Hamburger browses `series`. |
| D5 | Ship the coin as a flip viewer if enrichment is missing. Badge the page. |
| E1 | Azure Blob static website + Front Door. |
| E2 | Terraform owns infra only. Upload `dist/` in CI or a script. |
| F1 | Enrichment v1 obverse legend triad: `legend`, `legend_expanded`, `legend_translated`. `legend_note` remains for monograms, missing circular legends, or a reading problem. |
| F2 | Enrichment file `status` remains `Draft` / `Ready` / `Stale`. `complete` is applied only downstream after review; this desk never writes it. |
| F3 | Enrichment v1 section budget and source stack as specified under `enrichment/CC-0001.json`. |
| F4 | Enrichment v1 adds three optional topical blocks: `christian_history`, `economics`, `culture`. Each carries its own `paragraphs` and `sources` arrays. |
| F5 | `christian_history.relevance` is `direct` / `period` / `background`. `economics.evidence` is `priced` / `period` / `thin`. |
| F6 | Core `sources` stays the flat catalog stack. Topical sources live only in their block. |
| Roadmap | v1 flip viewer + search. v2 enrichment sections. v3 PWA / offline. |

`Gifted` and `Sold` stay in `catalog.json`. The companion may badge status. It still withholds dealer names, prices, source, comps, and desk notes.

## What this project emits

On demand, after the user asks to export:

```
export/
  catalog.json
  photos/
    CC-0001-obv.jpg
    CC-0001-rev.jpg
    …
  manifest.json
```

Copy photo files from the names in `photo_obv` / `photo_rev`. Do not resize, rename, or background-remove here. If a file is missing, list it in `manifest.json` and still emit the coin.

Do not put `export/` into the accession book. Do not write enrichment in this project.

## Register column `series` (A3)

Add column `series` to the Register. Allowed values:

| Value | Desk skill | Hamburger label |
|---|---|---|
| `roman` | ancient-coin-research-helper-roman | Roman |
| `greek` | ancient-coin-research-helper-greek | Greek |
| `byzantine` | ancient-coin-research-helper-byzantine | Byzantine |
| `medieval` | medieval-coin-research-helper | Medieval |
| `misc` | ancient-coin-research-helper-misc | Other |
| `modern` | closest skill; cabinet includes modern | Modern |

Set `series` when the coin is identified, before accession. Reattribution may change `series` in place. Seed `CC-0000` is `misc` and is never exported.

Column P on the Register. Backfill 2026-09-05:

| ID | series |
|---|---|
| CC-0000 | misc |
| CC-0001 | roman |
| CC-0002 | byzantine |
| CC-0003 | byzantine |
| CC-0004 | byzantine |
| CC-0005 | roman |
| CC-0006 | roman |

Exports read the cell. Do not derive `series` from `catalog_ref` once the column is filled. `series_derived` stays in the JSON schema as `false` for book-sourced values.

## Export filter

Include a row when:

- `accession_id` is present
- `status` is `Accessioned`, `Gifted`, or `Sold`

Exclude:

- `Seed` (CC-0000)
- `Reserved` tickets still on the desk
- blank rows

Pass `status` through. The companion may badge Gifted or Sold. It must not show who received a gift, who bought it, the dealer, or the price. Those stay in `source`, `notes`, and `market_comps`.

## `catalog.json`

Facts from the book. No essays. No dealer, price, comps, desk notes, chat title, or confidence.

```json
{
  "spec": "cc-catalog-export/v1",
  "generated": "2026-09-05T20:00:00-05:00",
  "source": "CC-Accession-Register.xlsx",
  "coins": [
    {
      "id": "CC-0001",
      "num": "0001",
      "accession_n": 1,
      "status": "Accessioned",
      "series": "roman",
      "series_derived": false,
      "name": "Crispus",
      "title": "Caesar",
      "date_struck": "317 AD",
      "mint": "Heraclea",
      "mintmark": "•MHTΓ•",
      "obv": "DN FL IVL CRISPUS NOB CAES",
      "rev": "CAMP GATE PROVIDENTIAE CAESS",
      "diameter_mm": 20,
      "weight_g": 2.5,
      "die_axis": "6H",
      "denomination": "AE3 Nummus",
      "catalog_ref": "RIC VII HER 23",
      "flip_text": "Crispus, Caesar\n317 AD\nHeraclea •MHTΓ•\nOBV: DN FL IVL CRISPUS NOB CAES\nREV: CAMP GATE PROVIDENTIAE CAESS\n20mm 2.5g 6H\nAE3 Nummus\nRIC VII HER 23",
      "photos": {
        "obv": "photos/CC-0001-obv.jpg",
        "rev": "photos/CC-0001-rev.jpg"
      }
    }
  ]
}
```

### Field rules

- `id` is `CC-` plus four digits. Never invent an id.
- `num` is the four-digit padded form of `accession_n` (`1` → `0001`). Search must accept `1`, `0001`, `CC-0001`, `cc0001`.
- Unknown book fields stay `"?"` or `null` for numeric empties. Do not replace `?` with guesses.
- `flip_text` is the eight-line flip as stored, newline separated.
- `photos.*` paths are relative to `export/`.
- Sort `coins` by `accession_n` ascending.

### Public vs withheld

Emit:

`id num accession_n status series series_derived name title date_struck mint mintmark obv rev diameter_mm weight_g die_axis denomination catalog_ref flip_text photos`

Never emit:

`confidence date_acquired source notes market_comps chat_title date_on_desk date_accessioned last_updated`

## `manifest.json`

```json
{
  "spec": "cc-catalog-export/v1",
  "generated": "2026-09-05T20:00:00-05:00",
  "coin_count": 6,
  "missing_photos": [],
  "excluded": [
    { "id": "CC-0000", "reason": "Seed" }
  ]
}
```

## `enrichment/CC-0001.json`

Written only by the enrichment project, after confirm. One file per `id`. The generator reads it if present and `status` is `Ready`.

`spec` stays `cc-enrichment/v1`. F1–F3 add keys and section rules inside that spec. Do not emit a new spec string.

```json
{
  "spec": "cc-enrichment/v1",
  "id": "CC-0001",
  "status": "Ready",
  "stale": false,
  "confidence": "High",
  "series": "roman",
  "obverse": {
    "description": "What this flan shows. Worn areas named as worn.",
    "legend": "DN FL IVL CRISPUS NOB CAES",
    "legend_expanded": "Dominus Noster Flavius Iulius Crispus Nobilissimus Caesar",
    "legend_translated": "Our Lord Flavius Julius Crispus, most noble Caesar",
    "legend_note": ""
  },
  "reverse": {
    "description": "What this flan shows.",
    "type_note": "Camp gate, three turrets. Mintmark Heraclea, third officina (Γ)."
  },
  "mint": {
    "name": "Heraclea",
    "overview": "Where it is and what it was striking in this issue. Ancient site plus modern city and country."
  },
  "authority": {
    "name": "Constantine I and Licinius I, Augusti",
    "overview": "Who issued the coin. Not a copy of the portrait line."
  },
  "portrait": {
    "who": "Crispus, Caesar",
    "overview": "Who is on the obverse, then a short life of that person."
  },
  "struck": {
    "when": "317 AD",
    "overview": "Why this year or issue matters."
  },
  "also": [
    "Mintmark or value-mark decode.",
    "Reverse slogan or module note."
  ],
  "sources": [
    "RIC VII",
    "OCRE RIC VII Heraclea 23",
    "Wikipedia, \"Crispus\""
  ],
  "christian_history": {
    "relevance": "period",
    "paragraphs": [
      "Lactantius tutored Crispus in Gaul. Jerome places the post in Lactantius’s extreme old age."
    ],
    "sources": [
      "Jerome, On Illustrious Men 80"
    ]
  },
  "economics": {
    "evidence": "thin",
    "paragraphs": [
      "By 317 the bronze had shrunk from the earlier follis standard. No dated reform of this Heraclea piece is recorded."
    ],
    "sources": [
      "Wikipedia, \"Follis\""
    ]
  },
  "culture": {
    "paragraphs": [
      "The camp gate with PROVIDENTIAE CAESS presents the Caesars’ foresight as the guard that keeps a military base secure."
    ],
    "sources": [
      "Wikipedia, \"Providentia\""
    ]
  }
}
```

### Enrichment keys

Do not add keys beyond this list. Empty optional strings are `""`. Unused `also` is `[]`. Unused topical blocks may be omitted or given empty arrays.

| Key | Source / rule |
|---|---|
| `spec` | Always `cc-enrichment/v1`. |
| `id` | Catalog `id`. Filename matches. |
| `status` | `Draft`, `Ready`, or `Stale` only. |
| `stale` | Boolean. `false` on a new Ready file. |
| `confidence` | `High`, `Medium`, `Low`, or `?` for the writeup, not the attribution. |
| `series` | Copy from the catalog object. |
| `obverse.description` | This flan when photos exist; catalog type when they do not. |
| `obverse.legend` | Copy catalog `obv` unchanged (the book string). Do not replace it with a photo reading. `""` only if catalog `obv` is empty. |
| `obverse.legend_expanded` | Full Latin or Greek of that book string. Do not invent letters the catalog did not give. `""` if the book string is not a legend. |
| `obverse.legend_translated` | One English rendering of that book string. `""` if unused. |
| `obverse.legend_note` | Monogram, missing circular legend, or a reading problem. `""` when the triad is enough. |
| `reverse.description` | This flan / this type. Field marks and exergue as visible or as catalogued. |
| `reverse.type_note` | Type name and mintmark or officina note. `""` if unused. No reverse legend triad in v1. |
| `mint.name` | Copy catalog `mint` unchanged. |
| `mint.overview` | Ancient site, role in this issue, modern city and country. |
| `authority.name` | Issuing house, not a copy of `portrait.who` when they differ. |
| `authority.overview` | Settlement or house, without repeating the portrait life. On anonymous Christ types only, the attributed emperor’s life lives here. |
| `portrait.who` | Catalog name and title when that name is a person. `Christ, Class X` when the catalog name is Anonymous. |
| `portrait.overview` | Named person: one or two dense paragraphs — role and accession, major events, death or fate. Templates: CC-0004, CC-0008. Anonymous Christ type: the image and the class, not an emperor’s life. Templates: CC-0002, CC-0003, CC-0007. |
| `struck.when` | Copy catalog `date_struck`. |
| `struck.overview` | Why this date window. Say when the coin itself carries no year. |
| `also` | One or two short cards. Mark decode, then slogan or module note. Not a third biography. |
| `sources` | Core stack only: catalog corpus, matching online type, primary only if used, Wikipedia page title for the life. No URLs. No dealer houses. |
| `christian_history.relevance` | `direct` (named in a primary Christian text), `period` (same generation or closely linked), `background` (broader context). |
| `christian_history.paragraphs` | One or two dense paragraphs. State the connection plainly. |
| `christian_history.sources` | Works actually opened for this block. |
| `economics.evidence` | `priced` (price, wage, tax, or reform tied to this piece), `period` (same monetary regime), `thin` (setting only). |
| `economics.paragraphs` | One or two dense paragraphs. |
| `economics.sources` | Works actually opened for this block. |
| `culture.paragraphs` | One or two dense paragraphs on type, symbol, or setting. |
| `culture.sources` | Works actually opened for this block. |

### Biography seat

Follow the uploaded models.

- Catalog `name` is a person (Crispus, Anastasius I, Justin I, Justin II, Salonina, Trajan): life goes in `portrait.overview`. Byzantine emperors use the same seat as Roman ones. CC-0004 and CC-0008 are the pattern. If the face is a monogram and not a bust, say that in the first sentence of `portrait.overview`, then write the life there anyway.
- Catalog `name` is Anonymous and the face is Christ: `portrait.who` is `Christ, Class X`. `portrait.overview` is the image and the class. The associated emperor’s life goes in `authority.overview`. CC-0002, CC-0003, and CC-0007 are the pattern.

Do not put the same life in both blocks.

### Section budget

Phone length. Default is one or two dense paragraphs for a life block. Shorter for mint, struck, and a house-only authority. User may ask for more or less.

| Block | Length |
|---|---|
| `obverse.description` | One or two sentences. |
| Legend triad | Book string always. Expansion and translation when the string is a legend. |
| `reverse.description` + `type_note` | Type, marks, exergue. |
| `mint.overview` | One or two sentences. |
| `authority.overview` | One or two sentences for the house. One or two dense paragraphs when the anonymous-type life sits here. |
| `portrait.overview` | One or two dense paragraphs for a named person. A few sentences for Christ and the class. |
| `struck.overview` | One or two sentences. |
| `also` | Zero, one, or two strings. |
| `christian_history` | One or two dense paragraphs when the person or issue has a Christian-historical link. |
| `economics` | One or two dense paragraphs on the monetary setting. |
| `culture` | One or two dense paragraphs on type, symbol, or setting. |

Gifted or Sold may be one clause. Do not name the recipient, dealer, or price. The catalog object may badge those statuses on the site without this file repeating them.

### Enrichment status

| `status` | Meaning | Site |
|---|---|---|
| `Draft` | Written, not confirmed | Treat as missing. Flip viewer + badge. |
| `Ready` | User confirmed | Render sections. |
| `Stale` | Book facts changed after Ready | Render old prose, badge “writeup stale”. Re-run enrichment. |

`complete` is applied only downstream after review. This desk writes `Ready` after confirm.

`stale` is true when the enrichment project (or ingest) detects that catalog facts for this `id` no longer match the facts used to write the file. Ingest may set a sidecar flag even if this file still says `Ready`.

### Source stack

List works actually opened, in this order when they apply:

1. The catalog corpus for the series (RIC volume, Sear, RPC, DOC, MIBE).
2. The matching type line in OCRE, RPC Online, or the equivalent live corpus. Match the catalog number. Do not cite a neighbor type.
3. A named primary source only when a sentence in the file depends on it.
4. Wikipedia page title for the person whose life is sketched (`Wikipedia, "Crispus"`).

No padded bibliography. No URLs. No auction houses, dealer lists, or market comps.

### Invention and tone

- Same ban as the desk: no invented RIC/Sear/RPC numbers, weights, axes, provenances, prices.
- Do not copy Register `notes` or `market_comps` into enrichment. Those are desk memory.
- Do not change locked catalog fields to feed a prettier legend. `obverse.legend` is the catalog `obv` string, even when the photo is worn or the book stored a type phrase (`Monogram 8`, `IC-XC Christ enthroned`).
- Describe the photographs when they are present. If the obverse legend is gone on the flan, say that in `obverse.description` or `legend_note`. Do not rewrite `legend`. Missing photos are a chat note; variety and wear stay generic in the file.
- Historical sections are one or two dense paragraphs, not articles. User may ask for more or less at the confirm gate.
- Uncertainty is stated in the prose. Do not invent a `?` catalog line.
- No desk language in the file: “the book,” “this draft,” “project folder,” “cabinet photo missing.”

### Confirm gate

Enrichment project workflow:

1. Read `export/catalog.json` entry + the two photos.
2. Load the skill that matches `series`.
3. Draft the enrichment JSON in chat.
4. Ask: `Proceed with enrichment of CC-00xx?`
5. User may request more mint, less biography, etc. Revise the draft.
6. On yes, write `enrichment/CC-00xx.json` with `status: Ready` and `stale: false`. Do not write `complete`.
7. One open enrichment at a time unless the user splits work.

Reattribution in the book does not edit enrichment in place automatically. Next export marks it stale. Next enrichment chat replaces the file.

## Website ingest (lives in the website repo)

1. Read `export/catalog.json` and `export/photos/`.
2. For each coin, look for `enrichment/{id}.json`. Render sections only when `status` is `Ready`. Ignore any non-v1 status word.
3. Build search keys: `num`, unpadded `accession_n`, `id`, `id` without hyphen, lowercased `name`, `mint`, `catalog_ref`, denomination tokens. Ready files may also carry `obverse.legend`, `legend_expanded`, and `legend_translated` for display.
4. Resize photos to web derivatives (thumb ~400 px, page ~1200 px). Masters stay out of `dist/`.
5. Merge only here. Write pages and `search-index.json`.
6. URL: `/c/0001`. Also resolve `/c/CC-0001` and `/c/1` to the same page.
7. Missing or Draft enrichment: render images + flip + badge `Writeup pending`.
8. Stale enrichment: render images + flip + old sections + badge `Writeup stale`.
9. If `status` is Gifted or Sold, badge that status. Do not render source, recipient, dealer, or price.

Hamburger filters `series`. Search is the top control on every page.

## What Terraform sees

```
release/
  dist/          # generator output only
  terraform/     # storage account, static website, Front Door, DNS
```

Terraform does not read `catalog.json` or JPEGs. A script or CI uploads `dist/` to `$web`.

## v1 / v2 / v3

- **v1** — on-demand export, pages with grid photos and flip, client search, series menu, pending badge. Blob + Front Door.
- **v2** — enrichment project writes Ready files. Sections render under the flip.
- **v3** — add-to-home-screen / offline cache. Optional. Not in this spec.

## D1 — Astro

Website repo is an Astro static site. It reads `export/catalog.json` and `enrichment/*.json`, resizes photos in the build, writes `dist/` with `/c/0001` pages, and uploads that folder to Blob. No Eleventy path.
