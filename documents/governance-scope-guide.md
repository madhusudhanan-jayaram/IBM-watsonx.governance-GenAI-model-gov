# Governance Scope: Entire Model vs. Specific Assets

> Understanding what to govern and when in your GenAI applications.

---

## The Short Answer

**You don't need to govern everything equally.** Apply governance based on risk, business impact, and regulatory requirements. Focus on the assets that matter most.

```
┌─────────────────────────────────────────────────────────────────┐
│                    GOVERNANCE SCOPE                              │
│                                                                  │
│   Full Governance ◄────────────────────────► Minimal Governance │
│                                                                  │
│   • Customer-facing        • Internal tools                     │
│   • High-risk decisions    • Experimental work                  │
│   • Regulated industries   • Low-impact outputs                 │
│   • Production systems     • Development/sandbox                │
└─────────────────────────────────────────────────────────────────┘
```

---

## What Can Be Governed?

In IBM watsonx.governance, you can govern different types of assets:

| Asset Type | Description | Governance Level |
|------------|-------------|------------------|
| **Foundation Model** | Base LLM (e.g., Llama, Granite) | Usually pre-governed by provider |
| **Fine-tuned Model** | Customized model on your data | Full governance required |
| **Prompt Template** | Structured prompts with instructions | Asset-level governance |
| **RAG Pipeline** | Retrieval + generation system | Component-level governance |
| **AI Agent** | Multi-step reasoning system | Full governance required |
| **Dataset** | Training/test/validation data | Data governance |

---

## Governance by Asset Type

### 1. Foundation Models (Pre-trained LLMs)

```
┌─────────────────────────────────────────┐
│  Foundation Model (e.g., google/flan-ul2) │
│  ─────────────────────────────────────── │
│  Governance: MINIMAL (provider handles)  │
│  Your focus: How you USE it              │
└─────────────────────────────────────────┘
```

**What the provider governs:**
- Model training and safety
- Bias mitigation at base level
- Security and infrastructure

**What YOU govern:**
- How you prompt the model
- What data you send to it
- How you use the outputs

---

### 2. Prompt Templates

```
┌─────────────────────────────────────────┐
│  Prompt Template                         │
│  ─────────────────────────────────────── │
│  Governance: ASSET-LEVEL                 │
│  Each prompt can be governed separately  │
└─────────────────────────────────────────┘
```

**Govern each prompt based on:**

| Prompt Use Case | Risk Level | Governance Needed |
|-----------------|------------|-------------------|
| Customer service responses | High | Full evaluation, monitoring |
| Internal document summary | Medium | Basic evaluation |
| Developer debugging helper | Low | Minimal |

**Example from this project:**

| Prompt | Risk | Governance |
|--------|------|------------|
| Claim Summarization | Medium | Evaluate quality |
| Key Info Extraction | Medium | Evaluate accuracy |
| Next Steps Suggestion | High | Full governance (affects decisions) |

---

### 3. Fine-tuned Models

```
┌─────────────────────────────────────────┐
│  Fine-tuned Model                        │
│  ─────────────────────────────────────── │
│  Governance: FULL                        │
│  You modified it = you own it            │
└─────────────────────────────────────────┘
```

**Required governance:**
- Training data documentation
- Bias and fairness testing
- Performance evaluation
- Drift monitoring
- Full AI Factsheet

---

### 4. RAG Systems

```
┌─────────────────────────────────────────┐
│  RAG Pipeline                            │
│  ─────────────────────────────────────── │
│  Governance: COMPONENT-LEVEL             │
│  Govern each piece separately            │
└─────────────────────────────────────────┘
```

| Component | What to Govern |
|-----------|----------------|
| Knowledge Base | Data quality, freshness, accuracy |
| Embeddings Model | Retrieval quality |
| Retrieval Logic | Relevance, coverage |
| Generation Prompt | Output quality, safety |
| Final Output | Hallucination, accuracy |

---

## Decision Framework: What to Govern?

### Step 1: Assess Risk Level

| Question | High Risk | Low Risk |
|----------|-----------|----------|
| Who uses it? | External customers | Internal team only |
| What decisions? | Financial, medical, legal | Informational only |
| Data sensitivity? | PII, confidential | Public information |
| Regulatory? | Regulated industry | No regulations |
| Reversibility? | Hard to undo | Easy to correct |

---

### Step 2: Apply Governance Tier

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  TIER 1: FULL GOVERNANCE                                        │
│  ────────────────────────                                        │
│  • Customer-facing AI                                            │
│  • Decision-making systems                                       │
│  • Regulated use cases                                           │
│  • High-value transactions                                       │
│                                                                  │
│  Requirements:                                                   │
│  ✓ AI Use Case documentation                                    │
│  ✓ Full evaluation (quality, fairness, risk)                    │
│  ✓ Compliance review                                             │
│  ✓ Stakeholder approvals                                         │
│  ✓ Production monitoring                                         │
│  ✓ Drift detection                                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  TIER 2: STANDARD GOVERNANCE                                    │
│  ───────────────────────────                                     │
│  • Internal tools                                                │
│  • Employee-facing AI                                            │
│  • Medium-risk applications                                      │
│                                                                  │
│  Requirements:                                                   │
│  ✓ Basic AI Use Case                                             │
│  ✓ Quality evaluation                                            │
│  ✓ Basic monitoring                                              │
│  ○ Compliance review (optional)                                  │
│  ○ Formal approvals (optional)                                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  TIER 3: MINIMAL GOVERNANCE                                     │
│  ──────────────────────────                                      │
│  • Experimentation                                               │
│  • Development/sandbox                                           │
│  • Low-impact use cases                                          │
│                                                                  │
│  Requirements:                                                   │
│  ✓ Basic documentation                                           │
│  ○ Evaluation (optional)                                         │
│  ○ Monitoring (optional)                                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Practical Examples

### Example 1: Insurance Claim Processing (This Project)

| Asset | User | Risk | Governance Tier |
|-------|------|------|-----------------|
| Summarization Prompt | Agent (internal) | Medium | Tier 2 |
| Extraction Prompt | Agent (internal) | Medium | Tier 2 |
| Next Steps Prompt | Agent → Customer impact | High | Tier 1 |
| Test Dataset | Developers | Low | Tier 3 |

**Recommendation:** Focus governance on the "Next Steps" prompt since it influences agent actions that affect customers.

---

### Example 2: Customer Service Chatbot

| Asset | User | Risk | Governance Tier |
|-------|------|------|-----------------|
| Main Chat Prompt | Customers | High | Tier 1 |
| FAQ Retrieval | Customers | Medium | Tier 2 |
| Escalation Logic | Internal routing | Low | Tier 3 |
| Knowledge Base | Source data | Medium | Tier 2 |

**Recommendation:** Full governance on the main chat prompt; standard governance on knowledge base quality.

---

### Example 3: Internal Code Assistant

| Asset | User | Risk | Governance Tier |
|-------|------|------|-----------------|
| Code Generation Prompt | Developers | Low | Tier 3 |
| Code Review Prompt | Developers | Low | Tier 3 |
| Security Scan Prompt | Security team | Medium | Tier 2 |

**Recommendation:** Minimal governance since it's internal and outputs are reviewed by developers.

---

## Governance Scope in watsonx.governance

### Asset-Level Tracking

```
AI Use Case: Insurance Claims Processing
│
├── Approach: LLM-based Processing
│   │
│   ├── Asset: Summarization Prompt ──── AI Factsheet
│   │   └── Evaluations, versions, approvals
│   │
│   ├── Asset: Extraction Prompt ──────── AI Factsheet
│   │   └── Evaluations, versions, approvals
│   │
│   └── Asset: Next Steps Prompt ──────── AI Factsheet
│       └── Evaluations, versions, approvals
│
└── Shared: Test Datasets (governed separately)
```

### What Gets Its Own AI Factsheet?

| Asset | Gets Factsheet? | Why |
|-------|-----------------|-----|
| Each prompt template | Yes | Track versions, evaluations |
| Each fine-tuned model | Yes | Full lineage required |
| Foundation model | No | Provider's responsibility |
| Dataset | Optional | Data governance separate |
| RAG pipeline | Yes (as composite) | Track overall system |

---

## Summary: Governance Scope Decision Matrix

| Question | Answer | Action |
|----------|--------|--------|
| Is it customer-facing? | Yes | Full governance (Tier 1) |
| Does it make decisions? | Yes | Full governance (Tier 1) |
| Is it regulated? | Yes | Full governance (Tier 1) |
| Is it internal only? | Yes | Standard governance (Tier 2) |
| Is it experimental? | Yes | Minimal governance (Tier 3) |
| Did you modify the model? | Yes | Full governance required |
| Using pre-trained as-is? | Yes | Govern your prompts only |

---

## Key Takeaways

1. **Not everything needs full governance** — Apply governance proportionally to risk

2. **Govern what you control** — Focus on prompts, fine-tuned models, and pipelines you build

3. **Asset-level flexibility** — Each prompt/model can have its own governance tier

4. **Start with high-risk assets** — Prioritize customer-facing and decision-making AI

5. **Use AI Use Cases to organize** — Group related assets under one governance umbrella

6. **Iterate as needed** — Start minimal, add governance as use case matures

---

**Document Version:** 1.0
**Last Updated:** 2026-02-01
