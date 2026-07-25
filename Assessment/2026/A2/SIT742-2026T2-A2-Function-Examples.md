# SIT742 2026T2 Assignment 2 Function Examples Guide

Release level: student-facing

This guide explains the expected input and output shapes for the three Part I functions in Assignment 2. It complements the [A2 Specification](SIT742-2026T2-A2-Specification.md), the [A2 Starter Notebook](SIT742-2026T2-A2-Starter.ipynb), and the [A2 Forecast Measures Guide](SIT742-2026T2-A2-Forecast-Measures-Guide.md).

The examples below are illustrative only. Your real submission must work for the public `ISF-TDF2023` dataset, all 20 destinations, the required validation months, and the final forecast period.

## Common Table Format

The Part I functions use pandas DataFrames in wide monthly format.

```text
Date, Australia, Japan, <other destination columns>
2023M03, 123.0, 456.0, ...
2023M04, 124.0, 458.0, ...
```

Use one row per month. The `Date` column stores labels such as `2023M03`. Destination columns store numeric values. This is not a long table with separate `destination` and `demand` columns.

For month arithmetic, convert labels such as `2023M12` and `2024M01` to a month-aware representation such as `pandas.Period(..., freq="M")`. Do not subtract strings.

## Q1. `generate_naive_forecast_wide(...)`

Required signature:

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

Expected inputs:

| Input | Meaning |
| --- | --- |
| `historical_actual_wide` | Pandas DataFrame containing a wide actual table with `Date` plus destination columns. |
| `cutoff_label` | Last month whose actual values can be used. |
| `forecast_months` | Month labels to forecast, in the requested output order. |
| `required_destinations` | Destination columns to forecast. If `None`, use all columns except `date_column`. |
| `lag` | Positive integer lag. `lag=1` is the previous-month naive baseline; `lag=12` is the same-month-previous-year baseline. |
| `date_column` | Month-label column, normally `"Date"`. |

Required return value:

- a pandas DataFrame;
- one row for each requested forecast month, in the supplied `forecast_months` order;
- columns in the exact order `date_column`, followed by the inferred or supplied destination order;
- numeric forecast values where a source value is available;
- no index column, `model_label`, diagnostics, or other extra columns.

Lag rule:

For forecast month `m`, use source month `m - lag`.

- If the source month is at or before `cutoff_label`, use the observed actual value.
- If the source month is after `cutoff_label` but has already been forecast, use that generated forecast recursively.
- If no observed or generated source value is available, return `NaN` or raise a clear error.

Toy input:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2022M03` | `70` | `180` |
| `2022M04` | `75` | `190` |
| `2022M05` | `80` | `200` |
| `2022M06` | `85` | `210` |
| `2022M07` | `90` | `220` |
| `2023M01` | `95` | `230` |
| `2023M02` | `100` | `240` |

Call with `lag=1`:

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

Explanation: `2023M03` uses observed `2023M02`. `2023M04` then uses the generated `2023M03` forecast, and `2023M05` uses the generated `2023M04` forecast. This is recursive because future actual values are not available at the cutoff.

Call with `lag=12`:

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

Explanation: `lag=12` uses the same month from the previous year. For example, `2023M03` uses `2022M03`.

## Q2. `validate_forecast_actual_wide(...)`

Required signature:

```python
validate_forecast_actual_wide(
    forecast_wide_df,
    actual_wide_df=None,
    required_months=None,
    required_destinations=None,
    date_column="Date",
)
```

Expected inputs:

| Input | Meaning |
| --- | --- |
| `forecast_wide_df` | Pandas DataFrame containing the wide forecast table to validate. |
| `actual_wide_df` | `None` for forecast-only mode, or a pandas DataFrame containing a wide actual/result table for forecast-vs-actual alignment. |
| `required_months` | Month labels that must be present for the current task. |
| `required_destinations` | Destination columns that must be present. If `None`, infer forecast destination columns except `date_column`. |
| `date_column` | Month-label column, normally `"Date"`. |

Forecast-only mode checks whether a forecast table is ready for export or later evaluation. Forecast-vs-actual mode also checks whether forecast and actual/result tables can align by `Date` and destination columns.

The function must return a Python dictionary. A Boolean-only result, DataFrame, renamed keys, or omitted keys does not satisfy the Q2 return contract.

Required audit dictionary shape for forecast-only mode:

```python
{
    "is_valid": True,
    "can_align": None,
    "forecast_row_count": 12,
    "actual_row_count": None,
    "expected_row_count": 12,
    "missing_months_in_forecast": [],
    "missing_months_in_actual": None,
    "extra_months_in_forecast": [],
    "extra_months_in_actual_source": None,
    "missing_destinations_in_forecast": [],
    "missing_destinations_in_actual": None,
    "extra_columns_in_forecast": [],
    "duplicate_forecast_dates": 0,
    "duplicate_actual_dates": None,
    "missing_forecast_value_count": 0,
    "missing_actual_value_count": None,
    "nonnumeric_forecast_value_count": 0,
    "nonnumeric_actual_value_count": None,
    "nonfinite_forecast_value_count": 0,
    "nonfinite_actual_value_count": None,
}
```

The dictionary must always contain all 20 keys in the displayed order.

Required field meanings:

| Field | Type | Meaning |
| --- | --- | --- |
| `is_valid` | `bool` | Whether every applicable schema, coverage, and value-quality check passes. A missing `date_column` makes this `False`. |
| `can_align` | `bool` or `None` | Whether required forecast and actual rows can align by month and destination. Use `None` in forecast-only mode. |
| `forecast_row_count` | `int` | Number of rows in the supplied forecast table. |
| `actual_row_count` | `int` or `None` | Number of actual/result rows after subsetting to `required_months`; `None` in forecast-only mode. |
| `expected_row_count` | `int` or `None` | Number of `required_months`, or `None` when no required-month list is supplied. |
| `missing_months_in_forecast` | `list[str]` | Required months absent from the forecast table, in required-month order. |
| `missing_months_in_actual` | `list[str]` or `None` | Required months absent from the actual source; `None` in forecast-only mode. |
| `extra_months_in_forecast` | `list[str]` | Forecast months not in `required_months`, in source order. |
| `extra_months_in_actual_source` | `list[str]` or `None` | Actual-source months not in `required_months`, in source order; `None` in forecast-only mode. Extra actual-source months are allowed when the required subset aligns. |
| `missing_destinations_in_forecast` | `list[str]` | Required destination columns absent from the forecast table, in required-destination order. |
| `missing_destinations_in_actual` | `list[str]` or `None` | Required destination columns absent from the actual table; `None` in forecast-only mode. |
| `extra_columns_in_forecast` | `list[str]` | Forecast columns other than `date_column` and the required destinations, in source-column order. |
| `duplicate_forecast_dates` | `int` | Duplicate forecast-date occurrences after the first occurrence. |
| `duplicate_actual_dates` | `int` or `None` | Duplicate actual-date occurrences after the first within the required-month subset; `None` in forecast-only mode. |
| `missing_forecast_value_count` | `int` | Missing cells among present required destination columns and required forecast-month rows. |
| `missing_actual_value_count` | `int` or `None` | Missing cells among present required destination columns after actual-month subsetting; `None` in forecast-only mode. |
| `nonnumeric_forecast_value_count` | `int` | Non-missing forecast cells in scope that cannot be converted to numeric values. |
| `nonnumeric_actual_value_count` | `int` or `None` | Non-missing actual cells in scope that cannot be converted to numeric values; `None` in forecast-only mode. |
| `nonfinite_forecast_value_count` | `int` | Successfully converted, non-missing forecast values in scope that are positive or negative infinity. |
| `nonfinite_actual_value_count` | `int` or `None` | Successfully converted, non-missing actual values in scope that are positive or negative infinity; `None` in forecast-only mode. |

List fields use `[]` when the relevant check was performed and found no issue. Missing destination columns are reported by name and are not converted into synthetic missing-cell counts. If `required_months=None`, evaluate row, date, destination, and value quality using the supplied forecast months, and use `expected_row_count=None`. If `required_destinations=None`, infer the forecast destination columns except `date_column`.

Valid forecast-only example:

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

Invalid forecast-only example:

```python
bad_forecast_wide_df = pd.DataFrame({
    "Date": ["2023M08", "2023M08"],
    "Australia": [12345.0, "not_a_number"],
})

validate_forecast_actual_wide(
    bad_forecast_wide_df,
    actual_wide_df=None,
    required_months=["2023M08", "2023M09"],
    required_destinations=["Australia", "Japan"],
)
```

The audit should flag:

- duplicate `Date` values;
- missing month `2023M09`;
- missing destination `Japan`;
- one nonnumeric forecast value;
- `is_valid=False`.

Forecast-vs-actual example with an extra source month:

```python
actual_wide_df = pd.DataFrame({
    "Date": ["2023M07", "2023M08", "2023M09"],
    "Australia": [12200.0, 12350.0, 12420.0],
    "Japan": [23200.0, 23480.0, 23540.0],
})

alignment_audit = validate_forecast_actual_wide(
    forecast_wide_df,
    actual_wide_df=actual_wide_df,
    required_months=["2023M08", "2023M09"],
    required_destinations=["Australia", "Japan"],
)
```

For this example, `actual_row_count=2`, `extra_months_in_actual_source=["2023M07"]`, `can_align=True`, and `is_valid=True`. Extra source months are acceptable because the two required months can be subset and aligned. Missing required actual months are not acceptable.

## Q3. `evaluate_forecast_wide(...)`

Required signature:

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

Expected inputs:

| Input | Meaning |
| --- | --- |
| `forecast_wide_df` | Pandas DataFrame containing the wide forecast table for the evaluation months. |
| `actual_wide_df` | Pandas DataFrame containing a wide actual/result table for the same evaluation months, or a source table that can be subset to them. |
| `training_actual_wide_df` | Pandas DataFrame containing wide actual history available up to the relevant cutoff, used only for the MASE denominator. |
| `start_month`, `end_month` | Inclusive evaluation period. |
| `required_destinations` | Destination columns to evaluate. If `None`, use forecast destination columns except `date_column`. |
| `naive_lag` | Lag used for the MASE denominator. Official validation uses `naive_lag=1` unless the specification states otherwise. |
| `date_column` | Month-label column, normally `"Date"`. |

### How MAE Is Calculated

Mean Absolute Error (MAE) summarises forecast error in the destination series' original unit. For each destination, align forecast and actual/result values by `Date` within the inclusive evaluation period. Keep only rows where both values are numeric and finite.

```text
absolute_error_t = |actual_t - forecast_t|

n = number of valid aligned forecast-actual rows

mae = mean(absolute_error_t) across those n rows
```

The `mae` value:

- is calculated separately for each destination;
- is the average absolute forecast error in the original unit of that destination series;
- is not a percentage and is not scaled;
- is used directly as the numerator of `mase`;
- must be `NaN` or clearly unavailable when `n=0`.

MAE is supporting calculation evidence. The required performance measures remain MASE and MAPE, so aggregate output does not require `mean_mae`.

### Required Return Structure

Return exactly:

```python
destination_metrics, aggregate_metrics
```

`destination_metrics` must be a pandas DataFrame. `aggregate_metrics` must be a Python dictionary.

### Destination-Level Output

`destination_metrics` must contain these columns in this order:

| Field | Meaning |
| --- | --- |
| `destination` | Destination column evaluated. |
| `n` | Number of valid aligned forecast-actual rows used for MAE and the MASE numerator. |
| `mae` | Mean absolute error across those `n` rows, in the destination series' original units. |
| `mase` | `mae` divided by the destination's MASE denominator. |
| `mape` | Mean absolute percentage error over valid rows with nonzero actual values. |
| `denominator` | Mean absolute lagged change from the cutoff-safe training actual history. |
| `n_denominator_pairs` | Number of valid training-history pairs used in `denominator`. |
| `mase_available` | Whether a valid MASE value could be calculated. |
| `denominator_warning` | Whether the MASE denominator was missing, zero, or otherwise unavailable. |

`aggregate_metrics` must contain exactly these keys in this order:

```text
n_destinations
mean_mase
median_mase
mean_mape
```

Toy forecast table:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `100` | `200` |
| `2023M04` | `100` | `220` |

Toy actual/result table:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `110` | `190` |
| `2023M04` | `130` | `250` |

Toy training actual table:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2022M12` | `95` | `180` |
| `2023M01` | `105` | `210` |
| `2023M02` | `100` | `200` |

For Australia:

```text
forecast errors = |110 - 100|, |130 - 100| = 10, 30
n = 2
MAE = mean(10, 30) = (10 + 30) / 2 = 20 original units per month
MASE denominator = mean(|105 - 95|, |100 - 105|) = mean(10, 5) = 7.5
MASE = 20 / 7.5 = 2.67
MAPE = mean(10 / 110, 30 / 130) * 100 = 16.08%
```

For Japan:

```text
forecast errors = |190 - 200|, |250 - 220| = 10, 30
n = 2
MAE = mean(10, 30) = (10 + 30) / 2 = 20 original units per month
MASE denominator = mean(|210 - 180|, |200 - 210|) = mean(30, 10) = 20
MASE = 20 / 20 = 1.00
MAPE = mean(10 / 190, 30 / 250) * 100 = 8.63%
```

Destination-level output for the toy example:

| destination | n | mae | mase | mape | denominator | n_denominator_pairs | mase_available | denominator_warning |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | --- | --- |
| Australia | `2` | `20.00` | `2.67` | `16.08` | `7.50` | `2` | `True` | `False` |
| Japan | `2` | `20.00` | `1.00` | `8.63` | `20.00` | `2` | `True` | `False` |

Both destinations have `mae=20`, but the MASE values differ because their historical movement differs. Australia has `mase=2.67`, while Japan has `mase=1.00`. This shows why MAE is useful calculation evidence but MASE is more suitable for comparing performance across destinations.

Aggregate output for the toy example:

| n_destinations | mean_mase | median_mase | mean_mape |
| ---: | ---: | ---: | ---: |
| `2` | `1.83` | `1.83` | `12.36` |

Calculate aggregate measures from unrounded destination-level values, then round only for display. Use all valid lagged pairs in the supplied training history. Do not restrict the MASE denominator to the same number of months as the forecast horizon. Do not fill unavailable denominators with zero. For MAE, do not replace missing forecast or actual values with zero. For MAPE, do not divide by zero; exclude zero-actual rows and report the effect clearly.

## Part I Integrated Evidence

Use Q1-Q3 together to create baseline validation evidence for both `naive_lag1` and `naive_lag12`.

Required workflow:

1. Create `training_actual_wide_to_2023M02` and `validation_actual_wide_2023M03_2023M07` from `raw_tourism_data`.
2. Generate a `lag=1` validation forecast table for `2023M03` to `2023M07` with cutoff `2023M02`.
3. Generate a `lag=12` validation forecast table for `2023M03` to `2023M07` with cutoff `2023M02`.
4. Validate both forecast tables against `validation_actual_wide_2023M03_2023M07`.
5. Evaluate both baselines with `evaluate_forecast_wide(..., naive_lag=1)`.
6. Display `baseline_validation_comparison`.
7. Display `baseline_validation_summary`.

Required `baseline_validation_comparison` columns:

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

Illustrative structure:

| model_label | lag | destination | n | mae | mase | mape | denominator | n_denominator_pairs | mase_available | denominator_warning |
| --- | ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: | --- | --- |
| `naive_lag1` | `1` | `Australia` | `5` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |
| `naive_lag1` | `1` | `Japan` | `5` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |
| `naive_lag12` | `12` | `Australia` | `5` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |
| `naive_lag12` | `12` | `Japan` | `5` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |
| `...` | `...` | `<remaining destinations>` | `...` | `...` | `...` | `...` | `...` | `...` | `...` | `...` |

Required `baseline_validation_summary` columns:

```text
model_label
lag
n_destinations
mean_mase
median_mase
mean_mape
```

Illustrative structure:

| model_label | lag | n_destinations | mean_mase | median_mase | mean_mape |
| --- | ---: | ---: | ---: | ---: | ---: |
| `naive_lag1` | `1` | `20` | `...` | `...` | `...` |
| `naive_lag12` | `12` | `20` | `...` | `...` | `...` |

Both tables must be visible in the notebook and exported PDF. This evidence does not carry separate marks, but it demonstrates that Q1-Q3 work together and provides the baseline comparison required for Q4.
