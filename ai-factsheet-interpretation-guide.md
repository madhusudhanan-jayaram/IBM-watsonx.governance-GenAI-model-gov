# Interpreting AI Factsheets & Deploying to Production

> A concise guide to reading AI Factsheets and deploying models in IBM watsonx.

---

## What is an AI Factsheet?

An AI Factsheet is the **single source of truth** for an AI asset. It tracks everything about your model or prompt throughout its lifecycle.

```
┌─────────────────────────────────────────────────────────────────┐
│                      AI FACTSHEET                                │
│  ─────────────────────────────────────────────────────────────── │
│  "The passport of your AI model"                                 │
│                                                                  │
│  • Who created it                                                │
│  • What it does                                                  │
│  • How it performs                                               │
│  • Where it's deployed                                           │
│  • Why decisions were made                                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## AI Factsheet Sections

### 1. Model Overview

| Field | What It Tells You |
|-------|-------------------|
| **Name** | Asset identifier |
| **Type** | Prompt template, model, pipeline |
| **Created** | When and by whom |
| **Version** | Current version number |
| **Status** | Development, Validation, Production |

**What to check:** Is this the correct version? Is the status current?

---

### 2. Model Details

| Field | What It Tells You |
|-------|-------------------|
| **Model ID** | Foundation model used (e.g., `google/flan-ul2`) |
| **Parameters** | Temperature, max tokens, decoding method |
| **Task Type** | Summarization, classification, extraction, etc. |
| **Prompt Structure** | Instruction, examples, input/output format |

**What to check:** Are parameters appropriate for the use case?

---

### 3. Evaluation Results

```
┌─────────────────────────────────────────────────────────────────┐
│  EVALUATION RESULTS                                              │
│  ─────────────────                                               │
│                                                                  │
│  Quality Metrics          │  Status                              │
│  ─────────────────────────┼────────────────                      │
│  ROUGE-1: 0.78            │  ✅ Pass (> 0.7)                     │
│  ROUGE-2: 0.65            │  ✅ Pass (> 0.5)                     │
│  BLEU: 0.72               │  ✅ Pass (> 0.6)                     │
│  Accuracy: 94%            │  ✅ Pass (> 90%)                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

| Metric | Good Score | What It Measures |
|--------|------------|------------------|
| **ROUGE-1** | > 0.7 | Word overlap with reference |
| **ROUGE-2** | > 0.5 | Phrase overlap with reference |
| **BLEU** | > 0.6 | N-gram precision |
| **Accuracy** | > 90% | Correct responses |

**What to check:**
- Are scores above thresholds?
- How many test samples were used?
- When was the last evaluation?

---

### 4. Lifecycle Tracking

```
DEVELOP ──────► VALIDATE ──────► OPERATE
   │                │                │
   ▼                ▼                ▼
┌──────┐       ┌──────┐        ┌──────┐
│ v0.1 │  ──►  │ v0.2 │  ──►   │ v1.0 │
└──────┘       └──────┘        └──────┘
```

| Stage | Meaning |
|-------|---------|
| **Develop** | Building and testing |
| **Validate** | Independent review and approval |
| **Operate** | Live in production |

**What to check:** Has it passed all required stages?

---

### 5. Governance Information

| Field | What It Tells You |
|-------|-------------------|
| **AI Use Case** | Business context and objectives |
| **Approach** | Solution path within use case |
| **Risk Level** | Low, Medium, High |
| **Approvals** | Who approved and when |
| **Compliance** | Regulatory status |

**What to check:** Are all required approvals in place?

---

### 6. Deployment Information

| Field | What It Tells You |
|-------|-------------------|
| **Deployment Space** | Where it's deployed |
| **Endpoint URL** | API access point |
| **Hardware** | CPU, memory, GPU allocation |
| **Scaling** | Min/max instances |
| **Status** | Active, Inactive, Failed |

**What to check:** Is the deployment healthy and accessible?

---

## Quick Interpretation Checklist

```
□ Version matches expected?
□ Evaluation scores pass thresholds?
□ Evaluation is recent (not stale)?
□ Lifecycle stage is correct?
□ Required approvals obtained?
□ Deployment status is active?
□ No critical alerts or warnings?
```

---

## Deployment to Production Flow

### Complete Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  STEP 1: DEVELOP                                                 │
│  ──────────────────────────────────────────────────────────────  │
│  Project → Create Prompt → Test → Evaluate → Document            │
│                                        │                         │
│                                        ▼                         │
│                              ┌─────────────────┐                 │
│                              │  AI Factsheet   │                 │
│                              │  (v0.0.1)       │                 │
│                              └────────┬────────┘                 │
│                                       │                          │
└───────────────────────────────────────┼──────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  STEP 2: TRACK IN AI USE CASE                                   │
│  ──────────────────────────────────────────────────────────────  │
│  AI Factsheet → Track in AI Use Case → Assign Version            │
│                                        │                         │
│                                        ▼                         │
│                              ┌─────────────────┐                 │
│                              │  Governance     │                 │
│                              │  Tracking       │                 │
│                              └────────┬────────┘                 │
│                                       │                          │
└───────────────────────────────────────┼──────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  STEP 3: VALIDATE                                                │
│  ──────────────────────────────────────────────────────────────  │
│  Export Project → Import to Validation Project → Re-evaluate     │
│                                        │                         │
│                                        ▼                         │
│                              ┌─────────────────┐                 │
│                              │  Validation     │                 │
│                              │  Results        │                 │
│                              └────────┬────────┘                 │
│                                       │                          │
└───────────────────────────────────────┼──────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  STEP 4: PROMOTE TO DEPLOYMENT SPACE                            │
│  ──────────────────────────────────────────────────────────────  │
│  Asset Menu → Promote to Space → Select Production Space         │
│                                        │                         │
│                                        ▼                         │
│                              ┌─────────────────┐                 │
│                              │  Asset in       │                 │
│                              │  Deploy Space   │                 │
│                              └────────┬────────┘                 │
│                                       │                          │
└───────────────────────────────────────┼──────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  STEP 5: DEPLOY                                                  │
│  ──────────────────────────────────────────────────────────────  │
│  Deployment Space → Asset → Deploy → Configure → Create          │
│                                        │                         │
│                                        ▼                         │
│                              ┌─────────────────┐                 │
│                              │  Live API       │                 │
│                              │  Endpoint       │                 │
│                              └────────┬────────┘                 │
│                                       │                          │
└───────────────────────────────────────┼──────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  STEP 6: MONITOR                                                 │
│  ──────────────────────────────────────────────────────────────  │
│  Enable Monitoring → Set Alerts → Track Metrics                  │
│                                        │                         │
│                                        ▼                         │
│                              ┌─────────────────┐                 │
│                              │  Production     │                 │
│                              │  Monitoring     │                 │
│                              └─────────────────┘                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Step-by-Step Deployment

### Step 1: Prepare for Deployment

| Action | Path |
|--------|------|
| Verify evaluation passed | AI Factsheet → Evaluations |
| Check approvals | AI Factsheet → Governance |
| Confirm version | AI Factsheet → Overview |

---

### Step 2: Promote to Deployment Space

| Action | Path |
|--------|------|
| Open asset | Project → Assets → Select prompt/model |
| Promote | Asset menu (⋮) → **Promote to space** |
| Select space | Choose production deployment space |
| Confirm | Click **Promote** |

---

### Step 3: Deploy the Asset

| Action | Path |
|--------|------|
| Open space | Deployments → Your deployment space |
| Find asset | Assets tab → Select your asset |
| Deploy | Asset menu (⋮) → **Deploy** |
| Configure | Set name, hardware, scaling |
| Create | Click **Create** |

---

### Step 4: Configure Deployment

| Setting | Recommendation |
|---------|----------------|
| **Name** | Descriptive (e.g., `claim-summary-prod-v1`) |
| **Hardware** | Match expected load |
| **Scaling** | Min: 2, Max: 5 (for production) |
| **Timeout** | 60 seconds default |

---

### Step 5: Get Endpoint Details

| Information | Where to Find |
|-------------|---------------|
| Endpoint URL | Deployment → API Reference |
| API Key | Deployment → Credentials |
| Code Snippets | Deployment → Code samples |

---

### Step 6: Enable Monitoring

| Action | Path |
|--------|------|
| Open deployment | Deployments → Select deployment |
| Enable monitoring | Settings → Monitoring → Enable |
| Set alerts | Configure thresholds |

---

## Production Deployment Checklist

```
PRE-DEPLOYMENT
□ Evaluation scores meet thresholds
□ AI Use Case documented
□ All approvals obtained
□ Rollback plan ready

DEPLOYMENT
□ Promoted to production space
□ Deployment created successfully
□ Endpoint responding
□ Authentication configured

POST-DEPLOYMENT
□ Monitoring enabled
□ Alerts configured
□ Documentation updated
□ Team notified
```

---

## Reading Deployment Status

| Status | Meaning | Action |
|--------|---------|--------|
| **Initializing** | Starting up | Wait |
| **Ready** | Active and serving | None |
| **Updating** | Configuration change | Wait |
| **Failed** | Deployment error | Check logs |
| **Inactive** | Manually stopped | Restart if needed |

---

## Common Issues & Solutions

| Issue | Check | Solution |
|-------|-------|----------|
| Deployment failed | Logs | Fix configuration errors |
| High latency | Metrics | Scale up instances |
| Low accuracy | Evaluation | Retrain or adjust prompt |
| Errors increasing | Monitoring | Check input data quality |

---

## Summary

### AI Factsheet Key Sections

| Section | Key Questions |
|---------|---------------|
| Overview | Right version? Correct status? |
| Evaluation | Scores passing? Recent? |
| Lifecycle | All stages completed? |
| Governance | Approvals in place? |
| Deployment | Active and healthy? |

### Deployment Flow

```
Develop → Track → Validate → Promote → Deploy → Monitor
```

---

**Document Version:** 1.0
**Last Updated:** 2026-02-01
