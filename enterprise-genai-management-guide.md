# Enterprise Gen AI: Code, Config & Model Management
## Short & Crisp Guide

---

## The Three-Layer Architecture

Enterprise Gen AI systems separate three distinct layers for different governance, change frequency, and risk:

| **Layer** | **Contains** | **Changes** | **Risk** | **Owner** |
|-----------|--------------|-------------|----------|-----------|
| **Application Code** | RAG logic, APIs, business rules | Weekly | High | Developers |
| **Configuration** | Prompts, settings, feature flags | Daily | Low | Ops/Business |
| **Model Parameters** | Model weights, training configs | Monthly | Very High | ML Engineers |

---

## Why Separate Them?

**Without Separation:**
- Changing a prompt requires full code deployment
- Can't quickly toggle features
- High risk for small changes
- Slow iteration

**With Separation:**
- Update prompts in minutes, not days
- Toggle features without deployment
- Right governance for each layer
- Fast, safe changes

---

## 1. Application Code Management

### What Goes Here
```
Application Code = Business Logic
├─ RAG orchestration (retrieval + generation)
├─ API endpoints (FastAPI/Flask routes)
├─ Business rules (validation, escalation)
├─ Data processing (chunking, embedding)
└─ Integrations (CRM, databases)
```

### Repository Structure
```
genai-app/
├─ src/
│   ├─ api/routes.py           # API endpoints
│   ├─ rag/pipeline.py         # RAG orchestration
│   ├─ core/config_loader.py   # Loads external config
│   └─ utils/
├─ tests/
│   ├─ unit/
│   └─ integration/
├─ Dockerfile
├─ requirements.txt
└─ .github/workflows/ci-cd.yml
```

### Key Principle: Decouple from Config

```python
# ❌ BAD: Hardcoded settings
def query_rag(question):
    response = openai.chat.completions.create(
        model="gpt-4",              # Hardcoded!
        temperature=0.7,            # Hardcoded!
        system="You are helpful..." # Hardcoded!
    )

# ✅ GOOD: Read from external config
def query_rag(question):
    config = ConfigLoader.load()  # From AWS Parameter Store
    
    response = openai.chat.completions.create(
        model=config.llm.model,
        temperature=config.llm.temperature,
        system=config.prompts.system
    )
```

### Version Control: GitFlow

```
main (production)
├─ Protected branch
├─ Requires 2+ approvals
├─ Auto-deploys after approval
└─ Tagged releases (v1.0.0)

develop (development)
├─ Active development
└─ Auto-deploys to dev environment

feature/* branches
├─ feature/add-reranking
└─ Merged via Pull Request
```

### CI/CD Pipeline

```yaml
On Pull Request:
├─ Run linters (black, flake8)
├─ Run unit tests (coverage > 80%)
├─ Security scan (bandit, safety)
├─ Build Docker image
├─ Integration tests
└─ Require 2 approvals

On Merge to Main:
├─ Deploy to staging
├─ Run smoke tests
├─ Manual approval gate
├─ Deploy to production
└─ Monitor for 24 hours
```

### Governance Gates

| **Gate** | **Automated?** | **Required?** |
|----------|----------------|---------------|
| Linting | ✅ Yes | ✅ Yes |
| Unit tests (80%+) | ✅ Yes | ✅ Yes |
| Security scan | ✅ Yes | ✅ Yes |
| Code review (2+) | ❌ Manual | ✅ Yes |
| Production approval | ❌ Manual | ✅ Yes |

---

## 2. Configuration Management

### What Goes Here
```
Configuration = Settings Without Code Changes
├─ Prompts (system, user templates)
├─ LLM parameters (temperature, max_tokens)
├─ Feature flags (enable/disable features)
├─ Business rules (rate limits, thresholds)
├─ Environment settings (API endpoints)
└─ Secrets references (NOT actual secrets)
```

### Repository Structure
```
genai-configs/
├─ environments/
│   ├─ dev/
│   │   ├─ config.yaml
│   │   └─ prompts.yaml
│   ├─ staging/
│   └─ production/
│
├─ prompts/
│   ├─ system-prompt.txt
│   └─ user-template.txt
│
└─ feature-flags.yaml
```

### Configuration Example

```yaml
# config.yaml (production)

llm:
  model: "gpt-4"
  temperature: 0.7
  max_tokens: 1000

retrieval:
  top_k: 5
  similarity_threshold: 0.75

business_rules:
  max_queries_per_hour: 100
  escalation_keywords:
    - "urgent"
    - "complaint"

features:
  web_search_fallback: false
  cache_enabled: true
  new_reranking: true  # Canary at 10%
```

```yaml
# prompts.yaml

system_prompt: |
  You are a helpful customer support assistant.
  
  Rules:
  - Answer using ONLY the provided context
  - Cite sources using [Source: doc.pdf, page X]
  - If unsure, say "I don't have that information"
  - Never make up information
  
  Context: {context}
  Question: {question}
```

### Where Config Lives

```
Development → Staging → Production

Config stored in:
├─ AWS Parameter Store (preferred)
├─ Azure Key Vault
├─ Kubernetes ConfigMaps
├─ HashiCorp Vault (secrets)
└─ LaunchDarkly (feature flags)

Application reads config at:
├─ Startup (cache for performance)
└─ On-demand (for feature flags)
```

### Config Deployment Flow

```
1. Edit config.yaml in Git
2. Create Pull Request
3. Auto-validation (schema check)
4. Peer review
5. Merge to main
6. Auto-deploy to Parameter Store
7. App reloads config (no restart needed)

Time: 10-30 minutes
```

### Config Governance

| **Gate** | **Automated?** |
|----------|----------------|
| YAML syntax validation | ✅ Yes |
| Schema validation | ✅ Yes |
| No secrets check | ✅ Yes |
| Peer review | ❌ Manual |
| Production approval | ❌ Manual (for major changes) |

---

## 3. Model Parameters Management

### What Goes Here
```
Model Parameters = ML Artifacts
├─ Model weights (model.pkl, model.safetensors)
├─ Training configurations
├─ Fine-tuning datasets
├─ Embedding models
└─ Model metadata (accuracy, bias metrics)
```

### Storage Strategy

```
Model Artifacts NOT in Git (too large)

Stored in:
├─ watsonx.ai Model Registry
├─ MLflow Model Registry
├─ AWS S3 (versioned buckets)
├─ Azure ML Model Registry
└─ HuggingFace Hub (private)

Metadata in Git:
└─ models/
    └─ model-v1.2.3.yaml  # Pointer to S3/watsonx
```

### Model Versioning

```yaml
# models/rag-model-v1.2.3.yaml

model:
  name: "customer-support-rag"
  version: "1.2.3"
  created: "2026-01-15"
  
  artifacts:
    location: "s3://models/rag-v1.2.3/"
    format: "safetensors"
    size_gb: 14.5
  
  performance:
    accuracy: 0.92
    latency_p95_ms: 450
    bias_score: 0.03  # Low is good
  
  training:
    dataset: "support-tickets-2024-q4"
    framework: "transformers"
    base_model: "llama-3-8b"
    fine_tuning_steps: 5000
  
  deployment:
    watsonx_space: "production-us-east"
    endpoint: "https://api.watsonx.ai/v1/rag-v123"
    hardware: "gpu-a100"
```

### Model Deployment Flow

```
1. Train model → Save to S3
2. Validate model (accuracy, bias)
3. Model Risk review ✅
4. Security scan ✅
5. Register in watsonx Model Registry
6. Deploy to dev-deployment-space
7. A/B test (10% traffic)
8. Monitor for 1 week
9. Full rollout (100% traffic)

Time: 2-4 weeks
```

### Model Governance (Strictest)

| **Gate** | **Required?** |
|----------|---------------|
| Model validation | ✅ Yes |
| Bias testing | ✅ Yes |
| Security scan | ✅ Yes |
| Model Risk approval | ✅ Yes |
| Performance benchmarks | ✅ Yes |
| A/B testing | ✅ Yes (production) |
| Executive approval | ✅ Yes (high-risk models) |

---

## Integration: How They Work Together

### Runtime Flow

```
1. Application Code (weekly deploys)
   └─ Loads config from AWS Parameter Store
   └─ Calls model API from watsonx

2. Configuration (daily updates)
   └─ Changed via GitOps
   └─ App reloads without restart

3. Model Parameters (monthly updates)
   └─ New model deployed to watsonx
   └─ Config updated to point to new endpoint
   └─ Gradual rollout via feature flags
```

### Example: Updating System Prompt

```
Traditional Way (everything coupled):
├─ Edit code
├─ Code review
├─ Testing
├─ Deploy application
└─ Time: 3-5 days

Modern Way (separated config):
├─ Edit prompts.yaml
├─ Peer review
├─ Auto-deploy to Parameter Store
├─ App reloads config
└─ Time: 30 minutes
```

---

## Best Practices Summary

### Application Code
- ✅ Version control in Git
- ✅ Strict CI/CD with tests
- ✅ Code review required
- ✅ Deploy weekly or less
- ✅ Never hardcode settings

### Configuration
- ✅ Separate Git repository
- ✅ Store in Parameter Store/Key Vault
- ✅ Lightweight approval process
- ✅ Deploy daily as needed
- ✅ Schema validation automated

### Model Parameters
- ✅ Store in Model Registry (not Git)
- ✅ Strict governance gates
- ✅ A/B testing required
- ✅ Deploy monthly or less
- ✅ Full validation pipeline

---

## Enterprise Reality Check

**Small Company (< 100 employees):**
- Application code in Git ✅
- Config in .env files (not ideal but works)
- Models from OpenAI API (no management needed)

**Medium Company (100-1000 employees):**
- Application code in Git with CI/CD ✅
- Config in AWS Parameter Store ✅
- Models in MLflow or watsonx ✅
- Basic governance

**Large Enterprise (1000+ employees):**
- Application code with strict GitOps ✅
- Config as Code with GitOps ✅
- Models in enterprise registry ✅
- Full governance, multiple approval gates ✅
- Separate teams for each layer

---

## Quick Reference

| **Need to...** | **Change Layer** | **Time** |
|---------------|------------------|----------|
| Fix a bug | Application Code | 2-5 days |
| Update prompt | Configuration | 30 min - 2 hours |
| Tune temperature | Configuration | 30 min - 2 hours |
| Toggle feature | Configuration | Instant |
| Deploy new model | Model Parameters | 2-4 weeks |
| A/B test prompt | Configuration + Feature Flag | 1-2 hours |

---

**Bottom Line:** Separate application code, configuration, and model parameters. Each has different change velocity and risk. Use the right tool and governance for each layer.
