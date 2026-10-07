# FlowFreeze Analyst Trial Guide

## Purpose

Use this guide to learn whether FlowFreeze helps MFS fraud/risk analysts investigate synthetic or, only under explicit operator authorization, approved trial cases. The current repository demo is **synthetic-only**. Do not enter customer or production data into it. A real-data trial requires the authorization and safeguards in [Business Workflow](BUSINESS_WORKFLOW.md).

## Trial design

1. Recruit representative fraud/risk analysts and explain that model output is advisory.
2. Use a preselected set of cases with known provenance and independently adjudicated outcomes. For any real cases, obtain documented operator, privacy, security, legal, and compliance approval first.
3. Have analysts first record their normal-workflow assessment, then review FlowFreeze evidence (or counterbalance order across analysts) to limit anchoring.
4. Record task start/end, evidence checked, downstream wallets found, confidence, final decision, rationale, and legitimate/customer impact. Do not let shadow results affect live actions.
5. Compare against baseline using paired cases and report uncertainty, missing labels, disagreements, and negative effects—not only favorable examples.

## Analyst questions

Rate each item (1 = not at all, 5 = very much) and add a concrete example.

1. Did FlowFreeze identify useful evidence faster?
2. Did the graph reveal downstream wallets you would otherwise miss?
3. Was the estimated exposure useful? What would make it more trustworthy?
4. Was the next-move prediction useful for prioritizing timing?
5. Was the recommendation appropriate under the applicable policy?
6. Did FlowFreeze reduce investigation effort without reducing investigation quality?
7. Did it create unnecessary alerts or draw attention away from more important cases?
8. Would you trust it as analyst decision support? What evidence would you need first?
9. What evidence was missing, misleading, or difficult to verify?
10. What would make this usable in your workflow?

## Trial measures

- Alert-to-disposition elapsed time and active analyst minutes (report separately).
- Useful evidence found, downstream-wallet discovery, and exposure estimate error against adjudicated reference.
- Analyst agreement, recommendation usefulness, and changes from baseline decision.
- Additional alerts, false-positive rate at a fixed threshold, and cases requiring rework.
- Legitimate cases/value affected, unnecessary restriction count/duration, disputes/reversals, and proportionate interventions.
- Analyst workload, cases completed per analyst-hour, and quality/reopen rate.

**Do not equate API/model latency with investigation-time improvement. Do not call a simulated amount “fraud loss prevented.”** Report sample sizes, confidence intervals, subgroup results where lawful/appropriate, and all stop events. Predefine acceptable harm thresholds with the operator before shadow mode.

## Trial record (one per case)

| Field | Entry |
|---|---|
| Trial case reference / analyst / date | |
| Normal-workflow start, active minutes, disposition | |
| FlowFreeze review start/end | |
| Evidence useful / missing / incorrect | |
| Downstream wallets discovered (baseline vs assisted) | |
| Exposure estimate vs adjudicated reference | |
| Recommendation usefulness and rationale | |
| Final human decision / owner / policy reference | |
| Legitimate-customer impact / dispute / reversal | |
| Follow-up or stop condition | |

## Gate to a controlled pilot

Proceed only if the shadow evaluation is authorized, independently reviewed, demonstrates operational value against the pre-registered baseline, meets calibration and subgroup checks, and remains within customer-harm guardrails. A pilot is a separate approval and implementation decision; this guide does not authorize live intervention.
