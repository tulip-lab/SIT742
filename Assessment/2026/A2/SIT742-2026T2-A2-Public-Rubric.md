# SIT742 2026T2 Assignment 2 Public Rubric

## Purpose

This student-facing rubric summarises the public assessment expectations for Assignment 2. It should be read together with the [Assignment Specification](SIT742-2026T2-A2-Specification.md), the [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md), and the [Starter Notebook](SIT742-2026T2-A2-Starter.ipynb) that students complete.

## Mark summary

| Part | Questions | Focus | Marks |
| --- | --- | --- | ---: |
| Part I | Q1-Q3 | Forecast table generation and evaluation functions | 15 |
| Part II | Q4-Q6 | Forecasting project, evaluation, and interpretation | 70 |
| Part III | Q7 | Group video and collaboration presentation | 15 |
|  |  | Total | 100 |

## How to read this rubric

This rubric summarises the public assessment expectations for each question. It should be read together with the Assignment Specification and completed using the Starter Notebook.

The mark allocations below show how each question is broadly assessed. They summarise public expectations rather than listing every possible test case, assessment scenario, or partial-credit case.

Forecast performance is assessed using MASE with naive lag-1 scaling, as explained in the [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md). Performance thresholds and internal marker calibration details are not published. Forecast performance is one of the strongest pieces of evidence for the broad Part II outcome, while communication, reproducibility, cutoff-safe data use, notebook clarity, figure/table labelling, dataset acknowledgement, and avoidance of local absolute paths help determine the confirmed score within the relevant questions. They are not separate additional mark categories unless stated in a question.

## Part I: Forecast Table Generation and Evaluation Functions [15 marks]

### Q1: General naive-lag forecast table generator [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and wide-format input/output | 1 | Defines `generate_naive_forecast_wide(...)`, accepts wide-format historical actual tables, and returns a wide-format forecast table. | Function is present but the signature, input assumptions, or return structure need review. | Required function is missing, renamed, or not assessable. |
| Configurable lag logic | 1 | Correctly supports positive integer `lag`, including `lag=1` recursive carry-forward logic and `lag=12` annual naive logic where available. | One lag mode is mostly correct but another is incomplete or unreliable. | Configurable lag behaviour is missing or mostly incorrect. |
| Fixed-cutoff implementation | 1 | Uses only actual values available at or before the supplied cutoff and does not use future actual/result values as forecast inputs. | Cutoff handling is plausible but needs clearer evidence or has a limited edge-case issue. | Forecast generation appears to use future actual/result values or unclear cutoff logic. |
| All-destination and arbitrary-horizon support | 1 | Supports all required destinations and arbitrary requested forecast months. | Works for some destinations or horizons but coverage is incomplete. | Does not support all-destination or multi-month forecasting. |
| Clean wide-format output | 1 | Output contains `Date` plus required destination columns only, with numeric forecasts and no `model_label` or diagnostic columns. | Output is mostly usable but one schema or value detail needs correction. | Output is not in the required wide forecast-table format. |

### Q2: Forecast and actual table validation/alignment [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and audit structure | 1 | Defines `validate_forecast_actual_wide(...)` and returns inspectable audit evidence. | Validator is present but the signature or return structure needs review. | Required validator is missing, renamed, or not assessable. |
| Forecast-only validation mode | 1 | With `actual_wide_df=None`, checks forecast schema readiness including required months, destination columns, duplicate dates, extra rows/columns, missing values, numeric conversion, and finite values. | Forecast-only checks are present but one or more important checks are incomplete. | Forecast-only validation is missing or unreliable. |
| Forecast-vs-actual alignment mode | 1 | When actual/result data is supplied, checks whether forecast and actual/result tables can align by `Date` and destination columns after using the required evaluation months. | Alignment checks are present but incomplete or difficult to inspect. | Forecast-vs-actual alignment checks are missing or unreliable. |
| Required month, destination, and value-quality checks | 1 | Checks required months, destination columns, numeric conversion, finite values, missing values, duplicate dates, and expected row counts. | Coverage or value-quality checks are present but incomplete. | Core coverage, duplicate-date, or value-quality checks are missing. |
| Useful audit output | 1 | Produces actionable audit fields for alignment, coverage, row counts, duplicates, missing/extra fields, and value quality. | Audit output is inspectable but missing one useful evidence category. | Audit output is not useful for self-checking or marking. |

### Q3: Forecast accuracy metrics from forecast and actual tables [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and destination-level output | 1 | Defines `evaluate_forecast_wide(...)` and returns inspectable destination-level metrics. | Metric function is present but the signature or output structure needs review. | Required metric function is missing, renamed, or not assessable. |
| Metric calculations | 1 | Correctly computes MAE, MASE, MAPE, and RMSE on aligned evaluated rows. | Metrics are mostly correct but one calculation or alignment detail needs review. | Metric calculations are missing or mostly incorrect. |
| MASE denominator and diagnostics | 1 | Computes each destination denominator from all valid chronologically sorted lag-1 training pairs for that destination and reports `denominator` plus `n_denominator_pairs`. | Denominator logic is mostly correct but lagged-pair counting, sorting, or one edge case needs review. | MASE denominator is incorrect, mixes destination histories, or omits denominator diagnostics. |
| Aggregate metrics | 1 | Returns useful unweighted aggregate metrics such as `n_destinations`, `mean_mase`, `median_mase`, `mean_mape`, and `mean_rmse`. | Aggregate metrics are present but incomplete or unclear. | Aggregate metrics are missing or not inspectable. |
| Edge-case handling | 1 | Handles missing actuals, missing forecasts, zero actuals for MAPE, nonfinite values, and zero or unavailable denominators explicitly with `mase_available` and `denominator_warning`. | Edge-case handling is present but incomplete. | Important metric edge cases are not handled. |

### Part I integrated evidence

Part I integrated evidence does not carry separate marks. It supports Q1-Q3 and provides the minimum baseline evidence required in Q4.

It is not an additional mark item; missing or weak integrated evidence affects the relevant Q1-Q3 evidence and the Q4 baseline-comparison evidence.

Stronger submissions use Q1-Q3 together to create and visibly display `baseline_validation_comparison` and `baseline_validation_summary` for both `naive_lag1` and `naive_lag12`, cover all 20 destinations, evaluate validation months `2023M03` to `2023M07`, and report MAE, MASE, MAPE, RMSE, denominator, `n_denominator_pairs`, `mase_available`, and `denominator_warning`.

## Part II: Forecasting Project, Evaluation, and Interpretation [70 marks]

### Q4: Forecasting workflow and model development [20 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Required baseline comparison | 4 | Includes required `naive_lag1` and `naive_lag12` validation baselines for all 20 destination series with validation MASE evidence. | Baseline evidence is present but one required baseline, all-20 support, or validation metric detail is incomplete. | Required baseline comparison evidence is missing or not meaningful. |
| Improved model evidence | 4 | Includes at least one improved forecasting method with reproducible evidence. | Improved model evidence is present but limited, hard to inspect, or not clearly compared. | Improved model evidence is missing. |
| Validation design and validation MASE | 4 | Uses validation months `2023M03` to `2023M07`, reports MASE with naive lag-1 scaling, and avoids training/validation leakage. | Validation evidence is present but MASE scaling, cutoff handling, or coverage needs review. | Validation design or naive lag-1 validation MASE evidence is missing or unreliable. |
| Model comparison and selection rationale | 4 | Justifies model choice by comparing the improved method against required baselines using validation evidence, destination behaviour, and limitations. | Model choice is plausible but baseline comparison, evidence linkage, or limitation discussion is incomplete. | Model selection is weakly justified or not compared against the required baselines. |
| Reproducibility and all-20 destination support | 4 | Workflow is organised, reproducible, and supports all-20 destination forecasting and final wide-table creation. | Workflow is partly reproducible but some all-20 or rerun evidence is weak. | Workflow is not reproducible or does not support all-20 forecasting. |

### Q5: Final forecast submission and assessment-period performance evaluation [30 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Required wide forecast object and CSV schema | 4 | `forecast_submission_wide` is visible in the notebook/PDF, and the submitted CSV uses `Date` plus exactly the 20 public-dataset destination columns with no extra columns. | Wide forecast schema is mostly usable but one column, order, visibility, or export detail needs review. | Required wide forecast object or CSV schema is missing. |
| Required month coverage | 4 | Covers exactly 12 rows for `2023M08` to `2024M07` without duplicate dates or extra rows. | Month coverage is mostly complete but one month, duplicate, or extra-row issue needs review. | Required forecast month coverage is incomplete. |
| Numeric, finite, non-placeholder forecasts | 4 | Forecast values are numeric, finite, destination-specific, non-placeholder values and do not include assessment-period actual/result values. | Some forecast values need review for validity, placeholder risk, or destination specificity. | Forecast values are missing, nonnumeric, nonfinite, placeholder-like, or include invalid actual/result information. |
| Assessment-period MASE performance evidence | 14 | Submitted forecasts show strong MASE performance when compared with assessment-period actual/result values by the teaching-team evaluator, subject to valid schema and marker confirmation. | MASE performance evidence is assessable but weaker, mixed across destinations, or requires manual review before confirming the score. | MASE performance evidence cannot be validly computed or indicates weak forecast performance. |
| Robustness and destination-level sanity | 4 | Forecasts are plausible enough for marker review, destination-specific, and not obviously copied, constant placeholders, or structurally broken. | Forecasts are mostly plausible but need destination-level review or stronger justification. | Forecasts appear structurally broken, copied, constant, or implausible. |

Exact assessment-period MASE thresholds and internal marker calibration details are not published.

### Q6: Metric discussion and selected-market forecast analysis [20 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Metric concepts: MASE, MAPE, and RMSE | 4 | Discusses MASE, MAPE, and RMSE, including strengths, weaknesses, and why MASE is the formal metric. | Metric discussion is present but one metric or tradeoff needs more detail. | Metric discussion is missing or superficial. |
| Evidence use from validation metrics and baseline comparison | 6 | Uses `baseline_validation_summary`, `validation_metrics`, and selected model evidence to support metric discussion and model interpretation. | Uses some validation evidence but baseline comparison or interpretation links are incomplete. | Validation or baseline evidence is missing from the discussion. |
| Selected-market coverage | 3 | Analyses `Australia`, `Japan`, and two justified additional destinations with destination-specific evidence. | Selected-market discussion is present but one required or selected market is incomplete. | Selected-market analysis is missing or not destination-specific. |
| Per-market forecast analysis | 5 | For each selected market, discusses historical pattern, baseline comparison, selected model validation behaviour, final forecast shape and plausibility, and uncertainty or risks. | Per-market discussion is present but one evidence type or market-specific interpretation is incomplete. | Per-market forecast analysis is generic, unsupported, or missing. |
| Evidence presentation and report quality | 2 | Uses clear tables/figures, coherent narrative, and technically defensible conclusions. | Report is understandable but evidence presentation or narrative needs improvement. | Report quality or evidence presentation is weak. |

## Part III: Group Video and Collaboration Presentation [15 marks]

### Q7: Group video and collaboration explanation [15 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Video submission and duration | 3 | Video is submitted and normally within the 8-12 minute requirement, unless an approved arrangement exists. | Video is submitted but duration, access, or format needs review. | Video is missing or inaccessible. |
| All-member participation | 3 | Every listed group member participates or an approved arrangement is documented. | Participation is partly evident but needs review. | Member participation is missing or unclear. |
| Code execution and reproducibility demo | 3 | Video shows code execution and explains how submitted outputs can be reproduced. | Reproducibility demo is present but incomplete. | Code execution or reproducibility demo is missing. |
| Technical explanation | 3 | Video explains problem, data, model, validation, forecast output, interpretation, and key results. | Technical explanation is present but incomplete or uneven across the workflow. | Technical explanation is missing or not connected to submitted work. |
| Contributions and collaboration | 3 | Video explains credible individual contributions and collaboration aligned with the submitted work. | Contribution explanation is present but underdeveloped or needs confirmation. | Contribution and collaboration explanation is missing or not credible. |

## Communication and reproducibility across the assignment

Communication and reproducibility are assessed within the relevant questions, especially Q4-Q7. They are not separate additional mark categories.

Across the assignment, stronger submissions generally:

- run from top to bottom in the submitted notebook;
- keep required object names and outputs easy to locate;
- use labelled figures and tables;
- connect written explanations to generated evidence;
- acknowledge the TULIP Lab `ISF-TDF2023` dataset and any external data sources;
- document cutoff-safe use of external data;
- export the final forecast CSV directly from `forecast_submission_wide` with `index=False`;
- avoid local absolute paths, private files, credentials, restricted data, or undocumented dependencies.
