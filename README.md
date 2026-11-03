# Coin Cabinet AI Projects

This folder contains instructions, research skills, and control files for running two related but deliberately separate coin-collection workflows with an AI platform such as Claude, another project-based assistant, or an agent with file and web tools.

## Keep the projects separate

| AI project | Purpose | Authority | May write |
|---|---|---|---|
| **Accession Desk** | Identify and accession coins; prepare public flips; export the catalog when requested | `CC-Accession-Register.xlsx` | The authorized accession register and, on request, `export/catalog.json`, `export/manifest.json`, and listed photo copies |
| **Companion Enrichment Desk** | Write visitor-facing descriptions for coins already in the exported catalog | The catalog object supplied for that coin, checked against its photos | `enrichment/CC-00xx.json`, only after the user confirms the draft |

Do not combine these into one project. The enrichment project must not open or change the accession workbook, assign accession numbers, or change catalog facts. The accession project does not write enrichment files as part of accession or export.

## Files in this package

```text
coin-cabinet-accession-and-writing/
├── README.md
├── cc-accession-desk-control-files/
│   ├── README.md
│   ├── AGENTS.md
│   ├── data/
│   │   ├── CC-Accession-Register.xlsx   # Live collection data; review before sharing
│   │   └── cc-catalog.json              # Empty sample export
│   ├── docs/
│   │   ├── CATALOG-EXPORT-SPEC.md
│   │   ├── DESK-RULES.md
│   │   ├── DESK-SPEC.md
│   │   ├── FLIP-FORMAT.md
│   │   ├── NEW-COIN-PROMPT.md
│   │   └── REGISTER-STRUCTURE.md
├── cc-enrichment-desk-control-files/
│   ├── AGENTS.md
│   ├── README.md
│   ├── docs/
│   │   ├── CATALOG-EXPORT-SPEC.md
│   │   ├── ENRICHMENT-PROJECT-BRIEF.md
│   │   ├── historical-sources-for-enrichment.md
│   │   └── research-guide.md
│   ├── data/
│   │   └── cc-catalog.json             # Empty sample/template; supply the current export
│   └── research/
│       └── research-sources.json
├── skills/
│   ├── ancient-coin-research-helper-byzantine/SKILL.md
│   ├── ancient-coin-research-helper-general/SKILL.md
│   ├── ancient-coin-research-helper-greek/SKILL.md
│   ├── ancient-coin-research-helper-misc/SKILL.md
│   ├── ancient-coin-research-helper-roman/SKILL.md
│   ├── companion-catalog-enrichment/SKILL.md
│   └── medieval-coin-research-helper/SKILL.md
└── LICENSE
```

The six coin-research skills are shared by both workflows. The `companion-catalog-enrichment` skill is for the enrichment project only. Each skill's instructions are in its `SKILL.md`.

## Set up on a project-based AI platform

Platform features vary. Treat project instructions as operating rules, project knowledge as reference files, and skills as reusable procedures. If your platform does not support a particular feature, use its closest equivalent and make the distinction explicit in the project instructions.

1. Create **two private projects**, one for the Accession Desk and one for the Companion Enrichment Desk.
2. Set up each project from the specific files listed below. Do not upload this whole folder wholesale: it contains the accession workbook, and duplicated or irrelevant instructions can cause confusion.
3. Add the appropriate operating instructions below to each project's instruction field. Upload the linked specs and prompts as project knowledge rather than pasting all of them into every chat.
4. If the platform supports skills, install the relevant skill folders. Otherwise, add the relevant `SKILL.md` files as reference material and tell the assistant to consult the matching skill before doing that work. Do not rely on skill frontmatter alone to activate a skill.
5. Enable web search or browsing for research and source verification when available. The coin skills require claims and catalog references to be checked, not guessed.
6. Before relying on file writes, confirm the platform has access to the intended project workspace and can create or update the required files. If it cannot, have it produce the proposed content for you to save. Keep a backup of the register before any authorized workbook update.
7. Start each coin task in a new chat. Use the supplied prompt templates and require the confirmation gate before any write.

### Project 1: Accession Desk

**Project instructions:** Use [`AGENTS.md`](./cc-accession-desk-control-files/AGENTS.md) as the core instructions. Add [`DESK-SPEC.md`](./cc-accession-desk-control-files/docs/DESK-SPEC.md), [`DESK-RULES.md`](./cc-accession-desk-control-files/docs/DESK-RULES.md), [`FLIP-FORMAT.md`](./cc-accession-desk-control-files/docs/FLIP-FORMAT.md), [`NEW-COIN-PROMPT.md`](./cc-accession-desk-control-files/docs/NEW-COIN-PROMPT.md), and [`REGISTER-STRUCTURE.md`](./cc-accession-desk-control-files/docs/REGISTER-STRUCTURE.md) as project knowledge. The export contract is [`CATALOG-EXPORT-SPEC.md`](./cc-accession-desk-control-files/docs/CATALOG-EXPORT-SPEC.md).

**Skills to install:** `ancient-coin-research-helper-general`, `ancient-coin-research-helper-roman`, `ancient-coin-research-helper-greek`, `ancient-coin-research-helper-byzantine`, `medieval-coin-research-helper`, and `ancient-coin-research-helper-misc`.

**Data and boundaries:**

- The `data/` folder contains `CC-Accession-Register.xlsx`. Treat it as a live, potentially sensitive collection record. Do not upload the whole package or workbook to an AI service unless you have reviewed and explicitly authorized that use.
- If you choose to use an AI platform to read or update the register, make the authorized workbook available only to the private Accession Desk project. It is the source of truth; the Control sheet supplies the next number. Do not use chat memory to assign numbers.
- If you do not want to upload the live register, use a separate blank workbook based on [`REGISTER-STRUCTURE.md`](./cc-accession-desk-control-files/docs/REGISTER-STRUCTURE.md); the AI cannot assign or verify the actual next number without access to the authoritative Control sheet.
- The project may analyze coin photos attached for the task. Follow the desk rules about not retaining those images in the project workspace.
- Let the assistant draft the identification and flip first. It must ask “Proceed with accession of CC-00xx?” and wait for an affirmative response before writing the register.
- After a write, require the assistant to recalculate and verify the row and next number before reporting success.
- Export only when explicitly requested. Use the export contract; do not generate enrichment files during accession or export.

**Start a task:** Use the template in [`NEW-COIN-PROMPT.md`](./cc-accession-desk-control-files/docs/NEW-COIN-PROMPT.md). For an export, explicitly ask for the on-demand catalog export described in the export spec.

### Project 2: Companion Enrichment Desk

**Project instructions:** Use [`AGENTS.md`](./cc-enrichment-desk-control-files/AGENTS.md) as the core instructions. Add [`ENRICHMENT-PROJECT-BRIEF.md`](./cc-enrichment-desk-control-files/docs/ENRICHMENT-PROJECT-BRIEF.md) and [`CATALOG-EXPORT-SPEC.md`](./cc-enrichment-desk-control-files/docs/CATALOG-EXPORT-SPEC.md) as project knowledge. The [companion enrichment skill](./skills/companion-catalog-enrichment/SKILL.md) contains the detailed workflow. Research references are in `docs/` and `research/`.

**Skills to install:** `companion-catalog-enrichment` and the six coin-research skills listed above. Route to the coin skill matching the catalog's `series` value; use the general skill if the series is unclear.

**Input and boundaries:**

- Give the project one catalog object from the current accession export (or a populated catalog file and the target ID) and the matching obverse and reverse photos. The checked-in `data/cc-catalog.json` is an empty sample, not the live catalog.
- The supplied catalog facts are locked. If the photos appear to contradict them, stop and send the coin back to the Accession Desk rather than silently re-identifying it.
- Draft and show the JSON first. The assistant must ask “Proceed with enrichment of CC-00xx?” and wait for confirmation before writing `enrichment/CC-00xx.json`.
- On confirmation, the file uses `cc-enrichment/v1`, `status: "Ready"`, and `stale: false`. This desk must never set `status` to `complete`.
- Never provide the accession workbook to this project. It does not assign numbers, export the catalog, build or deploy the website, or include dealer, price, recipient, or private desk notes in enrichment.
- `CC-0018` is held as a schema-test piece; do not enrich it unless that hold has been lifted.

The enrichment prompt and confirmation steps are in [`ENRICHMENT-PROJECT-BRIEF.md`](./cc-enrichment-desk-control-files/docs/ENRICHMENT-PROJECT-BRIEF.md).

## Normal handoff between projects

1. Accession and verify the coin in the Accession Desk.
2. When requested, export the catalog and photo list using the export contract.
3. Provide the Companion Enrichment Desk only the target coin's catalog object and its two photos.
4. Review the proposed enrichment and confirm before the assistant writes the JSON.
5. Treat the website repository as a separate downstream system; this package does not publish or deploy the site.

## Notes

- The export and enrichment specs define the data contracts. Where project notes or an AI's memory disagree with the current spec or accession register, follow the applicable authority named above.
- [`research-guide.md`](./cc-enrichment-desk-control-files/docs/research-guide.md) provides a standalone, optional research workflow for the Companion Enrichment Desk; it requires no local scripts or separate research repository. [`research-sources.json`](./cc-enrichment-desk-control-files/research/research-sources.json) is an optional source directory.
- An older accession instruction copy is retained at `cc-accession-desk-control-files/docs/archive/AGENTS-legacy.md` for reference only. Use the root [`AGENTS.md`](./cc-accession-desk-control-files/AGENTS.md) as the active instructions.
- These files define workflows, not a guarantee of any platform's capabilities. Always review proposed identifications, research, and file changes.

## License

The entire package, including the accession workbook and collection data, is licensed under the MIT License in [`LICENSE`](./LICENSE). The workbook may contain sensitive information; review its contents and confirm you have authority to share them before distributing a copy.
