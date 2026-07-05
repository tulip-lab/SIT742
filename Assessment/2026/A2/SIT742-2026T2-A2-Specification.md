# SIT742 2026T2 Assignment 2 Specification: Group Forecasting Project

Release level: student-facing

<div style="border: 2px solid #1d4ed8; padding: 0.9rem 1rem; border-radius: 6px; background: #eff6ff;">
<strong>Submission</strong>
<p>Submit the required files in CloudDeakin via <strong>Assignments</strong> located under <strong>Assessment</strong>. The PDF must be exported from your completed notebook and include relevant code outputs, figures, tables, and written explanations.</p>
</div>

<div style="border: 2px solid #0f766e; padding: 0.9rem 1rem; border-radius: 6px; background: #ecfdf5;">
<strong>Assessment Support</strong>
<p>For current Deakin information about assessment submission, extensions, special consideration, and late penalties, refer to the official Deakin assessment support page and the unit site.</p>
</div>

## Contents

- [Front matter](#front-matter)
  - [Assignment overview](#assignment-overview)
  - [Learning outcomes](#learning-outcomes)
  - [Assessment structure](#assessment-structure)
  - [Required final forecast table and CSV structure](#required-final-forecast-table-and-csv-structure)
  - [Dataset and resources](#dataset-and-resources)
  - [Starter notebook and submission workflow](#starter-notebook-and-submission-workflow)
  - [Required scaffold names and meanings](#required-scaffold-names-and-meanings)
  - [How the questions connect to forecasting](#how-the-questions-connect-to-forecasting)
- [Part I: Forecast table generation and evaluation functions](#part-i-forecast-table-generation-and-evaluation-functions)
- [Part II: Forecasting project, evaluation, and interpretation](#part-ii-forecasting-project-evaluation-and-interpretation)
- [Part III: Group video and collaboration presentation](#part-iii-group-video-and-collaboration-presentation)
- [Back matter](#back-matter)
  - [Submission requirements](#submission-requirements)
  - [Forecast CSV format](#forecast-csv-format)
  - [External data and cutoff rules](#external-data-and-cutoff-rules)
  - [Video requirement](#video-requirement)
  - [Reproducibility requirements](#reproducibility-requirements)
  - [Academic integrity and GenAI policy](#academic-integrity-and-genai-policy)
  - [Extensions, special consideration, and submission policy](#extensions-special-consideration-and-submission-policy)
  - [Submission checklist](#submission-checklist)

## Front matter

### Assignment overview

| Item | Details |
| --- | --- |
| Assignment | Assignment 2 |
| Title | Group forecasting project |
| Unit | SIT742 Modern Data Science |
| Teaching period | Trimester 2, 2026 |
| Mode | Group |
| Expected group size | Normally teams of three |
| Weight | 50% of the unit result |
| Internal marks | 100 marks |
| Due date | Saturday 19 September 2026, 8pm AEST |
| Submission location | CloudDeakin > Assessment > Assignments |
| Required files | Completed notebook (`.ipynb`), exported PDF (`.pdf`), final forecast CSV (`.csv`), and group video. If your group uses external data, include the required external-data evidence described below. Follow the CloudDeakin Assignment requirements for any peer/self-review or contribution material. |

In this assignment, your group will build a reproducible forecasting workflow for monthly Chinese outbound tourism demand using the public TULIP Lab `ISF-TDF2023` dataset. You will prepare the data, generate baseline forecast tables, validate forecast and actual/result tables, calculate forecast metrics, compare forecasting methods, submit forecasts for all 20 destination series, interpret selected market behaviour, and explain the workflow in a compulsory group video.

Any forecasting method is allowed, including statistical models, machine-learning models, deep-learning models, ensemble methods, rule-based baselines, or hybrid approaches. Marks are awarded for valid forecasting design, cutoff-safe implementation, reproducibility, forecast performance, and interpretation, not for using a specific algorithm.

Forecast performance is assessed using MASE with naive lag-1 scaling. For each destination, forecast errors over the evaluated horizon are scaled by the average absolute one-step change in the relevant historical actual series. Unless otherwise stated, the MASE denominator is computed from all valid lag-1 pairs in the relevant training actual history available up to the cutoff, not from only the most recent months or only the same number of months as the forecast horizon. Lower MASE indicates better scaled forecast accuracy. Detailed performance-band information is not published. Your marks also depend on valid forecast format, cutoff-safe modelling, validation design, reproducibility, and explanation quality.

<div style="border: 2px solid #b45309; padding: 0.9rem 1rem; border-radius: 6px; background: #fffbeb;">
<strong>Forecast Performance</strong>
<p>Forecast performance is an important driver of the broad Part II outcome and strongly influences the overall assignment result. The final score still depends on valid forecast format, cutoff-safe and reproducible code, validation evidence, model rationale, selected-market analysis, communication quality, and the compulsory group video.</p>
</div>

For the detailed definition of MASE, denominator handling, missing-value treatment, and worked examples, see the accompanying [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md).

### Learning outcomes

This assignment assesses the following unit learning outcomes:

- ULO3: Evaluate modern data analytics techniques for their effectiveness in solving real-world problems.
- ULO4: Apply modern data science methods to solve realistic data analytics problems.
- ULO5: Communicate data science findings, limitations, and implications to relevant audiences.

Use the current unit guide and unit site as the source of truth for final learning-outcome wording.

### Assessment structure

| Part | Component | Marks |
| --- | --- | ---: |
| Part I | Forecast table generation and evaluation functions | 15 |
| Part II | Forecasting project, evaluation, and interpretation | 70 |
| Part III | Group video and collaboration presentation | 15 |
|  | Total | 100 |

Question-level mark structure:

| Question | Component | Marks |
| --- | --- | ---: |
| Q1 | General naive-lag forecast table generator | 5 |
| Q2 | Forecast and actual table validation/alignment | 5 |
| Q3 | Forecast accuracy metrics from forecast and actual tables | 5 |
| Q4 | Forecasting workflow and model development | 20 |
| Q5 | Final forecast submission and assessment-period performance evaluation | 30 |
| Q6 | Metric discussion and selected-market forecast analysis | 20 |
| Q7 | Group video and collaboration explanation | 15 |
|  | Total | 100 |

Detailed marking rubrics and grade descriptors will be provided through the unit site or CloudDeakin assessment rubric.

### Required final forecast table and CSV structure

The required final forecast output is a wide-format forecast table. In your notebook, store this table as `forecast_submission_wide`. The submitted forecast CSV must be exported directly from this table without changing the structure.

The table must use the same structure as the original `ISF-TDF2023` dataset:

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
- exactly the 20 canonical destination columns from the public dataset;
- destination column names that match the public dataset exactly;
- numeric and finite forecast values only;
- no assessment-period actual/result values, which are not provided to students;
- no `model_label` column;
- no confidence intervals, diagnostic columns, notes, comments, or extra columns.

You may use any internal structure for modelling. Long format is allowed internally if it helps your workflow, but the required final submission is wide format. If you use long format internally, convert it to `forecast_submission_wide` before exporting the final forecast CSV.

For the detailed definition of MASE, denominator handling, missing-value treatment, and worked examples, see the accompanying [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md).

### Dataset and resources

Use the public TULIP Lab `ISF-TDF2023` dataset.

Dataset repository page:

https://github.com/tulip-lab/open-data/tree/main/ISF-TDF2023

Approved raw CSV URL for loading the dataset in Python:

```text
https://raw.githubusercontent.com/tulip-lab/open-data/main/ISF-TDF2023/ISF-TDF2023.csv
```

Use the repository page to inspect the dataset documentation. Use the raw CSV URL in your notebook when loading data with Python. Do not use the GitHub repository page URL inside `pandas.read_csv()`.

Dataset source and acknowledgement:

```text
Dataset: ISF-TDF2023.
Publisher: TULIP Lab.
Repository: TULIP Lab Open Data Repository, https://github.com/tulip-lab/open-data.
Licence: CC BY 4.0 unless otherwise stated.
Suggested attribution: TULIP Lab. TULIP Lab Open Data Repository. https://github.com/tulip-lab/open-data. Licensed under CC BY 4.0. Also cite Zhang, Song, Li, Law, and Li (2026).
Associated publication: Zhang, Yishuo; Song, Baobao; Li, Xin; Law, Rob; Li, Gang (2026). Imputation recovery tourism demand forecasting. Annals of Tourism Research, 118, Article 104144. https://doi.org/10.1016/j.annals.2026.104144.
```

Use this acknowledgement text, or an equivalent citation, in your notebook and exported PDF.

Your notebook and PDF must clearly acknowledge the dataset source and any additional external data sources used by your group.

### Starter notebook and submission workflow

You must complete the provided starter notebook for this assignment. The starter notebook contains the required question structure, group metadata, function stubs, object placeholders, forecast-output schema, and written-answer markers that correspond to this specification.

Use this specification as the authoritative description of the assessment requirements. Use the starter notebook as the working file in which you complete your code, tables, figures, forecasts, and written explanations.

For marking and feedback support, complete the `GROUP_INFO` cell and preserve the required question headings, function names, object names, and written-answer markers. You may add additional code or markdown cells where needed, but do not remove or rename required scaffold elements.

In `GROUP_INFO`, list the group identifier, group name, and each member's `student_id`, `name`, `deakin_email`, `email_username`, and `role_or_contribution`. The `email_username` is the part before `@deakin.edu.au`; for example, use `xyz` for `xyz@deakin.edu.au`. If your group has more or fewer than three members because of an approved arrangement, adjust the `members` list and explain this in the contribution and video evidence.

Replace `None` placeholders with completed objects, displayed outputs, or concise documented evidence summaries as appropriate. Hard-required outputs such as the Part I baseline tables, `forecast_submission_wide`, and `forecast_submission_audit` must not remain `None`.

Your exported PDF must be generated from the completed notebook and should show the evidence needed to assess your workflow, outputs, figures, tables, and explanations.

### Required scaffold names and meanings

The starter notebook contains required names that support marking, feedback, and reproducibility checks. Do not delete these names or rename them. You may create additional helper functions, variables, classes, or files as needed.

Required functions:

| Name | Meaning |
| --- | --- |
| `generate_naive_forecast_wide(historical_actual_wide, cutoff_label, forecast_months, required_destinations=None, lag=1, date_column="Date")` | Generates a wide-format naive-lag forecast table from historical actual data. If `required_destinations=None`, use all destination columns in `historical_actual_wide` except `date_column`. |
| `validate_forecast_actual_wide(forecast_wide_df, actual_wide_df=None, required_months=None, required_destinations=None, date_column="Date")` | Checks whether a forecast table has the required wide schema and, when actual/result data is supplied, whether forecast and actual/result tables can be safely aligned and compared. |
| `evaluate_forecast_wide(forecast_wide_df, actual_wide_df, training_actual_wide_df, start_month, end_month, required_destinations=None, naive_lag=1, date_column="Date")` | Calculates destination-level and aggregate forecast accuracy metrics from wide forecast, actual/result, and training actual tables. If `required_destinations=None`, use all destination columns in the forecast table except `date_column`. |

Required scaffold and evidence objects:

| Name | Meaning |
| --- | --- |
| `GROUP_INFO` | Structured group and member metadata, including Deakin email and email username for each member. |
| `A2_REQUIRED_OBJECTS` | The starter notebook's self-check list of required objects. Preserve the name and list intent. |
| `raw_tourism_data` | The public `ISF-TDF2023` dataset as loaded into Python. |
| `training_actual_wide_to_2023M02` | Wide public training actual table up to `2023M02`, used for validation MASE denominators and validation baseline generation cutoff. |
| `validation_actual_wide_2023M03_2023M07` | Visible validation actual/result table for `2023M03` to `2023M07`, used for Q1-Q3 integrated evidence and Q4 validation comparison. |
| `public_history_wide_to_2023M07` | Public historical actual table available up to `2023M07`, used for final forecast training cutoff and final MASE denominator concept; it does not include final-evaluation actual/result values. |
| `baseline_lag1_validation_forecast_wide` | Wide-format validation forecast table generated by the naive `lag=1` baseline. |
| `baseline_lag12_validation_forecast_wide` | Wide-format validation forecast table generated by the naive `lag=12` baseline. |
| `baseline_lag1_validation_audit` | Alignment/audit result for the `lag=1` validation baseline. |
| `baseline_lag12_validation_audit` | Alignment/audit result for the `lag=12` validation baseline. |
| `baseline_validation_comparison` | Destination-level validation metrics for both required naive baselines. |
| `baseline_validation_summary` | Aggregate validation metrics for both required naive baselines. |
| `tourism_series_long` | Scaffold evidence object for a simple long-format representation of the public dataset with at least `Date`, `destination`, and `demand` columns. This helps with analysis and plots, but your forecasting model does not have to use this table internally. |
| `modelling_data_or_feature_table` | Inspectable cutoff-safe modelling data, features, or model-ready evidence used to train, validate, and prepare final forecasts. If your method does not naturally use a feature table, use this name for a concise model-ready evidence summary. |
| `external_feature_log` | Evidence table for every external data source and feature, or a structured empty evidence table if no external data is used. |
| `validation_forecasts` | Your validation forecast evidence, in any clear structure that supports metric calculation and comparison. |
| `validation_metrics` | Validation MASE plus reference diagnostics such as MAPE, RMSE, and MAE. |
| `model_summary` | Summary of baseline and improved models, feature sets or modelling inputs, validation evidence, and selected final model. |
| `forecast_submission_wide` | Final wide-format all-20 destination forecast table for `2023M08` to `2024M07`. This is the object that should be exported directly to the final forecast CSV. |
| `forecast_submission_audit` | Output of your final forecast-table checker showing whether `forecast_submission_wide` is ready for submission. |

### How the questions connect to forecasting

The questions form one forecasting pipeline. Work from earlier questions should feed later questions.

| Question | Forecasting impact |
| --- | --- |
| Q1 | Provides configurable naive `lag=1` and `lag=12` wide-format baseline forecast tables. |
| Q2 | Provides the reusable table-alignment audit used for validation baselines, improved validation forecasts, and final forecast checks. |
| Q3 | Provides destination-level and aggregate validation metrics from forecast, actual/result, and training actual tables. |
| Q4 | Data preparation, modelling evidence, external-source evidence, validation modelling, and model selection are completed here. |
| Q5 | The final forecast CSV is assessed for wide-format structure, all-20 coverage, numeric validity, sanity, and MASE performance against assessment-period actual/result values, which are not provided to students. |
| Q6 | The report explains metric evidence, validation behaviour, final forecast behaviour, limitations, governance, and selected markets. |
| Q7 | The video provides reproducibility, code-execution, technical-ownership, and collaboration evidence. |

## Part I: Forecast table generation and evaluation functions

Complete the required functions and related objects in the starter notebook. Each function must return the required value and must not rely only on printed output. Do not use interactive input.

Part I continues A1-style reusable function design and table-validation practice, but the functions are now explicitly forecast-task oriented.

For the detailed definition of MASE, denominator handling, missing-value treatment, and worked examples, see the accompanying [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md).

The illustrative descriptions below clarify the required behaviour. They are not complete test cases, and your functions should work for valid inputs beyond any examples in the notebook.

### Q1. General naive-lag forecast table generator [5 marks]

Your task:

Write a function that generates configurable naive-lag baseline forecasts in wide format.

Required function signature:

```python
def generate_naive_forecast_wide(
    historical_actual_wide,
    cutoff_label,
    forecast_months,
    required_destinations=None,
    lag=1,
    date_column="Date",
):
    pass
```

Your function should:

- accept and return wide-format tables/DataFrames;
- if `required_destinations=None`, forecast every destination column in `historical_actual_wide` except `date_column`;
- if `required_destinations` is supplied, forecast exactly those destination columns;
- use only actual values available at or before `cutoff_label`;
- support any positive integer `lag`, especially `lag=1` and `lag=12`;
- support all required destinations;
- support arbitrary forecast horizons;
- compute recursive lag forecasts in chronological month order so that any required earlier forecast is available;
- return rows in the requested `forecast_months` order unless you clearly document and use chronological output consistently;
- return a wide-format forecast table with `Date` and the required destination columns;
- not include `model_label` in the wide forecast table.

General lag rule:

For each forecast month `m`, the naive source month is `m - lag` months.

- If the source month is at or before `cutoff_label`, use the observed actual value.
- If the source month is after `cutoff_label` but a generated forecast for that source month already exists, use that forecast recursively.
- If neither an observed value nor a previous forecast is available, return `NaN` or raise a clear error.

For recursive lag forecasts, compute forecasts in chronological month order so that any required earlier forecast is available. Return rows in the requested `forecast_months` order unless you clearly document and use chronological output consistently. `lag` must be a positive integer; if `lag <= 0`, raise a clear error.

You may convert month labels such as `2023M03`, `2023M12`, and `2024M01` to `pandas.Period(..., freq="M")` or an equivalent year-month representation when subtracting lag months. Avoid string subtraction.

When `lag=1`, the method uses the most recent previous month. Under a fixed-cutoff multi-step horizon, this normally becomes recursive/carry-forward because future actual values are unavailable.

When `lag=12`, the method uses the same month from the previous year where available. For monthly data, this represents an annual or seasonal naive baseline. For validation months `2023M03` to `2023M07`, `lag=12` uses `2022M03` to `2022M07` values, because those values are already available before the `2023M02` cutoff.

Illustrative toy example only. Your real submission must apply the same logic to all 20 destinations and the required validation/final periods.

Input `historical_actual_wide`:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2022M03` | `70` | `180` |
| `2022M04` | `75` | `190` |
| `2022M05` | `80` | `200` |
| `2022M06` | `85` | `210` |
| `2022M07` | `90` | `220` |
| `2023M01` | `95` | `230` |
| `2023M02` | `100` | `240` |

Call for `lag=1`:

```python
generate_naive_forecast_wide(
    historical_actual_wide,
    cutoff_label="2023M02",
    forecast_months=["2023M03", "2023M04", "2023M05"],
    lag=1,
)
```

Expected output:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `100` | `240` |
| `2023M04` | `100` | `240` |
| `2023M05` | `100` | `240` |

Call for `lag=12`:

```python
generate_naive_forecast_wide(
    historical_actual_wide,
    cutoff_label="2023M02",
    forecast_months=["2023M03", "2023M04", "2023M05"],
    lag=12,
)
```

Expected output:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `70` | `180` |
| `2023M04` | `75` | `190` |
| `2023M05` | `80` | `200` |

Useful student-facing reference:

```text
Hyndman, R.J. and Athanasopoulos, G. Forecasting: Principles and Practice, 3rd edition, OTexts. Simple forecasting methods: https://otexts.com/fpp3/simple-methods.html
```

### Q2. Forecast and actual table validation/alignment [5 marks]

Your task:

Write a function that checks whether a forecast table and an actual/result table can be safely compared.

Required function signature:

```python
def validate_forecast_actual_wide(
    forecast_wide_df,
    actual_wide_df=None,
    required_months=None,
    required_destinations=None,
    date_column="Date",
):
    pass
```

In this question, `df` means a pandas DataFrame or a DataFrame-equivalent table. The required inputs use the same wide monthly format as the final forecast CSV: one row per month, one `Date` column, and one numeric column per destination. This is not a long table with separate `destination` and `demand` columns.

Input meanings:

| Input | Meaning | Expected content |
| --- | --- | --- |
| `forecast_wide_df` | The forecast table to validate. | A wide table with one `Date` column and destination columns such as `Australia`, `Japan`, and the other required destination names. Each row is one forecast month. Forecast values should be numeric and finite. |
| `actual_wide_df` | The actual/result table used only when forecast values are being compared with known actual/result values. | `None` in forecast-only mode. In forecast-vs-actual mode, a wide table with a `Date` column and destination columns. It may contain extra source months, but the function must check that the required evaluation months can be selected and aligned. |
| `required_months` | The month labels that must be present for the current task. | A list such as `["2023M08", "2023M09"]` for a toy final-forecast check, `["2023M03", ..., "2023M07"]` for the validation period, or all final forecast months for the submitted CSV audit. |
| `required_destinations` | The destination columns that must be present and checked. | If `None`, infer all destination columns in `forecast_wide_df` except `date_column`. If supplied, check exactly those destination names. In the real assignment, use or verify all 20 destination columns from the public dataset. |
| `date_column` | The name of the month-label column. | Normally `"Date"`. The values are month labels such as `2023M08`. |

Your function should support two modes:

1. Forecast-only mode. Use this mode for checking `forecast_submission_wide` before exporting the final forecast CSV. In this mode, `actual_wide_df=None`, so the function checks forecast schema, required months, destination columns, duplicate dates, numeric values, finite values, missing values, extra rows, and extra columns.
2. Forecast-vs-actual mode. Use this mode for validation and evaluation logic. In this mode, both forecast and actual/result tables are supplied, so the function also checks whether they can be aligned by `Date` and destination columns.

Your function should:

- check that the `Date` column exists in the forecast table and, when supplied, the actual/result table;
- check that required destination columns exist;
- check that required months exist;
- check duplicate dates;
- check row counts against the expected number of required months;
- check numeric conversion for forecast values and, when supplied, actual values;
- check finite values;
- check missing values;
- check that forecast and actual/result tables can be safely aligned by `Date` and destination columns when `actual_wide_df` is supplied;
- return an inspectable audit dictionary or DataFrame.

Strictness rules:

- Forecast tables are strict: after subsetting to the required task, the forecast table should contain exactly the required months and required destinations, with no extra rows or extra columns for final submission.
- Actual/result tables used for validation may contain extra months in the source data, but the evaluation must subset them to the required evaluation months before alignment. Missing required actual months remain invalid for evaluation.
- Destination columns used for comparison must match the required destinations.

For this assignment, you should supply `required_months` whenever using this function for validation baselines, improved validation forecasts, or final forecast audit. If `required_destinations=None`, the function should infer all destination columns in `forecast_wide_df` except `date_column`; however, for all-20 assignment evidence you must still ensure the inferred destination set exactly matches the 20 public-dataset destination columns. For the final forecast audit, passing `required_destinations=REQUIRED_DESTINATIONS` is recommended, because this makes missing-destination and extra-column checks explicit. A validator that only checks whether a DataFrame exists is not sufficient.

Suggested audit fields:

```python
{
    "is_valid": True,
    "can_align": True,
    "forecast_row_count": 12,
    "actual_row_count": 12,
    "expected_row_count": 12,
    "missing_months_in_forecast": [],
    "missing_months_in_actual": [],
    "extra_months_in_forecast": [],
    "extra_months_in_actual_source": [],
    "missing_destinations_in_forecast": [],
    "missing_destinations_in_actual": [],
    "extra_columns_in_forecast": [],
    "duplicate_forecast_dates": 0,
    "duplicate_actual_dates": 0,
    "missing_forecast_value_count": 0,
    "missing_actual_value_count": 0,
    "nonnumeric_forecast_value_count": 0,
    "nonnumeric_actual_value_count": 0,
    "nonfinite_forecast_value_count": 0,
    "nonfinite_actual_value_count": 0,
}
```

Output meaning:

- `is_valid` should summarise whether the supplied forecast table satisfies the required schema and value-quality checks for the requested task.
- `can_align` should summarise whether the forecast table and actual/result table can be safely aligned by month and destination. In forecast-only mode, this may be `None`, omitted, or the same as `is_valid` if your documentation makes this clear.
- `forecast_row_count`, `actual_row_count`, and `expected_row_count` should make row-count problems inspectable.
- `missing_*` and `extra_*` fields should identify missing or unexpected months, destinations, or columns rather than only returning `False`.
- `duplicate_*_dates` should count duplicate month labels in the relevant table.
- `missing_*_value_count`, `nonnumeric_*_value_count`, and `nonfinite_*_value_count` should count value-quality problems over the required destination columns and required months after appropriate subsetting.

Your output may be a dictionary, a one-row DataFrame, or another clearly documented audit table. It should not be only a single Boolean, because students and markers need to see why a table passed or failed.

In forecast-only mode, actual-related fields may be `None`, empty lists, or omitted if your audit output remains clear and documented. In forecast-vs-actual mode, extra actual/source months are not necessarily invalid if your function subsets them correctly before alignment.

Illustrative valid forecast-only example:

```python
forecast_wide_df = pd.DataFrame({
    "Date": ["2023M08", "2023M09"],
    "Australia": [12345.0, 12400.0],
    "Japan": [23456.0, 23500.0],
})

validate_forecast_actual_wide(
    forecast_wide_df,
    actual_wide_df=None,
    required_months=["2023M08", "2023M09"],
    required_destinations=["Australia", "Japan"],
)
```

The audit should show `is_valid=True`, correct row counts, no missing months, no missing destinations, no duplicate dates, and numeric finite values.

Illustrative invalid example:

```python
bad_forecast_wide_df = pd.DataFrame({
    "Date": ["2023M08", "2023M08"],
    "Australia": [12345.0, "not_a_number"],
})
```

When checked against required months `["2023M08", "2023M09"]` and destinations `["Australia", "Japan"]`, the audit should flag duplicate `Date` values, missing month `2023M09`, missing destination `Japan`, a nonnumeric forecast value, and `is_valid=False`. If actual/result alignment is requested and cannot be performed, the audit should also report `can_align=False`.

For final evaluation, the teaching team uses the same kind of validation/alignment logic with the submitted final forecast CSV and the assessment-period actual/result table.

### Q3. Forecast accuracy metrics from forecast and actual tables [5 marks]

Your task:

Write a function that computes metrics from a forecast table, an actual/result table, and a training actual table.

This question uses the MASE definition in the [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md). Your implementation must follow that guide, including the treatment of missing denominator pairs, `n_denominator_pairs`, and the rule that official MASE uses `naive_lag=1`.

Required function signature:

```python
def evaluate_forecast_wide(
    forecast_wide_df,
    actual_wide_df,
    training_actual_wide_df,
    start_month,
    end_month,
    required_destinations=None,
    naive_lag=1,
    date_column="Date",
):
    pass
```

Your function should:

- work destination by destination;
- if `required_destinations=None`, evaluate every destination column in `forecast_wide_df` except `date_column`;
- if `required_destinations` is supplied, evaluate exactly those destination columns;
- select evaluation months from `start_month` to `end_month` inclusive;
- align forecast and actual values by `Date` and destination;
- compute MAE, MASE, MAPE, RMSE, denominator, `n`, `n_denominator_pairs`, `mase_available`, and `denominator_warning`;
- compute the MASE numerator as mean absolute forecast error over the evaluated period;
- compute the MASE denominator as the mean absolute `naive_lag`-step difference using all valid lagged pairs in `training_actual_wide_df`, separately for each destination;
- sort the training actual table chronologically by `Date` before computing lagged denominator pairs;
- use `naive_lag=1` for official validation comparisons unless this specification explicitly says otherwise;
- avoid mixing destination histories when computing a destination's MASE denominator;
- use only valid denominator pairs where both lagged training values are numeric and finite;
- handle missing actuals, missing forecasts, zero actuals for MAPE, nonfinite values, and zero or unavailable denominators explicitly;
- return destination-level metrics and an aggregate summary.

Do not fill missing denominator values with zero. Do not use validation actuals or assessment-period actual/result values, which are not provided to students, to repair a missing denominator. Do not use another destination's denominator. If there are no valid denominator pairs, or if the denominator is zero or unavailable, return `NaN` for that destination's MASE or clearly flag it as unavailable. Official validation and final performance use `naive_lag=1` unless explicitly stated otherwise. `lag=12` baseline forecasts are for comparison; they are not the official MASE denominator. Your function must not silently divide by zero.

Denominator window rule: before computing lagged differences for the MASE denominator, sort the training actual table chronologically by `Date`. The denominator uses all valid lagged pairs in chronological order from the supplied training actual history up to the relevant cutoff. Do not restrict the denominator to the same number of months as the evaluated forecast horizon. Report `n_denominator_pairs` so the marker can see how many training lagged pairs were used.

Suggested output:

Destination-level table:

```text
destination
n
mae
mase
mape
rmse
denominator
n_denominator_pairs
mase_available
denominator_warning
```

Aggregate summary:

```text
n_destinations
mean_mase
median_mase
mean_mape
mean_rmse
```

Aggregate metrics should be computed as unweighted summaries of destination-level metrics unless otherwise stated. For example, `mean_mase` is the mean of destination-level MASE values, and `mean_rmse` is the mean of destination-level RMSE values, not a pooled RMSE over all destination-month rows.

The function can return a dictionary such as:

```python
{
    "destination_metrics": destination_metrics_df,
    "aggregate_metrics": aggregate_metrics_dict,
}
```

Illustrative toy numeric example:

Validation forecast table:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `100` | `200` |
| `2023M04` | `100` | `220` |

Validation actual/result table:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `110` | `190` |
| `2023M04` | `130` | `250` |

Training actual table up to cutoff:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2022M12` | `95` | `180` |
| `2023M01` | `105` | `210` |
| `2023M02` | `100` | `200` |

For Australia:

```text
forecast errors = |110 - 100|, |130 - 100| = 10, 30
MAE = 20
denominator = mean(|105 - 95|, |100 - 105|) = mean(10, 5) = 7.5
MASE = 20 / 7.5 = 2.67
```

For Japan:

```text
forecast errors = |190 - 200|, |250 - 220| = 10, 30
MAE = 20
denominator = mean(|210 - 180|, |200 - 210|) = mean(30, 10) = 20
MASE = 20 / 20 = 1.00
```

Aggregate mean MASE:

```text
mean_mase = mean(2.67, 1.00) = 1.84
```

In this toy example, the training table has three rows, so there are two valid lag-1 denominator pairs. In the real assignment, use all valid lag-1 pairs available in the training actual table up to the cutoff.

### Part I integrated evidence

At the end of Part I, use Q1-Q3 together to produce a validation baseline comparison for both `lag=1` and `lag=12`.

For the detailed definition of MASE, denominator handling, missing-value treatment, and worked examples, see the accompanying [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md).

The Part I integrated evidence does not carry separate marks. It is used to verify that the Q1-Q3 functions work together and to provide the baseline evidence required in Q4.

Q1 assesses whether the naive-lag forecast generator works. Q2 assesses whether the forecast/actual audit function works. Q3 assesses whether the metric function works. The integrated evidence shows that these functions work together on the validation period and gives the minimum baseline comparison required for Q4.

Required objects:

```python
training_actual_wide_to_2023M02
validation_actual_wide_2023M03_2023M07
baseline_validation_comparison
baseline_validation_summary
```

Required workflow:

1. Create `training_actual_wide_to_2023M02` and `validation_actual_wide_2023M03_2023M07` from `raw_tourism_data` using the `Date` column. Use `pandas.Period(..., freq="M")` or an equivalent year-month representation for date filtering; do not rely on direct string comparison of month labels.
2. Use `generate_naive_forecast_wide(...)` to generate a `lag=1` validation forecast table for `2023M03` to `2023M07` with cutoff `2023M02`.
3. Use `generate_naive_forecast_wide(...)` to generate a `lag=12` validation forecast table for `2023M03` to `2023M07` with cutoff `2023M02`.
4. Use `validate_forecast_actual_wide(...)` to check that both validation forecast tables can be compared with the validation actual/result table.
5. Use `evaluate_forecast_wide(...)` to calculate MAE, MASE, MAPE, and RMSE for both baselines.
6. Produce `baseline_validation_comparison`, containing destination-level metrics for both baselines.
7. Produce `baseline_validation_summary`, containing aggregate metrics for both baselines.

Both `baseline_validation_comparison` and `baseline_validation_summary` must be visible in the notebook and exported PDF, for example using `display(...)` or a clearly rendered table.

When evaluating both `naive_lag1` and `naive_lag12` baseline forecasts in `baseline_validation_comparison`, use `evaluate_forecast_wide(..., naive_lag=1)` for the official MASE denominator unless the specification explicitly states otherwise. The `lag` column describes the forecast-generation baseline, not the denominator used for official MASE. The comparison must include both baseline labels, all 20 destinations, and validation months `2023M03` to `2023M07`. Aggregate metrics are unweighted summaries of destination-level metrics unless stated otherwise.

Required columns for `baseline_validation_comparison`:

```text
model_label
lag
destination
n
mae
mase
mape
rmse
denominator
n_denominator_pairs
mase_available
denominator_warning
```

Required columns for `baseline_validation_summary`:

```text
model_label
lag
n_destinations
mean_mase
median_mase
mean_mape
mean_rmse
```

Illustrative `baseline_validation_comparison` structure:

| model_label | lag | destination | n | mae | mase | mape | rmse | denominator | n_denominator_pairs | mase_available | denominator_warning |
| --- | ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- | --- |
| `naive_lag1` | `1` | `Australia` | `5` | `31.0` | `4.13` | `...` | `...` | `7.5` | `4` | `True` | `False` |
| `naive_lag1` | `1` | `Japan` | `5` | `...` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |
| `naive_lag12` | `12` | `Australia` | `5` | `...` | `...` | `...` | `...` | `7.5` | `4` | `True` | `False` |
| `naive_lag12` | `12` | `Japan` | `5` | `...` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |

The real `baseline_validation_comparison` table must cover both required baselines and all 20 destinations.

Illustrative `baseline_validation_summary` structure:

| model_label | lag | n_destinations | mean_mase | median_mase | mean_mape | mean_rmse |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| `naive_lag1` | `1` | `20` | `...` | `...` | `...` | `...` |
| `naive_lag12` | `12` | `20` | `...` | `...` | `...` | `...` |

The Part I integrated evidence helps you and the marker check that your baseline generation, table validation, and metric functions work together. It also gives you a minimum comparison point for Q4. If an improved model cannot outperform these simple baselines on validation data, you should discuss why.

## Part II: Forecasting project, evaluation, and interpretation

### Q4. Forecasting workflow and model development [20 marks]

Your task:

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

These scaffold names should remain in the notebook for marking and feedback support. Q4 is assessed on reproducible forecasting evidence, not on forcing every method into the same internal data structure. If a modelling approach does not naturally create one of these intermediate table forms, populate the scaffold with a concise documented evidence object or summary rather than leaving it as `None`.

Your work should:

- create or document `tourism_series_long` as a simple inspectable long-format representation of the public dataset with at least `Date`, `destination`, and `demand` columns for analysis and evidence presentation;
- prepare a modelling-ready dataset in any defensible structure;
- create cutoff-safe modelling inputs, features, or model-ready evidence;
- document any external data in `external_feature_log`, or create a structured empty log if no external data is used;
- build a validation workflow using validation months `2023M03` to `2023M07`;
- compare at least `naive_lag1`, `naive_lag12`, and at least one improved method;
- use MASE with naive lag-1 scaling as the formal validation comparison metric;
- use MAPE, RMSE, and MAE as diagnostic metrics;
- avoid training/validation leakage;
- document external data and cutoff compliance;
- justify model selection using validation evidence;
- support all 20 destination series;
- produce a selected model and final `forecast_submission_wide`.

Any forecasting method is allowed. Higher-quality work is judged by valid design, cutoff safety, evidence, reproducibility, and interpretation, not by using a particular algorithm.

`tourism_series_long` is a scaffold evidence object for analysis and evidence presentation. Your forecasting model does not have to use this table internally. If your model uses another structure, document it in `modelling_data_or_feature_table`. If your method does not use explicit machine-learning-style features, create an inspectable `modelling_data_or_feature_table` object or summary explaining the training data, target series, inputs, transformations, and model-ready structure used. Valid ETS, ARIMA, exponential smoothing, state-space, or other statistical time-series workflows should not be forced into a conventional modelling-table format if that format would misrepresent the method.

### Q5. Final forecast submission and assessment-period performance evaluation [30 marks]

Your task:

Submit a valid wide-format forecast table for all 20 destination series for every month from `2023M08` to `2024M07`.

Required object:

```python
forecast_submission_wide
```

Optional helper object if you use long format internally:

```python
forecast_submission_long
```

Both the visible final forecast table and the submitted CSV are required. In the notebook and exported PDF, show `forecast_submission_wide` or its final rows clearly enough for a marker to inspect the 12 forecast months and destination columns. In CloudDeakin, submit the separate forecast CSV exported directly from the same object.

`forecast_submission_wide` and the final forecast CSV must:

- be visible in the notebook;
- include exactly 12 rows for `2023M08` to `2024M07`;
- include one `Date` column;
- include exactly 20 destination columns matching the public dataset;
- include numeric finite forecast values;
- include no extra columns;
- exclude assessment-period actual/result values, which are not provided to students;
- be exported directly from `forecast_submission_wide` using the required filename pattern.

Before exporting the final CSV, run `validate_forecast_actual_wide(...)` in forecast-only mode and store the result as `forecast_submission_audit`.

Forecast performance for all 20 destinations is used by markers as assessment evidence. Do not include assessment-period actual/result values in your notebook, PDF, or forecast CSV.

Export the final forecast CSV directly from `forecast_submission_wide`:

```python
forecast_submission_wide.to_csv(
    "SIT742-2026T2-A2-<GroupID>-Forecast.csv",
    index=False,
)
```

Use `index=False` when exporting. A CSV containing an unnamed index column will be treated as having an extra column and may fail the schema check.

The teaching-team evaluator will read this CSV as a wide table and compare it with the assessment-period actual/result table after checking the schema.

Illustrative final forecast rows:

| Date | Australia | Japan | `<another destination>` | ... |
| --- | ---: | ---: | ---: | ---: |
| `2023M08` | `12345.67` | `23456.78` | `34567.89` | `...` |
| `2023M09` | `12500.00` | `23600.00` | `34600.00` | `...` |
| `...` | `...` | `...` | `...` | `...` |

Expected effect: the submitted CSV contains forecasts only. The real file must contain all 12 forecast months and all 20 destination columns.

### Q6. Metric discussion and selected-market forecast analysis [20 marks]

Your task has two parts.

Part A: Metric discussion [10 marks]

Use the [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md) to support your discussion of why MASE is the formal metric and how MAPE/RMSE complement it. Use evidence from `baseline_validation_summary`, `validation_metrics`, and your selected model evidence.

Discuss:

- MASE as a scale-free comparison across destinations and the formal metric;
- MASE limitations when the denominator is unstable or based on few valid denominator pairs;
- MAPE as an intuitive percentage error and its instability with zero or small actual values;
- RMSE as a large-error-sensitive diagnostic that is scale-dependent and can be dominated by larger-market series;
- evidence from your validation results, baseline comparison, and selected model.

Part B: Selected-market analysis [10 marks]

Analyse four markets:

- include `Australia`;
- include `Japan`;
- select two additional destinations from the dataset and justify your choice.

For each selected market, discuss:

- historical demand pattern;
- baseline comparison;
- selected model validation behaviour;
- final forecast shape and plausibility;
- uncertainty, limitations, and risks.

Include at least one supporting plot or table for each selected market. Do not imply that assessment-period actual/result values are available in the public dataset.

No separate object named `selected_market_analysis` is required. Complete Q6 in the written-answer cell and place any supporting plots or tables near the relevant discussion so they appear in the exported PDF.

## Part III: Group video and collaboration presentation

### Q7. Group video and collaboration explanation [15 marks]

Your group video is a required component of Assignment 2.

The video should:

- be 8-12 minutes long unless an approved alternative arrangement exists;
- include every group member, unless the unit chair has approved an alternative arrangement;
- show code execution;
- explain the problem, data, model, validation, forecast output, interpretation, reproducibility, individual contributions, and collaboration.

Follow the final CloudDeakin Assignment instructions for the accepted video format, submission channel, peer/self-review workflow, and any alternative-arrangement process.

Expected evidence: the video should show the notebook running or key cells being executed, identify the submitted forecast CSV, and let each member explain a meaningful part of the submitted work.

## Back matter

### Submission requirements

Submit the required files in CloudDeakin via Assignments located under Assessment.

Required files:

```text
SIT742-2026T2-A2-<GroupID>.ipynb
SIT742-2026T2-A2-<GroupID>.pdf
SIT742-2026T2-A2-<GroupID>-Forecast.csv
SIT742-2026T2-A2-<GroupID>-Video.<approved format or link>
```

If your group uses external data, also include:

```text
external_data/
```

Use this folder for reproducible snapshots of external data files where the data licence permits redistribution. The notebook should read submitted external data using relative paths such as `external_data/<filename>`, not absolute local paths.

Replace `<GroupID>` with the group identifier specified in CloudDeakin or by the teaching team.

Follow the CloudDeakin Assignment requirements for peer/self-review, contribution declarations, and any additional submission fields.

For general student help with CloudDeakin assignment submissions, refer to:

https://www.deakin.edu.au/students/help/about-clouddeakin/assessment/assignments

### Forecast CSV format

Your final forecast CSV must use the required wide format:

| Column | Meaning |
| --- | --- |
| `Date` | Month being forecast, formatted as `YYYYMmm`, such as `2023M08`. |
| Each of the 20 destination columns | Numeric forecast value for that destination-month. Column names must match the public dataset exactly. |

The CSV should contain 12 rows: one row for each month from `2023M08` to `2024M07`.

Do not include `model_label`, confidence intervals, diagnostic metrics, comments, notes, or extra columns in the final forecast CSV.

Use `index=False` when exporting from `forecast_submission_wide`. A CSV containing an unnamed index column will be treated as having an extra column and may fail the schema check.

### External data and cutoff rules

You may use public external data, web-scraped data, or manually collected public reference data if all of the following hold:

- the source is public or otherwise verifiable by the teaching team;
- the data is cited;
- the retrieval process is documented;
- the data is cutoff-safe;
- the data is reproducible through submitted files, stable public URLs, pinned versions, or clear retrieval instructions;
- no credentials, private data, restricted datasets, or unverifiable sources are included.

Cutoff rules:

- For validation, all demand data and external data used to create validation forecasts must be available no later than `2023M02`.
- For the final submitted `2023M08`-`2024M07` forecasts, all demand data and external data used by the forecasting workflow must be available no later than `2023M07`.

Do not use data that only became available after the relevant fixed cutoff. The task is not a rolling-origin or nowcasting task.

Recommended submission approach:

1. Include a snapshot of each external data file in a submitted `external_data/` folder, where redistribution is permitted.
2. Read those files with relative paths in the notebook so the teaching team can rerun your workflow.
3. If you also use GitHub or another public URL, use a stable raw URL or pinned commit/release URL where possible.
4. If the data licence does not allow redistribution, do not include the file. Instead, provide retrieval code, the exact public source URL or citation, and enough evidence for the teaching team to verify the data.

Document every external source in `external_feature_log`, including source URL or citation, local submitted filename or public URL, retrieval method, feature names, transformation, licence/access notes, latest observation used, and availability evidence.

Suggested `external_feature_log` fields:

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

### Video requirement

The Assignment 2 group video is compulsory and worth 15 marks. All group members should participate unless the unit chair has approved an alternative arrangement.

The final unit-site or CloudDeakin instructions will specify accepted formats and submission details. Make sure the video clearly shows code execution and explains how the submitted outputs were produced.

### Reproducibility requirements

Your group should ensure that:

- the notebook runs from top to bottom in a clean runtime;
- required functions, object names, and written-answer markers are preserved;
- public data is loaded from the approved raw URL or documented relative local paths;
- any external data source is cited, cutoff-safe, and available through the submitted `external_data/` folder or a stable public source;
- random seeds are set where relevant;
- package requirements are documented where relevant;
- figures and tables are labelled clearly;
- final forecast CSV output can be regenerated directly from `forecast_submission_wide`;
- no credentials, private paths, private data, or restricted links are included.

### Academic integrity and GenAI policy

Refer to the current SIT742 unit site and official Deakin pages for academic integrity, GenAI, extensions, late submission, special consideration, and related assessment requirements.

Do not replace current Deakin or unit-site policy with independently drafted policy statements. Follow the current unit-site instructions for permitted tools, acknowledgement requirements, collaboration rules, and peer/self-review requirements.

### Extensions, special consideration, and submission policy

For current Deakin information about assessments, including assignment submission, extensions, late penalties, special consideration, and related support, refer to:

https://www.deakin.edu.au/students/study-support/assessments-and-examinations/assessments

The unit site and CloudDeakin Assignment page are the source of truth for final submission instructions.

### Submission checklist

Before submitting, check that:

- `GROUP_INFO` is complete;
- all members are listed with Deakin email and email username;
- all required files are included;
- filenames follow the required group pattern;
- required headings, functions, objects, and written-answer markers are preserved;
- the [A2 MASE Guide](SIT742-2026T2-A2-MASE-Guide.md) has been used for MASE denominator handling, missing-value treatment, and validation-metric calculations;
- the notebook runs from top to bottom;
- the PDF export includes relevant outputs, figures, tables, and explanations;
- `baseline_validation_comparison` includes both `lag=1` and `lag=12` baselines;
- `baseline_validation_comparison` includes `mase_available` and `denominator_warning`;
- `baseline_validation_summary` includes aggregate metrics for both baselines;
- validation evidence covers `2023M03` to `2023M07` and includes MASE, MAPE, RMSE, and MAE;
- `forecast_submission_wide` has one `Date` column and exactly the 20 destination columns;
- `forecast_submission_wide` covers exactly `2023M08` to `2024M07`;
- `forecast_submission_audit` checks `forecast_submission_wide` in forecast-only mode before export;
- the final forecast CSV is exported directly from `forecast_submission_wide` using the required filename pattern;
- the final forecast CSV is exported with `index=False`;
- the final forecast CSV has exactly 12 rows and no extra columns;
- all forecast values are numeric and finite;
- all datasets and external resources are acknowledged;
- external data snapshots are included in `external_data/` where required and permitted;
- the video is included and follows the CloudDeakin Assignment requirements;
- peer/self-review or contribution material follows the CloudDeakin Assignment requirements;
- the notebook can regenerate the final forecast CSV;
- no credentials, private data, private paths, or restricted links are included.
