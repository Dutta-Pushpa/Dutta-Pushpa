# FlowFreeze MFS Business Workflow

## Product position

FlowFreeze is an **intelligence and decision-support layer** for an MFS fraud operation. It is not the financial transaction engine, payment switch, ledger, account-control service, or case-management system of record. It does not freeze, reverse, reject, or route real money. Existing operator systems and policy owners retain transaction and action authority.

## Business actor model

| Actor | Need / responsibility | FlowFreeze contribution |
|---|---|---|
| **Fraud/Risk Analyst (primary)** | Triage alerts, assemble evidence, assess urgency, document and decide. | Prioritized queue, risk explanation, graph, exposure estimate, next-move estimate, recommendation, and decision rationale capture. |
| **Fraud Operations Manager** | Balance workload, service levels, quality, and operational risk. | Queue and workload indicators; pilot measures for time, usefulness, and quality. |
| **Compliance/Risk Team** | Review policy adherence, explainability, governance, and accountability. | Evidence references, limitations, policy context, and append-only demo audit history. |
| **Customer Support / Dispute Team** | Resolve customer reports and reduce avoidable customer friction. | Case narrative and legitimate-value / unnecessary-hold indicators for review. |
| **MFS Risk Platform** | Supply authorized, minimal case context and consume approved evaluation outputs. | A future read-only shadow integration only; no action endpoint or write authority in this prototype. |
| **Transaction Monitoring System** | Generate existing transaction alerts. | Remains upstream and authoritative for alert generation; FlowFreeze enriches alerts rather than replacing it. |
| **MFS customer / account holder** | Have suspicious activity investigated while legitimate access and funds are protected. | Indirect beneficiary of timely, evidence-based and proportionate human decisions. |
| **MFS operator** | Protect funds and customers, meet policy obligations, and demonstrate measurable operational value. | A testable analyst aid; business value remains a hypothesis until validated. |

## Workflow placement

```text
TRANSACTION
    ↓
EXISTING TRANSACTION MONITORING
    ↓
SUSPICIOUS ALERT (case context and alert reference)
    ↓
FLOWFREEZE INTELLIGENCE LAYER (read-only analysis)
    ├── behavioral risk score and explanation
    ├── multi-hop fund tracing / money-flow graph
    ├── estimated exposure (tainted and potentially legitimate value)
    ├── likely next move and urgency signals
    └── policy-aware recommendation with evidence references
    ↓
FRAUD ANALYST (primary decision owner)
    ↓
DECISION UNDER EXISTING OPERATOR POLICY
    ├── monitor
    ├── enhanced review / seek more evidence
    ├── request hold/escalate through authorized system and policy
    └── close case / resolve as legitimate
    ↓
AUDIT / FEEDBACK (rationale, outcome, customer impact, provenance)
    ↓
GOVERNED MODEL AND POLICY IMPROVEMENT (after review and approval)
```

A score or recommendation never triggers an action. In this demo, recorded decisions and outcomes are synthetic simulations only.

## AI output to business decision

| AI output | Business use | User | Decision supported |
|---|---|---|---|
| Risk score | Prioritize cases | Fraud analyst | Which case or wallet merits attention first; not a finding of fraud |
| Money-flow graph | Identify downstream exposure | Fraud analyst | Which transfers/wallets to inspect and verify |
| Tainted value | Estimate financial exposure | Risk manager / analyst | Whether exposure warrants more investigation; not legal ownership |
| Next move | Prioritize intervention timing | Fraud analyst | Whether urgency justifies immediate enhanced review |
| Recommendation | Support policy decision | Fraud analyst | Monitor, review, or escalate under existing policy |
| Audit trail | Accountability and review | Compliance / risk team | Whether evidence, rationale, owner, and decision are documented |

## Case prioritization and operational case file

The intended queue uses **CRITICAL / HIGH / MEDIUM / LOW** as analyst-attention bands, considering risk signal, potential exposure, predicted cash-out/forwarding, and response urgency. A deployment must publish and validate the actual policy thresholds; this prototype's scores are not a calibrated operational SLA. Bands prioritize attention—they do not authorize action.

Each case file should make these fields easy to review: **What happened; Why risky; Money at risk; Where money moved; Likely next move; Recommended response; Decision owner; Deadline/urgency; Audit status.** Missing or unavailable evidence must be shown as such rather than inferred.

## Customer protection: “Don't freeze everything.”

Prefer **investigate first when appropriate**. Compare aggressive intervention with evidence-based intervention on the same adjudicated cases. Track legitimate cases affected, unnecessary holds, legitimate value affected, and proportionate interventions. Optimize neither capture nor speed in isolation: assess customer harm, dispute reversals, and service quality alongside exposure. A legitimate customer must have a route for timely review and appeal through the operator's existing process.

## Business KPIs and definitions

| KPI | Definition / source | Current status |
|---|---|---|
| Cases investigated | Cases with an analyst review completed; count and period | Trial measure; demo shows synthetic case inventory, not completed real investigations |
| High-risk cases | Cases meeting a pre-registered review threshold | Synthetic score distribution only until threshold calibrated |
| Potential exposure | Sum of appropriately deduplicated estimated tainted value at decision time | Synthetic label/taint estimate; not loss |
| Exposure identified through tracing | Validated downstream exposure found beyond direct recipient | Measure against analyst baseline and adjudicated reference |
| Simulated prevented exposure | Counterfactual estimate under stated action/outcome assumptions | Synthetic-only; never call observed loss reduction |
| Median investigation time | Alert receipt to documented analyst disposition, with pauses defined | Not measured by model inference latency |
| Analyst workload / productivity | Cases handled per analyst-hour plus quality/reopen rate | Collect in a controlled trial; do not infer from model throughput |
| False-positive rate | Alerted legitimate cases / all adjudicated legitimate cases at fixed threshold | Not defensible from current synthetic mix |
| Customer-harm indicators | Unnecessary holds, legitimate cases/value affected, duration, disputes, and reversal | Must be captured in authorized pilot; no live values available |

## Shadow-mode architecture

1. **Authorized, minimized input:** existing monitoring emits an alert reference and approved, pseudonymized fields through a controlled adapter; do not copy more data than the protocol requires.
2. **Read-only analysis boundary:** FlowFreeze computes scores, traces, and recommendations in an isolated shadow environment. No credentials, API path, or network permission can write to wallet/ledger systems.
3. **Versioned evidence output:** retain model/policy versions, as-of timestamp, features/evidence references, and score/recommendation provenance.
4. **Blinded comparison:** analysts follow normal operations; separately compare FlowFreeze output with existing workflow and independently adjudicated outcomes. Shadow output cannot influence customer actions during the measurement period.
5. **Privacy and retention controls:** access control, encryption, audit, retention/deletion schedule, incident response, and jurisdictional review are prerequisites set by the operator.
6. **Gate to pilot:** only after pre-registered success and harm thresholds, independent validation, approvals, and operational readiness may a separately authorized human-in-the-loop pilot be considered. Any action remains in the operator's existing policy-controlled systems.

## Real-world validation plan

- Obtain MFS operator sponsorship, data-use authority, privacy/security/legal and compliance approval; define purpose, access, retention, and prohibited uses.
- Co-design a representative, time-bounded case sample and independent adjudication protocol, including fraud, legitimate, wrong-recipient/dispute, and ambiguous cases.
- Freeze model, feature, threshold, and policy versions; split by time and customer/case lineage to avoid leakage. Assess calibration, precision/recall at operational capacity, drift, subgroup performance, and graph completeness.
- Run read-only shadow mode against existing analysts/monitoring. Measure baseline vs assisted case-resolution time, useful evidence found earlier, downstream-wallet discovery, exposure-estimate usefulness, extra alerts, and reviewer agreement.
- Record customer outcomes: unnecessary restrictions, legitimate value/duration affected, disputes, reversals, and timely review. Establish harm guardrails and stop conditions before trial.
- Report uncertainty and confidence intervals, missing labels, workload effects, and negative outcomes. Do not claim prevented losses from estimated labels.
- Only then consider a limited, separately approved human-authorized pilot with rollback, monitoring, appeal, and independent oversight.

## Competition criteria crosswalk

| Criterion | Evidence in project / next proof |
|---|---|
| Problem relevance | MFS suspicious-transfer investigation workflow and analyst needs |
| Business/customer impact | Exposure, time, productivity, and harm KPIs specified; real impact awaits validation |
| AI depth | Behavioral model, next-move prediction, graph tracing, proportional taint, explainability, and policy layer |
| Prototype | Synthetic end-to-end analyst workspace, decision simulation, and audit history |
| Innovation | Multi-hop fund-flow intelligence beyond a transaction-level alert |
| Scalability | Read-only adapter/shadow boundary is a proposed architecture; throughput and integration need testing |
| Responsible AI | Human authority, caveats, customer-harm measures, privacy gates, auditability, and no automatic action |

## Exact judge response

- **Judge 3 — “it need to align with the business”:** the product is positioned for MFS fraud/risk operations, identifies the analyst as primary decision owner, maps actors and outputs to existing alert-to-audit workflow, and defines operational and customer-protection KPIs.
- **Judge 1 — validate benefits using real MFS cases and analyst trials:** the current benchmark is explicitly synthetic, not passed off as MFS evidence. The analyst trial guide defines questions; the validation plan requires authorized representative cases, independent labels, shadow-mode measurement, and quantification of exposure, elapsed investigation time, and customer harm before any benefits claim.
- **Judge 2 — demonstrate measurable impact through shadow-mode/pilot:** a read-only shadow design, paired baseline comparison, pre-registered KPIs, harm guardrails, staged approval gate, and limited human-controlled pilot path are documented. The prototype does not claim that a real deployment has occurred.
