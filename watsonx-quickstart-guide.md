# IBM watsonx.governance Quick Start Guide

> A concise step-by-step guide to set up and govern AI models in IBM watsonx.

---

## Navigation Map

```
┌─────────────────────────────────────────────────────────────────────┐
│                        IBM CLOUD                                     │
│  └── watsonx.ai ──┬── Projects ──── Prompts/Models ──── Datasets    │
│                   │                                                  │
│                   └── Deployment Spaces ──── Deployed Models         │
│                                                                      │
│  └── watsonx.governance ──── AI Use Cases ──── AI Factsheets        │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Step 1: Access IBM watsonx

| Action | Path |
|--------|------|
| Login | [cloud.ibm.com](https://cloud.ibm.com) → Sign in |
| Navigate | Resource List → **watsonx** → Launch |
| URL | `https://dataplatform.cloud.ibm.com/wx/home` |

**Maps to:** Entry point for all watsonx services

---

## Step 2: Create a Project

| Action | Path |
|--------|------|
| Navigate | watsonx.ai → **Projects** → New Project |
| Options | Empty project OR From file (upload .zip) |
| Configure | Name, Description, Storage, Region |
| Associate | Manage → Services → Associate **Watson Machine Learning** |

**Maps to:** Container for all assets (prompts, models, data, notebooks)

```
Project
├── Assets (prompts, models, notebooks)
├── Data Assets (CSV, JSON, etc.)
├── Jobs (scheduled runs)
└── Environments (runtimes)
```

---

## Step 3: Add Datasets

| Action | Path |
|--------|------|
| Navigate | Project → **Assets** → **New asset** → Data |
| Upload | Local file OR Connected data source |
| Formats | CSV, JSON, Parquet, Excel |
| Use | Test data, validation data, training data |

**Maps to:** Input for model evaluation and prompt testing

| Dataset Type | Purpose |
|--------------|---------|
| Test Data | Evaluate prompt/model performance |
| Validation Data | Independent validation phase |
| Training Data | Fine-tuning (if applicable) |

---

## Step 4: Create AI Use Case

| Action | Path |
|--------|------|
| Navigate | **AI Governance** → **AI Use Cases** → New |
| Define | Name, Description, Business Owner |
| Add Details | Risk level, Business justification |
| Create Approach | Define solution path |

**Maps to:** Governance container that tracks AI assets through lifecycle

```
AI Use Case
├── Approach 1
│   ├── Model v0.0.1 (Experimental)
│   ├── Model v0.1.0 (Validated)
│   └── Model v1.0.0 (Production)
└── Approach 2 (alternative solution)
```

---

## Step 5: Create Deployment Space

| Action | Path |
|--------|------|
| Navigate | **Deployments** → **New deployment space** |
| Configure | Name, Description, Stage (Dev/Test/Production) |
| Associate | Watson Machine Learning service |
| Set Stage | Development, Pre-production, or Production |

**Maps to:** Environment where models are deployed and served

| Space Type | Purpose |
|------------|---------|
| Development | Testing and experimentation |
| Pre-production | UAT and validation |
| Production | Live serving to users |

---

## Step 6: Link Project to Deployment Space

| Action | Path |
|--------|------|
| From Project | Asset menu → **Promote to space** |
| Select Space | Choose target deployment space |
| Promote | Move asset for deployment |

**Maps to:** Transition from development to deployment

```
Project (Development)
    │
    ▼ Promote
Deployment Space (Staging/Production)
    │
    ▼ Deploy
Live API Endpoint
```

---

## Step 7: Configure Runtime Environment

| Action | Path |
|--------|------|
| Navigate | Project → **Manage** → **Environments** |
| Create | New environment template |
| Configure | Hardware (CPU/GPU), Software, Libraries |
| Associate | Attach to notebooks or jobs |

**Maps to:** Compute resources for running models

| Runtime Type | Use Case |
|--------------|----------|
| Default | Basic inference |
| GPU | Large models, fine-tuning |
| Custom | Special libraries needed |

---

## Step 8: View AI Model Specifications

| Action | Path |
|--------|------|
| Navigate | Project → Asset → **AI Factsheet** tab |
| View | Model card, parameters, lineage |
| Or | AI Governance → AI Use Cases → Select model |

**Maps to:** Complete documentation of model details

| Specification | Description |
|---------------|-------------|
| Model ID | Unique identifier |
| Model Type | Foundation model, fine-tuned, custom |
| Parameters | Temperature, max tokens, etc. |
| Training Data | Data sources used |
| Version | Current version number |
| Lifecycle Stage | Develop, Validate, Operate |

---

## Step 9: Evaluate Model Health

| Action | Path |
|--------|------|
| Navigate | Asset → **Evaluate** |
| Upload | Test data CSV |
| Map | Input columns → Prompt variables |
| Run | Execute evaluation |
| View | Results in AI Factsheet |

**Maps to:** Quality metrics and performance tracking

| Metric Category | Examples |
|-----------------|----------|
| **Quality** | ROUGE, BLEU, Accuracy |
| **Fairness** | Bias scores across groups |
| **Drift** | Performance change over time |
| **Risk** | Toxicity, PII detection |

---

## Complete Workflow Mapping

```
┌────────────────────────────────────────────────────────────────────┐
│  STEP              ACTION                 MAPS TO                  │
├────────────────────────────────────────────────────────────────────┤
│  1. Access         Login to watsonx       Platform Entry           │
│  2. Project        Create container       Development Workspace    │
│  3. Dataset        Upload test data       Evaluation Input         │
│  4. AI Use Case    Define governance      Lifecycle Tracking       │
│  5. Deploy Space   Create environment     Production Target        │
│  6. Link           Promote assets         Dev → Prod Transition    │
│  7. Runtime        Configure compute      Execution Environment    │
│  8. Specifications View model details     Documentation/Audit      │
│  9. Evaluate       Check model health     Quality Assurance        │
└────────────────────────────────────────────────────────────────────┘
```

---

## Quick Reference: Key URLs

| Service | Navigation |
|---------|------------|
| watsonx.ai | dataplatform.cloud.ibm.com/wx/home |
| Projects | Left menu → Projects |
| Deployments | Left menu → Deployments |
| AI Governance | Left menu → AI Governance |
| AI Use Cases | AI Governance → AI use cases |

---

## Summary Cheat Sheet

| I want to... | Go to... |
|--------------|----------|
| Build prompts | Project → Prompt Lab |
| Store data | Project → Assets → Data |
| Track governance | AI Governance → AI Use Cases |
| Deploy model | Deployment Space → Deploy |
| Check performance | Asset → Evaluate |
| View audit trail | Asset → AI Factsheet |
| Monitor production | Deployment → Monitoring |

---
