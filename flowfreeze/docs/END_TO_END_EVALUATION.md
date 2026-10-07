# End-to-End Synthetic Evaluation

## Reproduce

Generate the default deterministic demo data, train the models, then run the held-out experiment:

```powershell
python -m data_generator.generate --seed 42
python -m ml.train --seed 42
python -m core.baseline --split test
```

The command writes `ml/artifacts/end_to_end_metrics.json`; `GET /api/metrics` serves the results to the Evaluation page. Model artifacts are local and ignored by Git. The checked-in result JSON records one run so the demo is reviewable without local joblib files.

## Experiment design

- Both strategies use the same 165 held-out synthetic incident cases, decision-time model scores, proportional taint estimates, policy v1 thresholds, analysis snapshots, and per-wallet hold cap.
- **Direct-recipient-only** can propose only for the receiver of the reported transaction.
- **FlowFreeze network** can propose for any wallet with a positive proportional-taint estimate in the traced scenario.
- A recommendation remains a proposal requiring an analyst. For this offline what-if benchmark only, its amount is scored against the generator's `tainted_amount` remaining at `analysis_at`.
- “Estimated tainted value preserved” is `min(proposed amount, generated remaining taint)`. “Estimated legitimate value affected” is the excess proposal over that generated taint.
- Results assume a proposal takes effect immediately at `analysis_at`; no later transaction replay validates that assumption. These figures are not observed prevented losses or actual customer impact.
- Timing is local per-case model inference, replay, graph, taint, and policy time. It excludes Python process startup, initial feature-table construction, and model loading. It is machine-dependent and is not analyst investigation time.

## Recorded run

Seed 42, default policy v1, eleven generated families, test split (165 cases; the checked-in artifact is authoritative):

| Measure | Direct recipient only | FlowFreeze network | Difference (network minus baseline) |
|---|---:|---:|---:|
| Cases with a proposal | 53 (32.12%) | 88 (53.33%) | +35 cases |
| Wallets with a proposal | 53 | 109 | +56 |
| Proposed amount | ৳530,000.00 | ৳1,087,280.00 | +৳557,280.00 |
| Estimated tainted value preserved | ৳530,000.00 | ৳1,058,554.03 | +৳528,554.03 |
| Estimated legitimate value affected | ৳0.00 | ৳28,725.97 | +৳28,725.97 |

Graph tracing reported an average 0.812 downstream wallets per case (134 across these cases); generated labels indicate 130 tainted downstream-wallet occurrences were found. The measured median per-case recommendation computation time was 64.194 ms and p95 was 77.571 ms on the run environment. Detailed per-family results are in `ml/artifacts/end_to_end_metrics.json`.

The network strategy's additional estimated tainted value comes with additional estimated legitimate value affected. This is a precision/collateral tradeoff under generator assumptions, not evidence that real fraud loss was prevented, customers were protected, or network-based intervention is better for MFS customers. The benchmark does not measure analyst investigation time, operational false-positive rate on representative MFS cases, or actual customer impact. Those require the authorized shadow-mode and analyst-trial plan in [Business Workflow](BUSINESS_WORKFLOW.md) and [Analyst Trial Guide](ANALYST_TRIAL_GUIDE.md). Step 5 model metrics are reported separately and share the limitations in `MODEL_CARD.md`.
