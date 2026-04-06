# Cancer Data Extraction with LLMs — Notebook Outline

The goal is to extract information from clinical notes around cancer cases using LLMs, specifically detecting cancer diagnoses and determining values for **NAACCR** (North American Association of Central Cancer Registries) variables. Participants also build tools to connect to the ClinicalTrials.gov API.

Two angles of generative AI experience:
1. Using chat agents to explore data/documentation and develop code
2. Writing code that submits queries to an LLM (prompts, tools, document retrieval)

---

## 1. Intro to LLMs

Overview/introductory section.

---

## 2. Dataset and Documentation Exploration

Work with a dataset of synthetic clinical notes for patients with a mix of cancer and non-cancer diagnoses.

- **2a.** Ask the chatbot to describe the files in the dataset directory.
- **2b.** Write code to display descriptive visualizations of the file contents.
- **2c.** Ask the model about ground truth categories by searching manuals and/or the web.

---

## 3. Cancer Detection

Determine whether a patient has cancer from their clinical notes. Start with a single case, then loop through all test cases and evaluate against ground truth.

- **3a.** Write a function to submit prompts to an LLM using the OpenAI Responses SDK.
- **3b.** Write a function that loads notes from a single case (`notes_by_case.json`) and submits them to determine cancer status. Test against `ground_truth.csv`.
- **3c.** Create a method for submitting a followup prompt — if the patient has cancer, determine the affected tissue.
- **3c.** Determine cancer status and affected tissue for all cases in `TEST_CASE_IDS`.
- **3d.** Compare responses to ground truth and visualize performance metrics (accuracy, precision, recall, etc.).

---

## 4. NAACCR Variable Coding

Covers tool calling, information retrieval, ReAct loops, and structured outputs.

A `VariableCodingResult` Pydantic model is provided for structured output:
- `variable_name`, `variable_id`, `value`, `reasoning`, `confidence`

### Tasks

- **4a.** Develop a method for determining the NAACCR primary site code for a case. Define custom tools (e.g., look up coding rules/manuals) and/or call built-in ones (e.g., web search). Use a Pydantic data model for consistent output parsing.
- **4b.** Test method performance against the ground truth.
- **4c.** Iterate to improve performance — bring in other sources, include images (e.g., site-specific breast guidelines), etc.
- **4d.** Try other variables with ground truth values. Note: many NAACCR variables have site-specific instructions requiring tissue/primary site identification first. Site-specific coding rules for histology are included; others are on the [NCI SEER website](https://seer.cancer.gov/manuals/2026/appendixc.html).

---

## 5. Connect to ClinicalTrials.gov API

Connect extracted cancer information to real-world clinical trials using the [ClinicalTrials.gov API v2](https://clinicaltrials.gov/data-api/api) (free, unauthenticated access).

Two tools to build:
1. **Simple search** — query by condition and/or search terms
2. **Advanced search** — add demographic and geographic filters (e.g., location within North Carolina)

### Tasks

- **5a.** Review the ClinicalTrials.gov API documentation. Create functions around the `/studies` endpoint to search recruiting clinical trials. Hints:
  - Create a data model to validate request data
  - Focus on the `query` and `filter` parameters
  - Test with narrow queries (e.g., specific facility, specific condition)
- **5b.** Create functions for advanced queries using query construction features:
  - Use patient information to set search limits
  - Get lat/long of cities in case metadata and filter by distance
  - Include common synonyms (e.g., colon, colorectal)
