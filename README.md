# 🏦 Multi-Agent AI System: Insurance Claims Processing & Fraud Detection

A multi-agent AI pipeline that simulates an intelligent insurance claims processing system, built with **Microsoft AutoGen v0.7+** and powered by **Google Gemini 2.5 Flash Lite**.

---

## 📌 Project Overview

Real-world insurance claims departments involve multiple specialists reviewing every claim, from data extraction to policy validation, fraud analysis, and final approval. This project replicates that workflow using four collaborating AI agents, each acting as an expert in one domain.

The system processes raw claim submissions and produces a structured final recommendation - **APPROVE**, **APPROVE WITH CONDITIONS**, **INVESTIGATE**, or **REJECT** - with full reasoning at every step.

---

## 🧠 System Architecture

```
Raw Claim Input
       │
       ▼
┌─────────────────────┐
│  Claims_Extractor   │  → Structures raw claim into clean key fields
└─────────────────────┘
       │
       ▼
┌─────────────────────┐
│  Policy_Validator   │  → Checks policy eligibility, coverage & timelines
└─────────────────────┘
       │
       ▼
┌─────────────────────┐
│   Fraud_Detector    │  → Scores fraud risk (1–10), lists red flags
└─────────────────────┘
       │
       ▼
┌─────────────────────┐
│  Senior_Reviewer    │  → Synthesises all findings → Final decision
└─────────────────────┘
       │
       ▼
 Final Claims Report
```

Agents communicate via **RoundRobinGroupChat**, where each agent reads the full conversation history before contributing its analysis. The pipeline terminates automatically when the Senior Reviewer outputs `TERMINATE`.

---

## 🤖 The Four Agents

| Agent | Role | Output |
|---|---|---|
| `Claims_Extractor` | Reads and structures the raw claim | Bulleted summary of key claim fields |
| `Policy_Validator` | Checks coverage, active dates, and submission timelines | ELIGIBLE / REQUIRES REVIEW / INELIGIBLE |
| `Fraud_Detector` | Identifies red flags and scores fraud risk | Fraud risk score (1–10) + red flag list |
| `Senior_Reviewer` | Synthesises all findings into a final report | Final recommendation + reasoning |

---

## 🧪 Test Scenarios

### Claim 1 - Suspicious (CLM-2024-00892)
James Carter | Health Insurance | $18,500 | Emergency cardiac hospitalisation

**Results:**
- Policy Validator → **REQUIRES REVIEW** (45-day delayed submission needs verification)
- Fraud Detector → **Score: 7/10 (HIGH)** - 4 red flags identified:
  - Policy upgraded just 2 months before a large claim
  - 45-day delayed submission for an emergency hospitalisation
  - Unusually large claim amount
  - Pattern of 3 prior claims in 12 months
- Senior Reviewer → **INVESTIGATE** - medical event is plausible but red flags require deeper verification before approval

---

### Claim 2 - Low Risk (CLM-2024-00901)
Sarah Obi | Health Insurance | $850 | Annual health check & blood tests

**Results:**
- Policy Validator → **REQUIRES REVIEW** (routine check to confirm preventive care coverage)
- Fraud Detector → **Score: 1/10 (LOW)** - zero red flags:
  - Modest amount consistent with routine services
  - Prompt 5-day submission
  - Long-standing policy since 2020, clean history
- Senior Reviewer → **APPROVE** - routine claim, clean profile, low risk

---

## 🛠️ Tech Stack

| Component | Detail |
|---|---|
| Framework | AutoGen v0.7+ (`autogen-agentchat`, `autogen-ext`) |
| LLM Backend | Google Gemini 2.5 Flash Lite (via OpenAI-compatible endpoint) |
| Agent Pattern | RoundRobinGroupChat with TextMentionTermination |
| Environment | Jupyter Notebook (Python 3.x) |

---

## 📁 Repository Structure

```
├── MultiAgent_Claims_FraudDetection.ipynb   # Main notebook
└── README.md
```

---

## 💡 Key Takeaways

- Multi-agent systems can replicate complex, multi-step human workflows with specialist roles at each stage.
- Separating concerns across agents (extraction → validation → fraud → review) improves accuracy and interpretability compared to a single prompt approach.
- The same pipeline correctly differentiates between a high-risk suspicious claim and a routine low-risk claim, demonstrating generalisation across scenarios.
- AutoGen's RoundRobinGroupChat enables seamless agent-to-agent context passing without manual orchestration.

---

## 🔮 Possible Extensions

- Add a **Document_Verifier** agent to cross-check hospital records or doctor credentials
- Integrate a real claims database via tool use / function calling
- Extend to other insurance types (auto, property, life)
- Add a human-in-the-loop approval step using AutoGen's `UserProxyAgent`
- Deploy as a Streamlit or FastAPI web app

---

This project is for educational and portfolio purposes.
