# Coin Cabinet Accession Desk

Use this desk to research, identify, and accession coins into the authoritative cabinet register. It also prepares public-facing flips and produces a catalog export when requested.

The project is designed for an AI assistant that can consult the research skills in the repository-level `skills/` folder. `data/CC-Accession-Register.xlsx` is the live source of truth and may contain sensitive collection data. Do not upload or distribute it unless you have reviewed and explicitly authorized that use.

## Contents

```
cc-accession-desk-control-files/
├── AGENTS.md                          # Canonical project instructions
├── README.md                          # This file
├── data/
│   ├── CC-Accession-Register.xlsx     # Live register; handle as sensitive
│   └── cc-catalog.json
└── docs/
│   ├── CATALOG-EXPORT-SPEC.md         # Catalog export contract
│   ├── DESK-SPEC.md                   # Operating specification
│   ├── DESK-RULES.md                  # Desk rules digest
│   ├── FLIP-FORMAT.md                 # Public flip line order
│   ├── NEW-COIN-PROMPT.md             # Paste template for new intakes
│   ├── REGISTER-STRUCTURE.md          # Structure for a blank register
│   └── archive/
│       └── AGENTS-legacy.md            # Historical copy; do not use as instructions
```

The research skills are in the repository-level `skills/` folder, not inside this desk folder. Use `AGENTS.md` as the active project instructions; the archived copy is retained for reference only.

## Set up the AI project

1. Create a private AI project for the Accession Desk.
2. Add `AGENTS.md` as the project instructions. Add `docs/DESK-SPEC.md`, `docs/DESK-RULES.md`, `docs/FLIP-FORMAT.md`, `docs/CATALOG-EXPORT-SPEC.md`, `docs/NEW-COIN-PROMPT.md`, and `docs/REGISTER-STRUCTURE.md` as reference files.
3. Install or attach the six coin-research skills from `../skills/`: general, Roman, Greek, Byzantine, medieval, and misc.
4. Decide whether the AI service is authorized to access the live workbook. If yes, provide only the necessary workbook to this private project and keep a backup. If not, use a separate blank register built from `docs/REGISTER-STRUCTURE.md`; it cannot establish the live book's next available number.
5. Enable web search or browsing for source verification. Confirm the assistant can read and, if authorized, update the workbook before relying on it to write files.

## Series and skills

| Series     | Skill                                      | Notes |
|------------|--------------------------------------------|-------|
| roman      | ancient-coin-research-helper-roman         | RIC / RRC / RPC |
| greek      | ancient-coin-research-helper-greek         | Civic and Hellenistic |
| byzantine  | ancient-coin-research-helper-byzantine     | Official from Anastasius |
| medieval   | medieval-coin-research-helper              | European, Crusader, medieval Islamic |
| judaean    | ancient-coin-research-helper-misc          | No separate skill |
| misc       | ancient-coin-research-helper-misc          | Celtic, Punic, Parthian, etc. |
| modern     | closest skill                              | |
| unclear    | ancient-coin-research-helper-general       | |

## Intake and export

1. Start a new chat and use `docs/NEW-COIN-PROMPT.md`; attach coin photos and any measurements or observations.
2. The assistant reads the Control sheet for the next ID, drafts the attribution and flip, and asks for confirmation. It must not write the register before confirmation.
3. After an authorized write, verify the row and confirm that the Control sheet advanced before treating the accession as complete. Unknowns may be accessioned with catalog `?` and Low confidence; reattributions retain their existing IDs.
4. Export only when explicitly requested. Follow `docs/CATALOG-EXPORT-SPEC.md`; export does not create enrichment files.
5. For companion-site writing, hand off the exported catalog object and photos to the separate Companion Enrichment Desk. Do not give that desk the register.

## Design principles

- The spreadsheet is the single source of truth. Project memory is only a shadow.
- Confirm gate before any write.
- Flip is the public face; private notes, prices, and sources stay in the Register.
- Identification gates prevent common first-pass errors (wrong module, wrong script, unread devices named as attributes).
- Export is on demand and does not include private fields.

Photo masters are not included in this desk folder.
