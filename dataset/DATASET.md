# Dataset

83 synthetic patient cases for a clinical data extraction hackathon. Each case has a unique 8-character hex ID (e.g., `050788b4`).

## Top-Level Files

### `patient_demographics.csv`

- **Rows:** 83 (one per case)
- **Columns:** `case_id`, `age`, `sex`, `city`, `state`
- All patients are from North Carolina.

### `ground_truth.csv`

- **Rows:** 83 (one per case)
- **Columns:** `case_id`, `has_cancer`, `tissue`, `400`, `410`, `522`, `523`, `764`, `820`, `830`, `3843`, `3844`
- `has_cancer`: 1 if the case involves a cancer diagnosis, 0 otherwise.
- `tissue`: cancer site (e.g., lung, breast, prostate). Empty for non-cancer cases.
- Numeric columns (`400`, `410`, `522`, etc.) are NAACCR item numbers representing cancer registry coding fields. Empty for non-cancer cases.

### `notes_by_case.json`

- **Size:** ~2.8 MB
- **Structure:** JSON object keyed by `case_id`. Each value is an array of clinical visit notes.
- **Cases:** 83 | **Total notes:** 661 | **Notes per case:** 3–14 (avg 8)
- Each note contains:
  - `visit_number` — sequential visit index
  - `note_date` — date of the visit (YYYY-MM-DD)
  - `content` — full text of the clinical note
  - `symptoms_reported` — list of symptoms
  - `vitals` — dict of vital signs (e.g., Temperature, Heart_Rate, Blood_Pressure)
  - `tests_ordered` — list of tests
  - `diagnoses_considered` — list of diagnoses
  - `medications` — list of medications
  - `follow_up_recommendations` — list of follow-up items

## `cases/` Directory

83 JSON files, one per case (e.g., `050788b4.json`). Size range: 30–167 KB.

Each case JSON contains:

| Key | Type | Description |
|-----|------|-------------|
| `case_id` | string | 8-char hex identifier |
| `clinical_variables` | object | `primary_condition`, `comorbidities`, `age`, `sex`, `risk_factors` |
| `difficulty` | string | `easy`, `medium`, or `hard` |
| `case_type` | string | `acute`, `chronic`, or `resolved` |
| `intended_outcome` | string | e.g., `undiagnosed` |
| `narrative` | string | Prose narrative of the patient's medical journey |
| `timeline` | array | Detailed visit-by-visit timeline (see below) |
| `notes` | array | Clinical notes with structured metadata (see below) |
| `final_medical_history` | object | `demographics`, `known_conditions`, `current_medications`, `prior_visit_summaries`, `allergies` |

### `timeline` entries

Each entry in the `timeline` array represents a visit with:

`visit_number`, `visit_date`, `clinician_specialty`, `reason_for_visit`, `is_related_to_main_illness`, `note`, `patient_age`, `patient_sex`, `symptoms`, `vitals`, `relevant_history`, `known_conditions`, `current_medications`, `allergies`, `visit_scenario`, `examination_findings`, `tests_ordered`, `test_results`, `treatments_administered`, `patient_response`, `disease_progression_notes`

### `notes` entries

Each entry in the `notes` array contains:

`visit_number`, `clinician_specialty`, `note_date`, `content`, `symptoms_reported`, `vitals`, `tests_ordered`, `diagnoses_considered`, `medications`, `follow_up_recommendations`

## `cases/views/` Directory

83 HTML files, one per case (e.g., `050788b4.html`). These are styled, human-readable renderings of the case data with visit timelines, badges for difficulty/type, and formatted clinical notes. Suitable for viewing in a browser.

## Relationships

- All files share `case_id` as the common key.
- `notes_by_case.json` consolidates the notes from all individual case JSON files into a single file for bulk access.
- `ground_truth.csv` contains the target labels for the extraction task — the NAACCR coding fields that should be derived from the clinical notes.
- `patient_demographics.csv` provides basic demographics that may also appear within the case JSON `clinical_variables`.
