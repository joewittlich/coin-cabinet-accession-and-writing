# Desk Rules

Extracted from the DeskRules sheet of the Accession Register.

Desk rules
Scope
Anything that enters the cabinet. Ancient, medieval, modern, tokens, junk-box, fantasy. Route ID work to the matching coin skill.
Skills
general = series unclear. roman = RIC/RRC/RPC/Latin imperial. greek = civic and Hellenistic. byzantine = official Anastasius–Palaiologan. medieval = European hammered, Crusader, standard medieval Islamic. judaean = Hasmonean, Herodian, Judaean prefects/procurators, Jewish revolt and Bar Kokhba, Holy Land civic when Hendin/TJC is the primary catalog (ID skill: misc; no separate judaean skill). misc = Celtic, Punic, Parthian, Sasanian, Nabataean, other non-core, unidentifiable. companion-catalog-enrichment = only after the Register row is written, and only when the user asks to enrich.
Intake
New project chat. User drops images and/or a description. Read Control!B8 and announce that ticket. Do not assign from project memory. Photo names (downstream, not stored here): CC-0001-obv.jpg / CC-0001-rev.jpg.
Identify then ask
Describe what is visible. Identify with catalog evidence, confidence, and series (roman / greek / byzantine / medieval / judaean / misc / modern). One lock sentence (Locked / Guessed or unread). Draft the eight-line flip. Do not write the Register until the user says to proceed with accession.
Accession write
On confirmation, write one Register row. accession_n is the integer (1 for CC-0001). accession_id is visible text CC-0001. status=Accessioned. series=roman | greek | byzantine | medieval | judaean | misc | modern. judaean still uses the misc ID skill. One physical coin = one CC number. Recalc and re-read Control before saying the row is written.
Reattribution
Keep the same CC number. Change flip fields and Register details. Do not retire or reuse numbers.
Unknowns
Unknown coins may be accessioned: name Unknown (or a type label), catalog ?, confidence Low, named alternatives in notes. Never invent catalog numbers, weights, axes, provenances, prices, or CC numbers.
Chat rename
When attribution is complete, give the user this title to apply: NAME, DATE, DENOMINATION, CATALOG CC-0000. Example: CRISPUS, 317AD, AE3, RIC VII HER 23 CC-0001. For anonymous folles: ANONYMOUS CLASS X, DATE, AE FOLLIS, CATALOG CC-0000. Example: ANONYMOUS CLASS A2, c. 976–1030/5 AD, AE FOLLIS, SEAR 1813 CC-0189.
Register vs flip
Flip = eight public lines. Register also holds series, date acquired, source, confidence, notes, market comps, photo filenames, chat title, desk and accession dates. Live second catalog numbers stay in notes as prose. Gifted and Sold stay on the book.
Authority
This workbook is the accession book. Control is Next ID. Read Control at every intake and after every write. Project memory may keep a per-coin reading copy (shadow list) and format decisions. The shadow list is not the book. Never announce a CC number from memory.
Parallel tickets
Default is one open intake. If two coins are on the desk at once, assign sequential numbers explicitly in the chat before identifying the second.
Export
On demand only. Rebuild export/catalog.json and export/manifest.json from Accessioned, Gifted, and Sold. Exclude Seed and Reserved. Copy present CC-*-obv.jpg / CC-*-rev.jpg into export/photos/. List missing photos in the manifest. Missing faces are expected (photos live downstream). Do not write enrichment during export. After the files exist, reply in the canonical shape in AGENTS.md. No coin essay in that reply.
Title / rank
Register column title is the rank of the person (or type) on the obverse. Required on every drafted row. Never leave blank or ?. Roman: Caesar / Augustus / Augusta / Divus (use the rank this issue carries). Official Byzantine: Emperor (or Empress). Anonymous folles: Class A2 / Class C / Class D etc. Line 1 is the issuing authority; denomination line carries “AE Follis, Anonymous Class X”. Greek: civic ethnic or royal rank if a king. Medieval: the rank on the coin (King, Prince, …). Judaean: the office on the coin (King, Ethnarch, Prefect, Procurator, …). Modern / misc: the office or type label that belongs on flip line 1. Flip line 1 is always Name, rank.
Enrichment
After the Register row is written, if the user asks to enrich, load companion-catalog-enrichment, draft enrichment/CC-00xx.json, ask to proceed, and write the file only on yes with status Ready. Do not enrich during identification. Do not change book facts. Do not copy dealer, price, recipient, or desk notes into the file.
Issue date
Flip line 2, date_struck, and the chat-title date are the issue date, not the reign span. If the catalog dates the issue more narrowly than the reign (a Prieur group, a regnal year, a consulship), use that date. Keep the reign in notes when it differs. If no issue date is published, use the reign or the catalog bracket. Unknown = ?.

