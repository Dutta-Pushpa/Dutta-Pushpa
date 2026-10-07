## Project Overview

### Problem Addressed

Digital financial fraud can spread rapidly through multiple wallets and transactions, making manual investigation difficult and time-consuming. Fraud analysts often need to identify suspicious behavior, trace fund movement, estimate potential exposure, and determine appropriate intervention actions while minimizing impact on legitimate users.

### Proposed Solution

FlowFreeze is an AI-assisted fraud containment and fund-flow intelligence platform that combines:

* Fraud risk prediction
* Transaction graph analysis
* Fund-flow tracing
* Taint estimation
* Next-move prediction
* Analyst-reviewed intervention recommendations

The platform provides explainable evidence and decision-support tools to help investigators analyze suspicious incidents using synthetic data in a safe demonstration environment.

### Purpose

The project was developed as a prototype for the upay BD sponsored hackathon to demonstrate how AI can assist fraud investigation workflows while maintaining human oversight and responsible decision-making.

---

## MFS business value proposition

**For** MFS fraud and risk operations, **FlowFreeze** is an AI fund-flow investigation and decision-support platform that helps analysts detect, trace, quantify, predict, and prioritize suspicious money movement. Unlike transaction-level alerts alone, it combines behavioral risk intelligence with multi-hop fund-flow investigation and proportionate, policy-aware recommendations. It is an intelligence layer—not a payment switch, ledger, or transaction engine—and it does not automatically execute a hold or block. No monetary savings or real-world fraud reduction are claimed until independently validated.

### Business actors and needs

| Actor | What they need from FlowFreeze |
|---|---|
| Fraud/Risk Analyst (primary) | A prioritized case queue, explainable evidence, downstream fund trace, exposure estimate, likely next move, recommendation, and a place to record a reasoned decision. |
| Fraud Operations Manager | Workload and urgency visibility, investigation-time and case-outcome measures, and evidence to plan a controlled pilot. |
| Compliance/Risk Team | Traceable evidence, policy context, limitations, and an auditable record of human decisions and model versions. |
| Customer Support / Dispute Team | Clear case context and customer-harm indicators to resolve disputes and avoid unnecessary restrictions. |
| MFS Risk Platform | In a future authorized shadow integration, a read-only event/context interface and a return channel for evaluation; no action authority in this prototype. |
| Transaction Monitoring System | Existing alert context that can be enriched; FlowFreeze does not replace alert generation or transaction processing. |
| MFS customer / account holder | Timely investigation, proportionate evidence-based intervention, and protection from avoidable holds and legitimate-value impact. |
| MFS operator | Measurable operational effectiveness, controlled risk, policy compliance, and customer trust. |

### AI output → business decision

| AI output | Business use | User | Decision supported (never automatic) |
|---|---|---|---|
| Risk score | Prioritize cases for review | Fraud analyst | Which case/wallet to inspect first |
| Money-flow graph | Identify downstream exposure | Fraud analyst | Which linked wallets and transfers to investigate |
| Tainted value | Estimate potential financial exposure | Risk manager / analyst | Whether exposure merits further review |
| Next move | Prioritize intervention timing | Fraud analyst | Whether to investigate urgently or continue monitoring |
| Recommendation | Support a policy decision | Fraud analyst | Monitor, enhanced review, or escalate/hold under operator policy |
| Audit trail | Accountability and review | Compliance / risk team | Whether the rationale, evidence, and decision are documented |

### Business workflow

Transaction → existing transaction monitoring → suspicious alert → FlowFreeze intelligence (behavioral risk, fund tracing, exposure, next-move estimate, recommendation) → fraud analyst → monitor / enhanced review / policy-governed escalation or hold / close → audit and feedback → governed model improvement. The MFS operator's authorized systems retain all transaction and action authority. See [Business Workflow](docs/BUSINESS_WORKFLOW.md).

## Features

In addition to the functionality described throughout this README, FlowFreeze includes:

### AI-Assisted Fraud Risk Detection

* Machine learning based fraud risk scoring
* Decision-time feature engineering
* Explainable model outputs for analyst review

### Fund Flow Intelligence

* Multi-hop transaction tracing
* Downstream wallet discovery
* Transaction relationship visualization

### Taint Estimation

* Proportional attribution methodology
* Estimated exposure tracking
* Visibility constrained to incident analysis time

### Next-Move Prediction

Prediction of likely wallet behavior:

* Forward transfer
* Cash-out
* No movement

### Analyst Decision Support

* Intervention recommendations
* Policy-based evaluation
* Human-in-the-loop review process

### Incident Investigation Dashboard

* Incident management
* Fund-flow analysis
* Evaluation metrics
* Synthetic case replay

---

## Business validation and responsible impact

The current interface and checked-in benchmark use **synthetic generated cases only**. Dashboard counters are labeled as synthetic, counterfactual, or session-measured; they are not evidence of observed loss reduction, customer outcomes, or MFS performance. In the held-out synthetic 165-case comparison, the network strategy estimates ৳528,554.03 additional tainted value preserved versus direct-recipient-only, alongside ৳28,725.97 additional legitimate value affected. These are label-based immediate-action counterfactuals, not actual savings or prevented losses. The benchmark's local median recommendation computation time is 64.194 ms (excluding startup and model/table loading); it is not analyst investigation time. The current synthetic scenario mix is not a basis for a production false-positive rate.

A real-world evaluation requires operator authorization, privacy/legal and compliance review, representative independently adjudicated cases, a read-only shadow-mode deployment, pre-registered measures, subgroup/calibration checks, and a separately approved controlled pilot. Do not use customer or production data in this demo. See [Analyst Trial Guide](docs/ANALYST_TRIAL_GUIDE.md), [Business Workflow](docs/BUSINESS_WORKFLOW.md), [Responsible AI](docs/RESPONSIBLE_AI.md), and [End-to-end evaluation](docs/END_TO_END_EVALUATION.md).

## Technology Stack

### Programming Languages

* Python 3.11+
* JavaScript / TypeScript

### Backend

* FastAPI
* Uvicorn

### Frontend

* React
* Vite
* Tailwind CSS

### Machine Learning & Analytics

* scikit-learn
* pandas
* NumPy
* NetworkX
* joblib

### Database

* SQLite

### AI Components

* Fraud Risk Prediction Model
* Next-Move Prediction Model
* Explainability Pipeline
* Intervention Recommendation Engine

### Services & Deployment

* Render
* GitHub

---

## Requirements

### Software Requirements

#### Backend

* Python 3.11 or newer
* pip

#### Frontend

* Node.js 20+
* npm

### Recommended Hardware

* 4 GB RAM minimum
* 8 GB RAM recommended
* Multi-core CPU recommended for model training

---

## Environment Variables

### Backend Environment Variables

Create a `.env` file if required by your deployment environment.

| Variable           | Purpose                      | Example                        |
| ------------------ | ---------------------------- | ------------------------------ |
| APP_ENV            | Application environment      | development                    |
| DATABASE_URL       | Database connection string   | sqlite:///./data/flowfreeze.db |
| RANDOM_SEED        | Synthetic dataset seed       | 42                             |
| ENABLE_DEMO_WRITES | Enable synthetic demo writes | true                           |
| DEMO_WRITE_KEY     | Public synthetic-demo token  | public-synthetic-demo-v1       |

### Frontend Environment Variables

Create `frontend/.env.local`:

```env
VITE_API_BASE_URL=http://127.0.0.1:8000
VITE_DEMO_WRITE_KEY=public-synthetic-demo-v1
```

For the public Render deployment, `VITE_API_BASE_URL` is `https://flowfreeze-api.onrender.com`.

> `public-synthetic-demo-v1` is intentionally public and only unlocks synthetic decision/audit simulations. It is shipped in the frontend bundle, is not authentication, and must never be reused for real services. Never commit real credentials, secrets, or production keys.

---

## Build Instructions

### Frontend Production Build

```bash
cd frontend
npm run build
```

### Render Deployment Build

```bash
pip install -r requirements.txt
bash scripts/render-build.sh
```

### Render Startup Command

```bash
bash scripts/render-start.sh
```

---

## Live Deployment URL

### Frontend

Live Render deployment:

[https://flowfreeze-web.onrender.com](https://flowfreeze-web.onrender.com)

### Backend

Live Render API:

[https://flowfreeze-api.onrender.com](https://flowfreeze-api.onrender.com) · [API docs](https://flowfreeze-api.onrender.com/docs) · [Health](https://flowfreeze-api.onrender.com/health)

---

## Manual Verification Steps

Judges can verify the implemented features by following these steps:

1. Generate synthetic data.
2. Train the models.
3. Start the backend API.
4. Launch the frontend dashboard.
5. Open an incident.
6. Review fraud risk predictions.
7. Inspect downstream fund-flow tracing.
8. Review taint estimation results.
9. Generate intervention recommendations.
10. Execute scenario replay and evaluation workflows.

---

## Additional Configuration

### Important Documentation

The following project documents provide additional configuration, methodology, and evaluation details:

* `docs/API.md`
* `docs/DATA_DICTIONARY.md`
* `docs/INTERVENTION_POLICY.md`
* `docs/MODEL_CARD.md`
* `docs/RESPONSIBLE_AI.md`
* `docs/LIMITATIONS.md`
* `docs/TAINT_METHODOLOGY.md`
* `docs/END_TO_END_EVALUATION.md`
* `docs/BUSINESS_WORKFLOW.md`
* `docs/ANALYST_TRIAL_GUIDE.md`
* `docs/RENDER_FREE_TIER.md`

### Demo Notes

* Synthetic data only
* No production customer information
* No real fund freezing capability
* Human analyst review required for recommendations
* All intervention actions are simulated
