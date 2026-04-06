# Cancer Data Extraction with LLMs

**UNC AI For Public Good 2026 — Hackathon Activity**

## Overview

In this activity you will use large language models (LLMs) to extract cancer-related information from synthetic clinical notes and connect patients to potentially relevant clinical trials. Along the way you will get hands-on experience with two sides of generative AI:

1. **Using AI coding agents** (Claude Code, Codex CLI, etc.) to explore data, read documentation, and write analytical code
2. **Writing code that calls LLMs** — crafting prompts, defining tools, retrieving documents, and parsing structured outputs

The core activity walks through detecting cancer diagnoses, coding [NAACCR](https://www.naaccr.org/) (North American Association of Central Cancer Registries) variables from clinical notes, and searching the [ClinicalTrials.gov](https://clinicaltrials.gov/) database for matching trials. That said, you are encouraged to follow any rabbit holes or use cases that pique your interest.

## Repository Structure

```
.
├── hackathon_notebook.ipynb        # Main activity notebook
├── intro_to_llms.ipynb             # Background on LLMs, prompts, tools, and context
├── coding_agent_best_practices.md  # Tips for working effectively with AI coding agents
├── hackathon_notebook_outline.md   # Outline of the activity sections
├── config.yaml                     # Model list, test case IDs, and runtime settings
├── dataset/                        # Synthetic clinical notes and ground truth labels
│   ├── notes_by_case.json          #   All clinical notes keyed by case ID
│   ├── ground_truth.csv            #   Target NAACCR variable values for each case
│   ├── patient_demographics.csv    #   Age, sex, city, state for each patient
│   └── cases/                      #   Individual case JSON files + HTML views
├── coding_rules/                   # Site-specific histology, grade, and coding tables (JSON)
└── manuals/                        # NAACCR/SEER reference PDFs (FORDS, staging, coding)
```

See `dataset/DATASET.md`, `coding_rules/CODING_RULES.md`, and `manuals/MANUALS.md` for detailed descriptions of each directory's contents.

## Activity Outline

The main notebook (`hackathon_notebook.ipynb`) is organized into five sections. Start at the beginning or jump to whatever interests you most.

### 1. Best Practices for Coding Agents
Read `coding_agent_best_practices.md` for tips on getting the most out of AI coding assistants — context management, iterative prompting, plan mode, and more.

### 2. Dataset and Documentation Exploration
Use your coding agent to explore the dataset, create visualizations, and learn about NAACCR variables by searching the included manuals and the web.

### 3. Cancer Detection
Build functions that send clinical notes to an LLM and determine whether a patient has cancer (and which tissue is affected). Run across all test cases and evaluate performance with metrics like accuracy, precision, and recall.

### 4. NAACCR Variable Coding
Dive into tool calling, information retrieval, ReAct loops, and structured outputs. Build a system that looks up coding rules and assigns NAACCR variable codes (primary site, histology, grade, etc.) from clinical notes.

### 5. ClinicalTrials.gov API
Connect your extracted cancer information to real-world clinical trials using the free ClinicalTrials.gov API v2. Build simple and advanced search functions with filters for demographics, geography, and condition synonyms.

### Intro to LLMs
If you want more background on how LLMs work — tokens, context windows, temperature, tool calling — check out `intro_to_llms.ipynb`.

## Available Models

The following models are available via the OpenAI API (see `config.yaml`):

| Model | Notes |
|-------|-------|
| `gpt-4.1` | High capability, larger context |
| `gpt-4.1-mini` | Good balance of speed and capability |
| `gpt-4o-mini` | Fast and cost-effective |
| `gpt-5` | Latest generation |
| `o1-mini` | Reasoning-focused model |
| `DeepSeek-V3.2` | Open-weight model |
| `mistral-document-ai-2512` | Document-focused model |

Try different models and compare their performance on extraction tasks — you may be surprised by the differences.

## Try Different Tools

Part of the experience is exploring the ecosystem of AI-powered development tools. Consider trying:

**CLI Coding Agents** — AI assistants that work directly in your terminal
- **Codex CLI** — OpenAI's CLI coding agent
- **Claude Code** — Anthropic's CLI coding agent (requires an existing account)

**Chat Interfaces** — For brainstorming, prompt development, and research
- **ChatGPT / Claude** — Web-based chat interfaces
- **goodbot** — JupyterAI chat interface (available in JupyterLab sidebar)

Each tool has different strengths. Experiment and find what works best for your workflow.

## Useful Links

- [ClinicalTrials.gov API v2 Documentation](https://clinicaltrials.gov/data-api/api)
- [ClinicalTrials.gov API Spec (machine-readable)](https://clinicaltrials.gov/api/oas/v2)
- [Query Construction Guide](https://clinicaltrials.gov/find-studies/constructing-complex-search-queries)
- [NAACCR Data Dictionary](https://apps.naaccr.org/data-dictionary/data-dictionary)
- [NCI SEER Site-Specific Coding Rules](https://seer.cancer.gov/manuals/2026/appendixc.html)
- [OpenAI Responses API Documentation](https://platform.openai.com/docs/api-reference/responses)

## License

See [LICENSE](LICENSE) for details.
