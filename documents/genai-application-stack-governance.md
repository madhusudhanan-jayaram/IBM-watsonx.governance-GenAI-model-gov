# Building and Governing GenAI Applications with IBM watsonx

> A guide to stacking GenAI application components and evaluating them through IBM watsonx.governance.

---

## GenAI Application Stack Overview

A production GenAI application consists of multiple layers that work together to deliver AI-powered functionality to end users.

```
┌─────────────────────────────────────────────────────────────────┐
│                      USER INTERFACE                              │
│                  (Web App, Mobile, API)                          │
├─────────────────────────────────────────────────────────────────┤
│                    APPLICATION LAYER                             │
│              (Business Logic, Orchestration)                     │
├─────────────────────────────────────────────────────────────────┤
│                      AI/ML LAYER                                 │
│         (Prompts, Models, RAG, Agents, Guardrails)              │
├─────────────────────────────────────────────────────────────────┤
│                      DATA LAYER                                  │
│          (Vector DB, Knowledge Base, Documents)                  │
├─────────────────────────────────────────────────────────────────┤
│                   INFRASTRUCTURE LAYER                           │
│              (Cloud, Containers, Networking)                     │
└─────────────────────────────────────────────────────────────────┘
```

---

## Layer 1: User Interface

| Component | Description |
|-----------|-------------|
| Web Application | React, Angular, or Vue frontend |
| Chat Interface | Conversational UI for user queries |
| API Gateway | REST/GraphQL endpoints for integration |
| Mobile App | iOS/Android applications |

**Governance Consideration:** Track user interactions, consent, and feedback

---

## Layer 2: Application Layer

| Component | Description |
|-----------|-------------|
| Orchestrator | LangChain, LlamaIndex, or custom logic |
| Session Manager | Conversation history and context |
| Authentication | User identity and access control |
| Rate Limiter | Usage controls and throttling |

**Governance Consideration:** Log all requests, enforce access policies

---

## Layer 3: AI/ML Layer

| Component | Description |
|-----------|-------------|
| Foundation Model | LLM (watsonx, OpenAI, Claude, Llama) |
| Prompt Templates | Structured prompts with variables |
| RAG Pipeline | Retrieval-Augmented Generation |
| AI Agents | Multi-step reasoning and tool use |
| Guardrails | Content filtering and safety checks |

**Governance Consideration:** This is the primary focus of watsonx.governance

---

## Layer 4: Data Layer

| Component | Description |
|-----------|-------------|
| Vector Database | Pinecone, Milvus, Weaviate |
| Document Store | S3, Cloud Object Storage |
| Knowledge Base | Curated enterprise knowledge |
| Embeddings | Text-to-vector conversion |

**Governance Consideration:** Data lineage, PII handling, access controls

---

## Layer 5: Infrastructure Layer

| Component | Description |
|-----------|-------------|
| Cloud Platform | IBM Cloud, AWS, Azure, GCP |
| Containers | Docker, Kubernetes |
| Monitoring | Logging, metrics, alerting |
| Security | Encryption, network policies |

**Governance Consideration:** Deployment spaces, runtime environments

---

## Stacking a GenAI Application

### Step-by-Step Build Process

```
1. DEFINE USE CASE
   │
   ▼
2. SELECT FOUNDATION MODEL
   │
   ▼
3. DESIGN PROMPTS
   │
   ▼
4. BUILD DATA PIPELINE (if RAG)
   │
   ▼
5. DEVELOP APPLICATION LOGIC
   │
   ▼
6. ADD GUARDRAILS
   │
   ▼
7. CREATE USER INTERFACE
   │
   ▼
8. DEPLOY & MONITOR
```

### Example Stack for Insurance Claim Processing

```
┌────────────────────────────────────────────────────┐
│  UI: Agent Dashboard (React)                       │
├────────────────────────────────────────────────────┤
│  APP: Flask API + LangChain Orchestrator           │
├────────────────────────────────────────────────────┤
│  AI: watsonx.ai                                    │
│  ├── Summarization Prompt (google/flan-ul2)        │
│  ├── Extraction Prompt (google/flan-ul2)           │
│  └── Next Steps Prompt (google/flan-ul2)           │
├────────────────────────────────────────────────────┤
│  DATA: Claims Database + Policy Documents          │
├────────────────────────────────────────────────────┤
│  INFRA: IBM Cloud + Kubernetes                     │
└────────────────────────────────────────────────────┘
```

---

## Evaluating with IBM watsonx.governance

### Governance Evaluation Framework

IBM watsonx.governance evaluates GenAI applications across five dimensions:

```
                    ┌─────────────┐
                    │   QUALITY   │
                    └──────┬──────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
┌───────▼───────┐  ┌───────▼───────┐  ┌───────▼───────┐
│   FAIRNESS    │  │     RISK      │  │  COMPLIANCE   │
└───────────────┘  └───────────────┘  └───────────────┘
                           │
                    ┌──────▼──────┐
                    │    DRIFT    │
                    └─────────────┘
```

---

### Dimension 1: Quality Evaluation

| Metric | What It Measures | Target |
|--------|------------------|--------|
| ROUGE | Overlap with reference text | > 0.7 |
| BLEU | N-gram precision | > 0.6 |
| Accuracy | Correct responses | > 90% |
| Latency | Response time | < 2 sec |

**How to Evaluate:**
1. Project → Prompt Template → **Evaluate**
2. Upload test data with reference outputs
3. Map columns and run evaluation
4. View results in AI Factsheet

---

### Dimension 2: Fairness Evaluation

| Metric | What It Measures | Target |
|--------|------------------|--------|
| Demographic Parity | Equal outcomes across groups | < 10% variance |
| Equalized Odds | Equal error rates across groups | < 5% variance |
| Disparate Impact | Ratio of positive outcomes | 0.8 - 1.2 |

**How to Evaluate:**
1. Prepare test data with demographic attributes
2. Run model on all demographic groups
3. Compare outcomes across groups
4. Document findings in AI Factsheet

---

### Dimension 3: Risk Evaluation

| Risk Type | What to Check | Mitigation |
|-----------|---------------|------------|
| Toxicity | Harmful content generation | Content filters |
| Hallucination | Factual inaccuracies | RAG with citations |
| PII Leakage | Personal data exposure | Data masking |
| Jailbreak | Prompt manipulation | Input validation |
| Bias | Unfair treatment | Debiasing techniques |

**How to Evaluate:**
1. Run adversarial tests
2. Check guardrail effectiveness
3. Document risk scores
4. Set up continuous monitoring

---

### Dimension 4: Compliance Evaluation

| Regulation | Requirements | Evidence |
|------------|--------------|----------|
| GDPR | Data privacy, consent | Privacy impact assessment |
| CCPA | Consumer rights | Data handling documentation |
| EU AI Act | Risk classification | AI use case documentation |
| Industry | Sector-specific rules | Compliance certificates |

**How to Evaluate:**
1. Complete AI Use Case documentation
2. Attach compliance evidence
3. Get legal/compliance sign-off
4. Record approvals in AI Factsheet

---

### Dimension 5: Drift Evaluation

| Drift Type | What Changes | Detection Method |
|------------|--------------|------------------|
| Data Drift | Input distribution shifts | Statistical tests |
| Concept Drift | Relationship changes | Performance monitoring |
| Model Drift | Output quality degrades | Continuous evaluation |

**How to Evaluate:**
1. Set up production monitoring
2. Compare current vs. baseline metrics
3. Alert on threshold breaches
4. Trigger re-evaluation when needed

---

## Governance Evaluation Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVELOPMENT PHASE                             │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐      │
│  │ Build   │ →  │ Test    │ →  │ Evaluate│ →  │ Document│      │
│  │ Prompt  │    │ Locally │    │ Quality │    │ in      │      │
│  │         │    │         │    │         │    │Factsheet│      │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    VALIDATION PHASE                              │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐      │
│  │ Risk    │ →  │ Fairness│ →  │Compliance│→  │ Approval│      │
│  │ Review  │    │ Testing │    │ Check   │    │         │      │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    PRODUCTION PHASE                              │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐      │
│  │ Deploy  │ →  │ Monitor │ →  │ Detect  │ →  │ Update  │      │
│  │         │    │ Metrics │    │ Drift   │    │ Model   │      │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘      │
└─────────────────────────────────────────────────────────────────┘
```

---

## Mapping Stack Components to Governance

| Stack Layer | Governance Focus | watsonx.governance Feature |
|-------------|------------------|---------------------------|
| User Interface | Usage tracking | Monitoring dashboards |
| Application | Request logging | Audit trails |
| AI/ML | Model quality | Evaluations, AI Factsheets |
| Data | Data lineage | Data governance integration |
| Infrastructure | Deployment tracking | Deployment spaces |

---

## Evaluation Checklist

### Before Development
- [ ] Define AI Use Case in watsonx.governance
- [ ] Document business objectives
- [ ] Identify risk level and stakeholders

### During Development
- [ ] Create prompt templates in watsonx.ai
- [ ] Prepare test and validation datasets
- [ ] Run initial quality evaluations
- [ ] Document in AI Factsheet

### Before Deployment
- [ ] Complete risk evaluation
- [ ] Pass fairness testing
- [ ] Obtain compliance approval
- [ ] Get stakeholder sign-off

### After Deployment
- [ ] Set up monitoring
- [ ] Configure drift detection
- [ ] Establish review cadence
- [ ] Plan for model updates

---

## Summary

| Phase | Stack Focus | Governance Focus |
|-------|-------------|------------------|
| **Build** | Prompts, Models, Data | Quality evaluation |
| **Validate** | Integration, Testing | Risk & Compliance |
| **Deploy** | Infrastructure, APIs | Deployment tracking |
| **Monitor** | Performance, Logs | Drift detection |

Building a GenAI application requires stacking multiple components, but governance must be embedded at every layer. IBM watsonx.governance provides the tools to evaluate, track, and monitor AI applications throughout their lifecycle—ensuring they remain accurate, fair, safe, and compliant.

---

**Document Version:** 1.0
**Last Updated:** 2026-02-01
