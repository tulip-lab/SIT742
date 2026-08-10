# SIT742 2026T2 Assignment 2 Specification: Group Forecasting Project

<div style="border: 3px solid #1d4ed8; padding: 0.9rem 1rem; border-radius: 6px; background: #eff6ff;">
<strong>Forecasting Performance Determines Your Final Grade Band</strong>
<p>Your Forecasting Performance Grade determines the grade band for your final A2 mark.</p>
<ul>
<li><strong>Forecasting measures:</strong> The grade is determined primarily by MASE. MAPE is used as supporting cross-check evidence.</li>
<li><strong>Questions Q1-Q7:</strong> Your marks for these questions determine the exact final mark within the grade band.</li>
<li><strong>Grade bands:</strong> HD: 80-100; D: 70-79; C: 60-69; P: 50-59; F: 0-49.</li>
</ul>
<p>For example, if your Forecasting Performance Grade is D, your final A2 mark will be between 70 and 79, with the exact mark determined by your marks for Questions Q1-Q7.</p>
</div>

<div style="border: 2px solid #b91c1c; padding: 0.9rem 1rem; border-radius: 6px; background: #fef2f2;">
<strong>Required autograding markers</strong>
<p>The starter notebook contains START/END comments such as <code>&lt;!-- A2:ANSWER:Q1:START --&gt;</code>. Do not delete, rename, edit, move, or reorder any <code>A2:ANSWER</code> marker. Write each response between the matching START and END markers for that question. Changed or missing markers may prevent a response from being detected correctly during marking.</p>
</div>

<div style="border: 2px solid #0f766e; padding: 0.9rem 1rem; border-radius: 6px; background: #ecfdf5;">
<strong>Submission</strong>
<p>Submit the required files in CloudDeakin via <strong>Assignments</strong> located under <strong>Assessment</strong>. The PDF must be exported from your completed notebook and include relevant outputs, figures, tables, and written explanations.</p>
</div>

<div style="border: 2px solid #0f766e; padding: 0.9rem 1rem; border-radius: 6px; background: #ecfdf5;">
<strong>Assessment Support</strong>
<p>For current Deakin information about assessment submission, extensions, special consideration, and late penalties, refer to the official Deakin assessment support page and the unit site.</p>
</div>

## Contents

- [SIT742 2026T2 Assignment 2 Specification: Group Forecasting Project](#sit742-2026t2-assignment-2-specification-group-forecasting-project)
  - [Contents](#contents)
  - [Overview](#overview)
  - [Assessment Structure](#assessment-structure)
  - [Required Files](#required-files)
  - [Required Final Forecast CSV](#required-final-forecast-csv)
  - [Dataset](#dataset)
  - [Required Scaffold Names](#required-scaffold-names)
  - [Part I: Forecast Table Functions](#part-i-forecast-table-functions)
    - [Q1. General Naive-Lag Forecast Table Generator \[5 Marks\]](#q1-general-naive-lag-forecast-table-generator-5-marks)
    - [Q2. Forecast And Actual Table Validation/Alignment \[5 Marks\]](#q2-forecast-and-actual-table-validationalignment-5-marks)
    - [Q3. Forecast Accuracy Measures From Forecast And Actual Tables \[5 Marks\]](#q3-forecast-accuracy-measures-from-forecast-and-actual-tables-5-marks)
    - [Part I Integrated Evidence](#part-i-integrated-evidence)
  - [Part II: Forecasting Project](#part-ii-forecasting-project)
    - [Q4. Forecasting Workflow And Model Development \[20 Marks\]](#q4-forecasting-workflow-and-model-development-20-marks)
    - [Q5. Final Forecast Submission And Assessment-Period Performance Evaluation \[30 Marks\]](#q5-final-forecast-submission-and-assessment-period-performance-evaluation-30-marks)
    - [Q6. Performance-Measure Discussion And Selected-Market Forecast Analysis \[20 Marks\]](#q6-performance-measure-discussion-and-selected-market-forecast-analysis-20-marks)
  - [Part III: Group Video](#part-iii-group-video)
    - [Q7. Group Video And Collaboration Explanation \[15 Marks\]](#q7-group-video-and-collaboration-explanation-15-marks)
  - [External Data And Cutoff Rules](#external-data-and-cutoff-rules)
  - [Reproducibility Requirements](#reproducibility-requirements)
  - [Submission Checklist](#submission-checklist)

## Overview

In this assignment, your group will build a reproducible forecasting workflow for monthly Chinese outbound tourism demand using the public TULIP Lab `ISF-TDF2023` dataset. You will:

- prepare public tourism demand data;
- implement wide-table forecast generation, validation, and performance-measure functions;
- compare simple baselines with at least one improved forecasting method;
- submit all-20 destination forecasts for `2023M08` to `2024M07`;
- discuss forecast performance and selected destination markets;
- explain the workflow in a compulsory group video.

Any forecasting method is allowed, including statistical models, machine-learning models, deep-learning models, ensemble methods, rule-based baselines, or hybrid approaches. Marks are awarded for valid forecasting design, cutoff-safe implementation, reproducibility, forecast stability, forecast performance, and interpretation, not for using a specific algorithm.

<div style="border: 2px solid #475569; padding: 0.9rem 1rem; border-radius: 6px; background: #f8fafc;">
<strong>Reproducibility and Stability</strong>
<p>Reproducibility and stability are assessed across Q4-Q7. Your notebook should rerun from a clean runtime using documented inputs, avoid local absolute paths and manual post-processing, preserve required object names and forecast schema, handle missing/nonfinite/zero-value edge cases explicitly, and produce stable all-20 destination forecasts rather than placeholders or brittle one-off outputs.</p>
</div>

Forecast performance is assessed using both MASE and MAPE. These measures are not interchangeable and neither one should be omitted. For definitions, denominator handling, missing-value handling, zero-actual handling, and worked examples, use the [A2 Forecast Measures Guide](SIT742-2026T2-A2-Forecast-Measures-Guide.md). For detailed Q1-Q3 toy inputs, expected outputs, and function I/O examples, use the [A2 Function Examples Guide](SIT742-2026T2-A2-Function-Examples.md).

Detailed marking rubrics will be provided through CloudDeakin site.

## Assessment Structure

This assignment assesses ULO3, ULO4, and ULO5. Use the current unit guide and unit site as the source of truth for final learning-outcome wording.

| Part | Questions | Component | Marks |
| --- | --- | --- | ---: |
| Part I | Q1-Q3 | Forecast table generation and evaluation functions | 15 |
| Part II | Q4-Q6 | Forecasting project, evaluation, and interpretation | 70 |
| Part III | Q7 | Group video and collaboration presentation | 15 |
|  |  | Total | 100 |

Question-level mark structure:

| Question | Component | Marks |
| --- | --- | ---: |
| Q1 | General naive-lag forecast table generator | 5 |
| Q2 | Forecast and actual table validation/alignment | 5 |
| Q3 | Forecast accuracy measures from forecast and actual tables | 5 |
| Q4 | Forecasting workflow and model development | 20 |
| Q5 | Final forecast submission and assessment-period performance evaluation | 30 |
| Q6 | Performance-measure discussion and selected-market forecast analysis | 20 |
| Q7 | Group video and collaboration explanation | 15 |
|  | Total | 100 |

## Required Files

Submit these files using the group identifier specified in CloudDeakin or by the teaching team:

```text
SIT742-2026T2-A2-<GroupID>.ipynb
SIT742-2026T2-A2-<GroupID>.pdf
SIT742-2026T2-A2-<GroupID>-Forecast.csv
SIT742-2026T2-A2-<GroupID>-Video.<approved format or link>
```

If your group uses external data, also submit reproducible external-data evidence, normally:

```text
external_data/
```

Follow the CloudDeakin Assignment requirements for video format, peer/self-review, contribution declarations, and any additional submission fields.

## Required Final Forecast CSV

The required final forecast output is a wide-format forecast table named `forecast_submission_wide` in the notebook. The submitted forecast CSV must be exported directly from this table.

Required schema:

```text
Date, <20 destination columns exactly matching the public dataset>
2023M08, <forecast>, <forecast>, ...
2023M09, <forecast>, <forecast>, ...
...
2024M07, <forecast>, <forecast>, ...
```

`forecast_submission_wide` and the final forecast CSV must have:

- one `Date` column;
- exactly 12 rows, one for each month from `2023M08` to `2024M07`;
- exactly the 20 destination columns from the public dataset;
- destination column names that match the public dataset exactly;
- numeric and finite forecast values only;
- no assessment-period actual/result values, which are not provided to students;
- no `model_label`, confidence intervals, diagnostic columns, notes, comments, or extra columns.

You may use long format internally, but the final submitted forecast CSV must be wide. If you model in long format, convert to `forecast_submission_wide` before exporting the CSV.

Use:

```python
forecast_submission_wide.to_csv(
    "SIT742-2026T2-A2-<GroupID>-Forecast.csv",
    index=False,
)
```

Illustrative final forecast rows:

| Date | Australia | Japan | `<another destination>` | ... |
| --- | ---: | ---: | ---: | ---: |
| `2023M08` | `12345.67` | `23456.78` | `34567.89` | `...` |
| `2023M09` | `12500.00` | `23600.00` | `34600.00` | `...` |
| `...` | `...` | `...` | `...` | `...` |

## Dataset

Use the public TULIP Lab `ISF-TDF2023` dataset.

Dataset repository page:

https://github.com/tulip-lab/open-data/tree/main/ISF-TDF2023

Approved raw CSV URL for Python loading:

```text
https://raw.githubusercontent.com/tulip-lab/open-data/main/ISF-TDF2023/ISF-TDF2023.csv
```

Use the raw CSV URL in `pandas.read_csv(...)`. Do not use the GitHub repository page URL inside `read_csv(...)`.

Dataset source and acknowledgement:

```text
Dataset: ISF-TDF2023.
Publisher: TULIP Lab.
Repository: TULIP Lab Open Data Repository, https://github.com/tulip-lab/open-data.
Licence: CC BY 4.0 unless otherwise stated.
Suggested attribution: TULIP Lab. TULIP Lab Open Data Repository. https://github.com/tulip-lab/open-data. Licensed under CC BY 4.0. Also cite Zhang, Song, Li, Law, and Li (2026).
Associated publication: Zhang, Yishuo; Song, Baobao; Li, Xin; Law, Rob; Li, Gang (2026). Imputation recovery tourism demand forecasting. Annals of Tourism Research, 118, Article 104144. https://doi.org/10.1016/j.annals.2026.104144.
```

Use this acknowledgement text, or an equivalent citation, in your notebook and exported PDF. Also acknowledge any additional external data sources.

## Required Scaffold Names

Complete the starter notebook. Preserve the required question headings, function names, object names, and written-answer markers. You may add helper cells, helper functions, models, packages, and intermediate variables.

Required functions:

| Name | Meaning |
| --- | --- |
| `generate_naive_forecast_wide(historical_actual_wide, cutoff_label, forecast_months, required_destinations=None, lag=1, date_column="Date")` | Returns a pandas DataFrame containing `date_column` followed by the requested destination columns. If `required_destinations=None`, use all destination columns in `historical_actual_wide` except `date_column`. |
| `validate_forecast_actual_wide(forecast_wide_df, actual_wide_df=None, required_months=None, required_destinations=None, date_column="Date")` | Returns the fixed Q2 audit dictionary defined below for forecast-only readiness and, when actual/result data is supplied, alignment readiness. |
| `evaluate_forecast_wide(forecast_wide_df, actual_wide_df, training_actual_wide_df, start_month, end_month, required_destinations=None, naive_lag=1, date_column="Date")` | Returns `(destination_metrics, aggregate_metrics)`, where the first item is a fixed-column DataFrame and the second is a fixed-key dictionary. |

Required scaffold and evidence objects:

| Name | Meaning |
| --- | --- |
| `GROUP_INFO` | Group and member metadata, including Deakin email and email username for each member. |
| `A2_REQUIRED_OBJECTS` | Self-check list of required objects. Preserve the name and list intent. |
| `raw_tourism_data` | Public `ISF-TDF2023` dataset loaded into Python. |
| `training_actual_wide_to_2023M02` | Wide public training actual table up to `2023M02`, used for validation baseline generation and validation MASE denominator history. |
| `validation_actual_wide_2023M03_2023M07` | Visible validation actual/result table for `2023M03` to `2023M07`. |
| `public_history_wide_to_2023M07` | Public historical actual table available up to `2023M07`, used for final forecast training cutoff. |
| `baseline_lag1_validation_forecast_wide` | Wide validation forecast from `generate_naive_forecast_wide(..., lag=1)`. |
| `baseline_lag12_validation_forecast_wide` | Wide validation forecast from `generate_naive_forecast_wide(..., lag=12)`. |
| `baseline_lag1_validation_audit` | Validation/alignment audit for the `lag=1` baseline. |
| `baseline_lag12_validation_audit` | Validation/alignment audit for the `lag=12` baseline. |
| `baseline_validation_comparison` | Destination-level validation measures for both required naive baselines. |
| `baseline_validation_summary` | Aggregate validation measures for both required naive baselines. |
| `tourism_series_long` | Long-format DataFrame with at least `Date`, `destination`, and `demand` columns. |
| `modelling_data_or_feature_table` | Cutoff-safe modelling data, features, or concise model-ready evidence summary. |
| `external_feature_log` | DataFrame with the fixed external-source columns defined below; use a zero-row DataFrame with those columns if no external data is used. |
| `validation_forecasts` | Long-format DataFrame with the fixed minimum columns defined below. |
| `validation_metrics` | Destination-level DataFrame with the fixed minimum columns defined below. |
| `model_summary` | One-row-per-model DataFrame with the fixed minimum columns defined below. |
| `forecast_submission_wide` | Final wide all-20 destination forecast table for `2023M08` to `2024M07`. |
| `forecast_submission_audit` | Final forecast-only audit showing whether `forecast_submission_wide` is ready for CSV export. |

## Part I: Forecast Table Functions

Complete Q1-Q3 in the starter notebook. Each required function must return the type and fields specified below and must not rely only on printed output or interactive input. The starter notebook contains compact toy examples and fixed-schema constants. The [A2 Function Examples Guide](SIT742-2026T2-A2-Function-Examples.md) explains the expected Q1-Q3 inputs and outputs in more detail, and the [A2 Forecast Measures Guide](SIT742-2026T2-A2-Forecast-Measures-Guide.md) contains measure definitions and worked examples.

### Q1. General Naive-Lag Forecast Table Generator [5 Marks]

Implement:

```python
generate_naive_forecast_wide(
    historical_actual_wide,
    cutoff_label,
    forecast_months,
    required_destinations=None,
    lag=1,
    date_column="Date",
)
```

Requirements:

- input and output are wide monthly tables with `Date` plus destination columns;
- if `required_destinations=None`, forecast every destination column except `date_column`;
- if `required_destinations` is supplied, forecast exactly those destinations;
- use only actual values available at or before `cutoff_label`;
- support any positive integer `lag`, especially `lag=1` and `lag=12`;
- support arbitrary forecast months and all required destinations;
- compute recursive lag forecasts in chronological month order;
- return a pandas DataFrame with exactly one row per requested forecast month;
- return rows in the supplied `forecast_months` order;
- return columns in this exact order: `date_column`, followed by the inferred or supplied destination order;
- return no index column, `model_label`, diagnostics, or other extra columns.

Lag rule: for forecast month `m`, use source month `m - lag`. If the source month is after the cutoff but has already been forecast, use the generated forecast recursively. If no observed or generated source value is available, return `NaN` or raise a clear error. Convert labels such as `2023M12` and `2024M01` with `pandas.Period(..., freq="M")` or an equivalent year-month representation; do not subtract strings.

### Q2. Forecast And Actual Table Validation/Alignment [5 Marks]

Implement:

```python
validate_forecast_actual_wide(
    forecast_wide_df,
    actual_wide_df=None,
    required_months=None,
    required_destinations=None,
    date_column="Date",
)
```

In this question, `df` means a pandas DataFrame. Both table inputs use the same wide monthly shape as the final CSV: one row per month, one `Date` column, and one numeric column per destination. This is not a long table with separate `destination` and `demand` columns.

The function must support two modes:

- forecast-only mode: `actual_wide_df=None`, used to check `forecast_submission_wide` before CSV export;
- forecast-vs-actual mode: both forecast and actual/result tables are supplied, used for validation and performance-measure alignment.

Requirements:

- check required `Date`, months, destination columns, row counts, duplicate dates, extra rows/columns, missing values, numeric values, and finite values;
- when `actual_wide_df` is supplied, check safe alignment by `Date` and destination columns;
- if `required_destinations=None`, infer all destination columns in `forecast_wide_df` except `date_column`;
- for all-20 assignment evidence, verify the inferred or supplied destinations exactly match `REQUIRED_DESTINATIONS`;
- for final forecast audit, pass `required_destinations=REQUIRED_DESTINATIONS`;
- return a Python dictionary containing every required Q2 audit key below.

Forecast tables are strict: after subsetting to the required task, they should contain exactly the required months and required destinations, with no extra rows or extra columns for final submission. Actual/result tables used for validation may contain extra source months, but missing required evaluation months remain invalid.

Required Q2 audit dictionary keys, in this order:

```text
is_valid
can_align
forecast_row_count
actual_row_count
expected_row_count
missing_months_in_forecast
missing_months_in_actual
extra_months_in_forecast
extra_months_in_actual_source
missing_destinations_in_forecast
missing_destinations_in_actual
extra_columns_in_forecast
duplicate_forecast_dates
duplicate_actual_dates
missing_forecast_value_count
missing_actual_value_count
nonnumeric_forecast_value_count
nonnumeric_actual_value_count
nonfinite_forecast_value_count
nonfinite_actual_value_count
```

The dictionary must always contain all 20 keys. In forecast-only mode, set `can_align` and every actual-related field to `None`; do not omit those keys. In forecast-vs-actual mode, `actual_row_count` is the number of actual/result rows after subsetting to `required_months`. `can_align` reports structural month/destination alignment, while `is_valid` reports whether all applicable structure and value-quality checks pass. Count only duplicate occurrences after the first occurrence of a month. List fields contain month, destination, or column names; count fields contain integers. The [A2 Function Examples Guide](SIT742-2026T2-A2-Function-Examples.md) defines each audit field and its counting scope.

### Q3. Forecast Accuracy Measures From Forecast And Actual Tables [5 Marks]

Implement:

```python
evaluate_forecast_wide(
    forecast_wide_df,
    actual_wide_df,
    training_actual_wide_df,
    start_month,
    end_month,
    required_destinations=None,
    naive_lag=1,
    date_column="Date",
)
```

Requirements:

- align forecast and actual/result tables by `Date` and destination;
- work destination by destination;
- select evaluation months from `start_month` to `end_month` inclusive;
- if `required_destinations=None`, evaluate every destination column in `forecast_wide_df` except `date_column`;
- if `required_destinations` is supplied, evaluate exactly those destinations;
- compute MAE, MASE, MAPE, denominator, `n`, `n_denominator_pairs`, `mase_available`, and `denominator_warning`;
- keep MASE and MAPE separately identifiable;
- sort `training_actual_wide_df` chronologically by `Date` before computing MASE denominator pairs;
- compute each destination's MASE denominator from all valid lagged training pairs for that destination;
- use `naive_lag=1` for official validation comparisons unless this specification explicitly says otherwise;
- handle missing actuals, missing forecasts, zero actuals for MAPE, nonfinite values, and zero or unavailable denominators explicitly;
- return exactly two objects as `(destination_metrics, aggregate_metrics)`.

Do not fill missing denominator values with zero. Do not use validation actuals or assessment-period actual/result values to repair denominators. Do not use another destination's denominator. If actual values are zero, do not divide by zero for MAPE; exclude those rows and report the effect clearly. `lag=12` baseline forecasts are for comparison; they are not the official MASE denominator.

`destination_metrics` must be a pandas DataFrame with these columns in this order:

```text
destination
n
mae
mase
mape
denominator
n_denominator_pairs
mase_available
denominator_warning
```

`aggregate_metrics` must be a Python dictionary with exactly these keys in this order:

```text
n_destinations
mean_mase
median_mase
mean_mape
```

Aggregate measures are unweighted summaries of destination-level measures unless otherwise stated.

### Part I Integrated Evidence

Use Q1-Q3 together to create visible validation baseline evidence. Integrated evidence does not carry separate marks; it demonstrates Q1-Q3 working together and provides baseline evidence required for Q4.

Required workflow:

1. Create `training_actual_wide_to_2023M02` and `validation_actual_wide_2023M03_2023M07` from `raw_tourism_data`.
2. Generate `lag=1` and `lag=12` validation forecasts for `2023M03` to `2023M07` with cutoff `2023M02`.
3. Validate both baseline forecasts against `validation_actual_wide_2023M03_2023M07`.
4. Evaluate both baselines with `evaluate_forecast_wide(..., naive_lag=1)` for official MASE denominator scaling.
5. Produce and display `baseline_validation_comparison` and `baseline_validation_summary`.

`baseline_validation_comparison` must include:

```text
model_label
lag
destination
n
mae
mase
mape
denominator
n_denominator_pairs
mase_available
denominator_warning
```

`baseline_validation_summary` must include:

```text
model_label
lag
n_destinations
mean_mase
median_mase
mean_mape
```

Both tables must cover both required baselines and all 20 destinations, be visible in the notebook, and appear in the exported PDF.

## Part II: Forecasting Project

### Q4. Forecasting Workflow And Model Development [20 Marks]

Build a reproducible modelling workflow that creates validation forecasts, compares forecasting approaches, selects a final method, and produces `forecast_submission_wide`.

Expected Q4 evidence objects:

```python
tourism_series_long
modelling_data_or_feature_table
external_feature_log
validation_forecasts
validation_metrics
model_summary
forecast_submission_wide
```

Requirements:

- create or document `tourism_series_long` with at least `Date`, `destination`, and `demand` columns;
- prepare cutoff-safe modelling inputs, features, or model-ready evidence;
- document external data in `external_feature_log`, or create a structured empty log if no external data is used;
- use validation months `2023M03` to `2023M07`;
- compare at least `naive_lag1`, `naive_lag12`, and one improved method;
- use MASE with naive lag-1 scaling and MAPE as required validation performance measures;
- report MAE where it helps audit the MASE numerator and forecast-error handling;
- explain trade-offs if MASE and MAPE favour different methods;
- avoid training/validation leakage;
- document cutoff compliance, rerun steps, package assumptions, random seeds, and file paths where relevant;
- support all 20 destination series;
- produce a stable final `forecast_submission_wide`.

Minimum Q4 evidence contracts:

| Object | Required type and minimum columns |
| --- | --- |
| `tourism_series_long` | DataFrame containing `Date`, `destination`, `demand`. |
| `external_feature_log` | DataFrame containing every column in `EXTERNAL_FEATURE_LOG_COLUMNS`; zero rows are valid only when no external data is used. |
| `validation_forecasts` | DataFrame containing `model_label`, `Date`, `destination`, `forecast`. |
| `validation_metrics` | DataFrame containing `model_label`, `destination`, `n`, `mae`, `mase`, `mape`, `denominator`, `n_denominator_pairs`, `mase_available`, `denominator_warning`. |
| `model_summary` | DataFrame containing `model_label`, `model_family`, `n_destinations`, `mean_mase`, `median_mase`, `mean_mape`, `selected`, `selection_rationale`, `reproducibility_notes`, `stability_notes`. |

These are minimum columns; additional useful columns are allowed. `validation_forecasts`, `validation_metrics`, and `model_summary` must cover `naive_lag1`, `naive_lag12`, and at least one improved method. `model_summary` must contain exactly one row with `selected=True`.

Q4 is assessed on reproducible forecasting evidence, not on forcing every method into the same internal data structure. `modelling_data_or_feature_table` therefore remains flexible: if a method does not naturally use a feature table, use this scaffold name for an inspectable model-ready evidence object or concise documented summary.

### Q5. Final Forecast Submission And Assessment-Period Performance Evaluation [30 Marks]

Create and submit the final all-20 destination forecast table for `2023M08` to `2024M07`.

Required object:

```python
forecast_submission_wide
```

Requirements:

- show `forecast_submission_wide` or its final rows in the notebook and PDF;
- ensure it has exactly 12 final forecast rows and exactly the 20 public-dataset destination columns;
- ensure all forecast values are numeric, finite, destination-specific, and stable enough for assessment;
- exclude assessment-period actual/result values;
- avoid placeholder constants, copied actuals, all-constant outputs, uncontrolled rerun changes, and manual post-export edits;
- run `validate_forecast_actual_wide(...)` in forecast-only mode and store the result as `forecast_submission_audit`;
- ensure `forecast_submission_audit` contains all required Q2 keys, has `can_align=None`, and reports `is_valid=True` before export;
- export the final CSV directly from `forecast_submission_wide` with `index=False`.

The teaching-team evaluator will validate the submitted wide CSV and then compare it with assessment-period actual/result values, which are not provided to students. The evaluator uses both MASE and MAPE as assessment evidence.

### Q6. Performance-Measure Discussion And Selected-Market Forecast Analysis [20 Marks]

Part A [10 marks]: discuss forecast-performance measures using the [A2 Forecast Measures Guide](SIT742-2026T2-A2-Forecast-Measures-Guide.md). Use evidence from `baseline_validation_summary`, `validation_metrics`, and selected model evidence. Discuss:

- MASE as a scale-free comparison across destinations;
- MASE limitations when denominators are unstable or based on few valid denominator pairs;
- MAPE as an intuitive percentage error and its instability with zero or small actual values;
- what your validation evidence says about model performance.

Part B [10 marks]: analyse four markets:

- `Australia`;
- `Japan`;
- two additional destinations from the dataset, with a brief justification.

For each selected market, discuss historical demand pattern, baseline comparison, selected model validation behaviour, final forecast shape and plausibility, uncertainty, limitations, and risks. Include at least one supporting plot or table for each selected market.

No separate object named `selected_market_analysis` is required. Complete Q6 in the written-answer cell and place supporting plots or tables near the relevant discussion.

## Part III: Group Video

### Q7. Group Video And Collaboration Explanation [15 Marks]

The group video is compulsory.

The video should:

- be 8-12 minutes long unless an approved alternative arrangement exists;
- include every group member unless the unit chair has approved an alternative arrangement;
- show code execution;
- identify the submitted forecast CSV;
- explain how the CSV can be regenerated from `forecast_submission_wide`;
- explain the problem, data, model, validation, forecast output, interpretation, reproducibility, forecast stability, individual contributions, and collaboration.

Follow the final CloudDeakin Assignment instructions for accepted video format, submission channel, peer/self-review workflow, and alternative-arrangement process.

## External Data And Cutoff Rules

You may use public external data, web-scraped data, or manually collected public reference data if it is public or verifiable, cited, cutoff-safe, and reproducible through submitted files, stable public URLs, pinned versions, or clear retrieval instructions.

Cutoff rules:

- For validation forecasts, all demand data and external data must be available no later than `2023M02`.
- For final `2023M08`-`2024M07` forecasts, all demand data and external data must be available no later than `2023M07`.

Do not use data that only became available after the relevant fixed cutoff. Do not include credentials, private data, restricted datasets, private links, or unverifiable sources.

Use submitted `external_data/` files where redistribution is permitted, and read them with relative paths. If redistribution is not permitted, provide retrieval code, exact public source URL or citation, and enough evidence for verification.

Document every external source in `external_feature_log`. Required columns:

```text
source_name
source_url_or_reference
local_file_or_url
retrieval_date
publication_or_release_date
latest_observation_used
cutoff_applicable
features_created
transformation_summary
licence_or_access_notes
```

## Reproducibility Requirements

Your group should ensure that:

- the notebook runs from top to bottom in a clean runtime;
- required functions, object names, and written-answer markers are preserved;
- public data is loaded from the approved raw URL or documented relative local paths;
- external data is cited, cutoff-safe, and available through submitted files or stable public sources;
- random seeds are set where relevant;
- package requirements are documented where relevant;
- figures and tables are labelled clearly;
- final forecast CSV output can be regenerated directly from `forecast_submission_wide`;
- final forecasts are finite, destination-specific, stable enough for assessment, and not manually edited after export;
- no credentials, private paths, private data, or restricted links are included.

Refer to the current SIT742 unit site and official Deakin pages for academic integrity, GenAI, extensions, late submission, special consideration, and related assessment requirements. The unit site and CloudDeakin Assignment page are the source of truth for final submission instructions.

For current Deakin information about assessments, including assignment submission, extensions, late penalties, special consideration, and related support, refer to:

https://www.deakin.edu.au/students/study-support/assessments-and-examinations/assessments

## Submission Checklist

Before submitting, check that:

- `GROUP_INFO` is complete;
- all required files are included and filenames follow the group pattern;
- required headings, functions, objects, and written-answer markers are preserved;
- the [A2 Forecast Measures Guide](SIT742-2026T2-A2-Forecast-Measures-Guide.md) has been used for MASE and MAPE handling;
- the notebook runs from top to bottom and can regenerate the final forecast CSV;
- the PDF includes relevant outputs, figures, tables, and explanations;
- Q1 returns a DataFrame with the required row and column order, Q2 returns all 20 audit dictionary keys, and Q3 returns `(destination_metrics, aggregate_metrics)`;
- `baseline_validation_comparison` includes both `lag=1` and `lag=12` baselines, all 20 destinations, `mase_available`, and `denominator_warning`;
- `baseline_validation_summary` includes aggregate measures for both baselines;
- `validation_forecasts`, `validation_metrics`, and `model_summary` use their required minimum columns and cover the required models;
- `forecast_submission_wide` has one `Date` column, exactly 20 destination columns, and exactly 12 rows for `2023M08` to `2024M07`;
- `forecast_submission_audit` checks `forecast_submission_wide` in forecast-only mode, contains all 20 Q2 keys, and reports `is_valid=True` before export;
- the final forecast CSV is exported directly from `forecast_submission_wide` with `index=False`;
- all forecast values are numeric, finite, destination-specific, and not placeholders or copied actual/result values;
- stochastic modelling steps use documented seeds or controlled rerun behaviour where relevant;
- all datasets and external resources are acknowledged;
- external data snapshots are included in `external_data/` where required and permitted;
- the video and any peer/self-review or contribution material follow CloudDeakin requirements;
- no credentials, private data, private paths, or restricted links are included.
