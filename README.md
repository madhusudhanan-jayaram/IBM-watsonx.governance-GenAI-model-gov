# IBM watsonx.governance - GenAI Model Governance

> A hands-on project for evaluating and governing Generative AI models throughout their lifecycle using IBM watsonx.governance.

---

## Project Overview

This project demonstrates how to govern GenAI/LLM applications using IBM watsonx.governance. It uses an **auto insurance claim processing** use case where AI helps insurance agents by:

- **Summarizing** lengthy claim narratives
- **Extracting** key information (car details, location, dates)
- **Suggesting** next steps for claim processing

---

## Project Structure

```
IBM-watsonx.governance-GenAI-model-gov/
│
├── Auto-claim-summary.zip              # Packaged project for watsonx.ai import
│
├── assets/
│   ├── data_asset/                     # Test & validation datasets
│   │   ├── Insurance claim summarization test data.csv
│   │   ├── Insurance claim summarization validation data.csv
│   │   ├── insurance claim fact extraction test data.csv
│   │   └── insurance claim suggested next steps test data.csv
│   │
│   ├── wx_prompt/                      # Prompt templates
│   │   ├── Insuranceclaimsummarization.json
│   │   ├── Insuranceclaimkeyinformationextraction.json
│   │   └── Insuranceclaimsuggestednextsteps.json
│   │
│   └── .METADATA/                      # Asset metadata (auto-generated)
│
├── assettypes/                         # Schema definitions
│   ├── model_entry_user.json
│   ├── modelfacts_user.json
│   └── wx_prompt.json
│
└── documents/                          # Comprehensive guides
    ├── watsonx-quickstart-guide.md
    ├── IBM-watsonx-governance-teams-guide.md
    ├── enterprise-genai-deployment-workflow.md
    ├── enterprise-genai-management-guide.md
    ├── genai-application-stack-governance.md
    ├── governance-scope-guide.md
    └── ai-factsheet-interpretation-guide.md
```

---

## Key Components

### Prompt Templates

| Prompt | Type | Description |
|--------|------|-------------|
| **Claim Summarization** | Zero-shot | Generates 3-sentence summaries of insurance claims |
| **Key Information Extraction** | Few-shot (6 examples) | Extracts car make/model, location, date, time |
| **Suggested Next Steps** | Few-shot (5 examples) | Recommends actions for claim processing |

### Datasets

| Dataset | Records | Purpose |
|---------|---------|---------|
| Summarization Test Data | 10 | Evaluate summarization prompt |
| Summarization Validation Data | 10 | Independent validation |
| Fact Extraction Test Data | 15 | Evaluate extraction accuracy |
| Next Steps Test Data | 18 | Evaluate recommendations |

### Model Configuration

| Setting | Value |
|---------|-------|
| Model | `google/flan-ul2` |
| Decoding | Greedy |
| Max Tokens | 200 |
| Temperature | 0.7 |

---

## Getting Started

### Import into watsonx.ai

1. Go to **watsonx.ai** → **Projects** → **Create project from file**
2. Upload `Auto-claim-summary.zip`
3. Associate your **Watson Machine Learning** service

### Run Evaluations

1. Open a prompt template
2. Click **Evaluate**
3. Upload the corresponding test data CSV
4. Map input/output columns
5. Run evaluation

### Track in AI Use Case

1. View evaluation results in **AI Factsheet**
2. Click **Track in AI use case**
3. Create or select an AI use case
4. Assign version and lifecycle stage

---

## Governance Workflow

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   DEVELOP   │ →  │  VALIDATE   │ →  │   DEPLOY    │
├─────────────┤    ├─────────────┤    ├─────────────┤
│ • Build     │    │ • Test      │    │ • Promote   │
│ • Evaluate  │    │ • Review    │    │ • Monitor   │
│ • Track     │    │ • Approve   │    │ • Govern    │
└─────────────┘    └─────────────┘    └─────────────┘
```

---

## Documentation

### Quick Start & Basics

| Document | Description |
|----------|-------------|
| [Quick Start Guide](documents/watsonx-quickstart-guide.md) | Step-by-step setup and navigation guide |
| [Teams Guide](documents/IBM-watsonx-governance-teams-guide.md) | Roles and collaboration in watsonx.governance |

### Enterprise Guides

| Document | Description |
|----------|-------------|
| [Enterprise Deployment Workflow](documents/enterprise-genai-deployment-workflow.md) | Detailed deployment process for enterprises |
| [Enterprise Management Guide](documents/enterprise-genai-management-guide.md) | GenAI management best practices |

### Governance Deep Dives

| Document | Description |
|----------|-------------|
| [Governance Scope Guide](documents/governance-scope-guide.md) | What to govern and when |
| [GenAI Application Stack Governance](documents/genai-application-stack-governance.md) | Governing the full AI stack |
| [AI Factsheet Interpretation Guide](documents/ai-factsheet-interpretation-guide.md) | Understanding evaluation metrics and factsheets |

### External Resources

| Resource | Description |
|----------|-------------|
| [IBM Official Guide (PDF)](https://watsonx.governance-samples.s3.us.cloud-object-storage.appdomain.cloud/watsonx.governance%20first%20steps.pdf) | Official IBM step-by-step guide |

---

## Steps

```
  Setup          →        Build          →        Deploy         →        Monitor
┌─────────┐         ┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│  1 - 3  │         │    4 - 7    │         │    8 - 9    │         │   10 - 11   │
└─────────┘         └─────────────┘         └─────────────┘         └─────────────┘
```

| Step | Action | Phase |
|:----:|--------|:-----:|
| 1 | **Set up an AI Use Case** | Setup |
| 2 | **Create a new Deployment Space** | Setup |
| 3 | **Create a Project** | Setup |
| 4 | **Understand the Dataset** | Build |
| 5 | **Link Workspace** (Development Phase) | Build |
| 6 | **Link Workspace** (Validation Phase) | Build |
| 7 | **View all Assets in the Project** | Build |
| 8 | **View AI Model Specifications** | Deploy |
| 9 | **Promote the Model to the Next Space** | Deploy |
| 10 | **Evaluate the AI Model** | Monitor |
| 11 | **Evaluate Model Health** | Monitor |

---

## License

This project is part of IBM watsonx.governance samples.