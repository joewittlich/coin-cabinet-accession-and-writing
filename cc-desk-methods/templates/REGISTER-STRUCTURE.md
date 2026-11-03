# Register Structure

The accession book is an Excel workbook (`CC-Accession-Register.xlsx`) with four sheets. This document describes the structure for creating a blank register; it does not replace the live workbook, which may contain private collection data.

## Sheets

### Control

Tracks the next available CC number. Key cells (approximate layout):

- Last written number / Last written ID
- Next number to assign / Next ID to assign
- Named rows / Accessioned coins counts

**Rule:** Always read Control at the start of intake and after every write. Never assign a CC number from memory.

### Register

One row per coin. Headers (column order):

1. `accession_n` — integer (1 for CC-0001)
2. `accession_id` — visible text `CC-00xx` (never a formula)
3. `status` — Accessioned / Gifted / Sold / Seed / Reserved
4. `name`
5. `title` — obverse rank (required, never blank or ?)
6. `date_struck` — issue date
7. `mint`
8. `mintmark`
9. `obv`
10. `rev`
11. `diameter_mm`
12. `weight_g`
13. `die_axis` — 6H style
14. `denomination`
15. `catalog_ref` — primary catalog
16. `series` — roman / greek / byzantine / medieval / judaean / misc / modern
17. `confidence` — High / Medium / Low / ?
18. `date_acquired`
19. `source`
20. `notes`
21. `market_comps`
22. `photo_obv` — e.g. CC-0001-obv.jpg
23. `photo_rev`
24. `chat_title`
25. `date_on_desk`
26. `date_accessioned`
27. `last_updated`
28. `flip_text` — the eight-line flip as a single cell or multi-line

Live second catalog numbers stay in `notes` as prose. No extra columns.

### FlipFormat

Authoritative line order for the public flip (see `docs/FLIP-FORMAT.md`).

### DeskRules

Digest of operating rules (see `docs/DESK-RULES.md`).

## Seed

CC-0000 is a dummy seed coin, status=Seed, not a cabinet coin. First real coin is CC-0001. Never reuse numbers. Reattribute in place.
