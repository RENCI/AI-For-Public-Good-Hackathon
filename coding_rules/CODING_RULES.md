# Coding Rules

Cancer registry coding reference files for assigning NAACCR data item codes (histology, primary site, grade) from clinical notes. Used alongside the SEER and FORDS reference manuals.

## File Categories

### Histology Tables (`{Site}_histology.json`) — 34 files

Site-specific lookup tables for assigning ICD-O-3 histology codes. Each file is a JSON object with two keys:

- **`Required Histology Terms`** — indexed dict of term combinations that must be present to assign a specific histology code
- **`Histology Combination Term and Code`** — indexed dict mapping term combinations to their histology codes (e.g., `8500`, `8522`)

**Sites covered (30):** Anus, Bone, Breast, Colon, Esophagus, Fallopian Tube, Gallbladder/Extrahepatic Bile Ducts, Head and Neck (2 files), Kidney, Liver/Intrahepatic Bile Duct, Lung (2 files), Malignant CNS, Melanoma, Non-Malignant CNS (2 files), Ovary, Pancreas, Penis/Scrotum, Peritoneum, Prostate, Small Intestine/Ampulla of Vater, Soft Tissue, Stomach, Testis, Thymus, Thyroid, Urinary, Uterine Cervix, Uterine Corpus, Vagina, Vulva

Some sites have multiple files (e.g., `Lung_histology_1.json`, `Lung_histology_2.json`) when the table is split across pages.

### Site Group Instructions (`{Site}_sitegroupinstr.json`) — 9 files

Mapping tables from anatomical terms/descriptions to ICD-O-3 topography (primary site) codes. Each file is a JSON object with two keys:

- **`Terms and Descriptive Language`** — indexed dict of anatomical terms and descriptive language
- **`Site Term and Code`** — indexed dict of standardized site names with their C-codes (e.g., `Nipple C500`, `Central portion of breast C501`)

**Sites covered (7):** Breast, Colon, Lung, Malignant CNS (2 files), Non-Malignant CNS (2 files), Urinary

### Grade Dictionaries — 2 files

- **`dict_grade_clin.json`** (269 KB) — Clinical grade coding rules
- **`dict_grade_path.json`** (432 KB) — Pathological grade coding rules

Both are JSON objects keyed by site/organ name (141 entries). Each entry contains:

- `notes` — markdown-formatted coding instructions and special rules
- `definition` — column schema (Code, Description)
- `rows` — array of `[code, description]` pairs defining valid grade values (e.g., `["1", "G1: Low combined histologic grade..."]`)

### SEER Manual Selected Codes (`seer_manual_selected_codes.json`) — 43 KB

Definitions for key NAACCR data items relevant to this hackathon. JSON object with 5 entries keyed by NAACCR item number:

| Item | Name | Description |
|------|------|-------------|
| `390` | Date of Diagnosis | When the neoplasm was first identified |
| `400` | Primary Site | ICD-O-3 topography code for tumor location |
| `410` | Laterality | Side of a paired organ involved |
| `523` | Histologic Type ICD-O-3 | Morphology code for tumor type |
| `764` | CS Tumor Size | Size of the primary tumor |

Each entry contains: `naaccr_name`, `naaccr_xml` (XML element name), `description` (full coding instructions from the SEER manual).

### Lookup Tables — 4 files

These map a primary site name to the relevant JSON file(s) in this directory. Available in both CSV and JSON formats.

**`primary_site_to_histology_lookup`** (`.json` / `.csv`)
- Maps 30 primary site names to their histology table file(s)
- Example: `"Breast" → ["Breast_histology.json"]`, `"Lung" → ["Lung_histology_1.json", "Lung_histology_2.json"]`

**`primary_site_to_sitegroupinstr_lookup`** (`.json` / `.csv`)
- Maps 7 primary site names to their site group instruction file(s)
- Example: `"Breast" → ["Breast_sitegroupinstr.json"]`, `"Malignant CNS" → ["Malignant_CNS_sitegroupinstr_1.json", "Malignant_CNS_sitegroupinstr_2.json"]`

## Typical Workflow

1. Identify the cancer's **primary site** from the clinical notes
2. Use `primary_site_to_histology_lookup.json` to find the correct histology table(s)
3. Use `primary_site_to_sitegroupinstr_lookup.json` to find site group instructions (if available for that site)
4. Match terms from the clinical notes against the histology table to determine the ICD-O-3 histology code
5. Match anatomical descriptions against site group instructions to determine the ICD-O-3 topography code
6. Use `dict_grade_clin.json` / `dict_grade_path.json` to assign grade codes
7. Use `seer_manual_selected_codes.json` for guidance on coding other NAACCR items (date of diagnosis, laterality, tumor size)
