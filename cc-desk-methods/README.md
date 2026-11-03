# Coin Cabinet Accession Desk Methods

This repository packages a complete, ready-to-use method for running a personal coin cabinet accession desk: identifying, attributing, and accessioning ancient, medieval, and related coins into a structured register.

The desk is designed for use with an AI assistant that can consult the research skills in the sibling `skills/` folder. This folder currently includes `CC-Accession-Register.xlsx`, a live collection record that may contain sensitive information. The root MIT license applies to the whole package, including this workbook; review its contents and confirm you have authority before distributing a copy.

## Contents

```
cc-desk-methods/
├── CC-Accession-Register.xlsx         # Treat as private collection data
├── cc-catalog.json
├── README.md                          # This file
├── docs/
│   ├── AGENTS.md                      # Canonical project instructions
│   ├── DESK-SPEC.md                   # Operating specification (v3, locked 2026-10-18)
│   ├── DESK-RULES.md                  # Desk rules digest
│   ├── FLIP-FORMAT.md                 # Public flip line order
│   ├── CATALOG-EXPORT-SPEC.md         # Export contract for companion site
│   └── NEW-COIN-PROMPT.md             # Paste template for new intakes
└── templates/
    └── REGISTER-STRUCTURE.md          # How to build a blank register
```

The research skills are in the sibling `../skills/` folder, not inside this directory. Use `docs/AGENTS.md` as the canonical project-instructions file. A second, nested copy exists at `cc-desk-methods/docs/AGENTS.md`; do not add that duplicate to the AI project.

## How the desk works

1. **Ticket.** A new coin arrives in a chat with photos (and optional notes). The agent reads the Control sheet of the register and announces the next CC number.
2. **Identify.** The agent describes what is visible, loads the matching research skill, and proposes an identification with confidence and series. Identification gates (module, script, second search, devices) must pass before the leading ID is locked.
3. **Lock and flip.** One lock sentence (what is actually read vs. type-supplied), then an eight-line public flip (drafted in a code block). User may correct mintmark, axis, or catalog.
4. **Confirm.** Agent asks “Proceed with accession of CC-00xx?” Only on yes does it write the Register row.
5. **Write and verify.** Writes the row with visible `CC-00xx` text (not a formula), recalculates, and re-reads Control to confirm Next ID advanced. Then returns the completed flip and a chat title.
6. **Optional enrichment.** After the row exists, the user may request a phone-length companion-site writeup using the enrichment skill.

Unknowns may be accessioned with catalog `?` and Low confidence. Reattribution happens in place; numbers are never reused.

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

## Using this package

1. Create a private AI project and add `docs/AGENTS.md` as its operating instructions.
2. Add `docs/DESK-SPEC.md`, `docs/DESK-RULES.md`, `docs/FLIP-FORMAT.md`, `docs/CATALOG-EXPORT-SPEC.md`, `docs/NEW-COIN-PROMPT.md`, and `templates/REGISTER-STRUCTURE.md` as reference files.
3. Install or attach the six coin research skills from `../skills/`: general, Roman, Greek, Byzantine, medieval, and misc.
4. Decide whether the AI platform may access the current register. If not, create and use a separate blank workbook from `templates/REGISTER-STRUCTURE.md`; a blank book is not a substitute for the live book's next-number authority.
5. Start each intake in a new chat using `docs/NEW-COIN-PROMPT.md`. Require confirmation before any workbook write.
6. For the companion website workflow, use the separate `cc-enrichment-desk-control-files` project materials described in the top-level README.

## Design principles

- The spreadsheet is the single source of truth. Project memory is only a shadow.
- Confirm gate before any write.
- Flip is the public face; private notes, prices, and sources stay in the Register.
- Identification gates prevent common first-pass errors (wrong module, wrong script, unread devices named as attributes).
- Export is on demand and does not include private fields.

## License

The entire package, including this workbook, is licensed under the MIT License in the repository root. Photo masters are not included.

---

*Desk spec v3 locked 2026-10-18. Package assembled for sharing.*
