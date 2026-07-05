# SIT742 2026T2 Assignment 2 MASE Guide

Release level: student-facing

This guide explains how to calculate and interpret MASE for Assignment 2. Use it as the practical reference for the detailed definition of MASE, denominator handling, missing-value treatment, and worked examples when computing validation metrics in your notebook.

This guide accompanies the [A2 Specification](SIT742-2026T2-A2-Specification.md).

## 1. Purpose of MASE in Assignment 2

Mean Absolute Scaled Error, or MASE, evaluates forecast accuracy in a scale-adjusted way.

In Assignment 2:

- MASE is the formal forecast-performance metric.
- Lower MASE is better.
- MASE is computed separately for each destination and then averaged across destinations where required.
- MASE helps compare forecasts across destinations with different demand scales.
- MAPE and RMSE may be used as reference diagnostics, but MASE is the formal metric.

Performance bands and thresholds are not published.

## 2. Forecast Table And Actual/Result Table Format

MASE is easiest to explain destination by destination. In Assignment 2, the required final forecast CSV uses wide format, matching the original `ISF-TDF2023` dataset.

The required final forecast table in your notebook is:

```python
forecast_submission_wide
```

It must use this structure:

```text
Date, Australia, Japan, ..., <remaining destination columns>
2023M08, <forecast>, <forecast>, ...
2023M09, <forecast>, <forecast>, ...
...
2024M07, <forecast>, <forecast>, ...
```

For metric calculation, the evaluator conceptually compares:

- one forecast table;
- one actual/result table;
- one training actual table for the MASE denominator.

These tables are aligned by `Date` and destination column. The tables may be converted internally for calculation, but the submitted final forecast CSV must remain wide format.

Validation evaluation compares a student-generated validation forecast table with visible validation actual/result values. Final evaluation compares the submitted final forecast CSV with assessment-period actual/result values, which are not provided to students. The calculation logic is the same; only the actual/result table differs.

Forecast tables are strict: after subsetting to the required task, the forecast table should contain exactly the required months and required destinations, with no extra rows or extra columns for final submission. Actual/result tables used for validation may contain extra months in the source data, but the evaluation must subset them to the required evaluation months before alignment. Missing required actual months remain invalid for evaluation.

## 3. MASE Formula Used In A2

Assignment 2 uses MASE with naive lag-1 scaling:

```text
MASE = mean_absolute_forecast_error / mean_absolute_training_one_step_change
```

where:

```text
mean_absolute_forecast_error =
mean(|actual_t - forecast_t|) over the evaluated forecast period

mean_absolute_training_one_step_change =
mean(|training_actual_t - training_actual_{t-1}|) over all valid lag-1 pairs in the training period available up to the cutoff
```

For validation MASE:

- The numerator uses validation actual values for `2023M03` to `2023M07`.
- The denominator uses only training actual values available up to `2023M02`.

For final evaluation MASE:

- The teaching-team evaluator uses assessment-period actual/result values, which are not provided to students, for `2023M08` to `2024M07` when calculating the numerator.
- The denominator uses only public historical actual values available up to `2023M07`.

Do not use validation actual values or assessment-period actual/result values to construct forecasts.

Official MASE uses `naive_lag=1` unless the specification explicitly says otherwise. A `lag=12` naive forecast is useful as a baseline comparison, but it is not the official MASE denominator.

### Denominator window: how much training history is used?

Unless otherwise stated, use all valid lagged pairs from the training actual history available up to the relevant cutoff. Do not restrict the denominator to the same number of months as the forecast horizon.

Before computing lagged differences for the MASE denominator, sort the training actual table chronologically by `Date`. The denominator uses all valid lagged pairs in chronological order from the supplied training actual history up to the relevant cutoff. Do not assume the input table rows are already sorted.

For validation, the denominator uses all valid lag-1 pairs available up to `2023M02`. For final evaluation, the denominator uses all valid lag-1 pairs available in the public historical actual series up to `2023M07`.

If the validation horizon has five forecast months but the training history contains 60 monthly observations up to `2023M02`, then the denominator should normally use up to 59 valid lag-1 pairs, not only five pairs.

## 4. Step-By-Step Single-Destination Example

Training actuals up to cutoff:

```text
2022M10 = 80
2022M11 = 90
2022M12 = 95
2023M01 = 105
2023M02 = 100
```

Validation actuals:

```text
2023M03 = 110
2023M04 = 130
2023M05 = 125
2023M06 = 140
2023M07 = 150
```

Forecasts:

```text
2023M03 = 100
2023M04 = 100
2023M05 = 100
2023M06 = 100
2023M07 = 100
```

Calculate the numerator:

```text
|110 - 100| = 10
|130 - 100| = 30
|125 - 100| = 25
|140 - 100| = 40
|150 - 100| = 50

forecast MAE = mean(10, 30, 25, 40, 50) = 31
```

Calculate the denominator:

```text
|90 - 80| = 10
|95 - 90| = 5
|105 - 95| = 10
|100 - 105| = 5

training one-step denominator = mean(10, 5, 10, 5) = 7.5
```

Calculate MASE:

```text
MASE = 31 / 7.5 = 4.13
```

A MASE of 4.13 means the average forecast error is 4.13 times the average one-step month-to-month movement in the training history. In this toy example, the average forecast error is much larger than the average one-step movement in the training history.

## 5. Wide-Table Example

Suppose your validation forecast table is:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `100` | `200` |
| `2023M04` | `100` | `220` |

The validation actual/result table is:

| Date | Australia | Japan |
| --- | ---: | ---: |
| `2023M03` | `110` | `190` |
| `2023M04` | `130` | `250` |

The training actual table up to `2023M02` is:

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

The aggregate MASE is the mean of the destination-level MASE values:

```text
mean_mase = mean(2.67, 1.00) = 1.84
```

The destination-level output should include the denominator, the number of valid denominator pairs used for each destination, `mase_available`, and `denominator_warning`.

## 6. Multi-Destination Calculation

Assignment 2 has 20 destination series.

For each destination:

1. Align the forecast table and actual/result table by `Date`.
2. Select the evaluated months.
3. Compute forecast MAE over the evaluated months.
4. Compute the naive lag-1 denominator using all valid chronologically sorted lag-1 pairs in that destination's training actual history available up to the relevant cutoff.
5. Compute destination-level MASE.

Then aggregate:

```text
all_20_mase = mean(destination_mase values across the 20 destinations)
```

Each destination receives equal weight in the aggregate unless the specification later states otherwise. Aggregate metrics should be computed as unweighted summaries of destination-level metrics unless otherwise stated. For example, `mean_mase` is the mean of destination-level MASE values, and `mean_rmse` is the mean of destination-level RMSE values, not a pooled RMSE over all destination-month rows.

## 7. Relation To Naive Lag Baselines

There are two related ideas:

- the baseline forecast model;
- the MASE scaling denominator.

Baseline model:

```text
forecast for a future month = value from a selected lag in the available history
```

For `lag=1`, the baseline uses the most recent previous month. Under a fixed-cutoff multi-step horizon, this usually becomes a recursive carry-forward baseline. For example, if the cutoff value is `2023M02 = 100`, then validation baseline forecasts for `2023M03` to `2023M07` may all be `100` unless your group uses and clearly justifies a different non-leaking design.

For `lag=12`, the baseline uses the same month from the previous year when available. For validation months `2023M03` to `2023M07`, `lag=12` uses `2022M03` to `2022M07` values.

A `lag=12` forecast is a baseline model. It does not change the official MASE denominator. The official MASE denominator uses `naive_lag=1` unless the specification explicitly states otherwise. When evaluating both `naive_lag1` and `naive_lag12` baseline forecasts in `baseline_validation_comparison`, use `evaluate_forecast_wide(..., naive_lag=1)` for the official MASE denominator unless the specification explicitly states otherwise. The `lag` column describes the forecast-generation baseline, not the denominator used for official MASE.

MASE denominator:

```text
mean absolute one-step changes in the training actual history
```

The official denominator is not calculated from the forecast values. It is calculated from historical actual values available before the cutoff, using `naive_lag=1`.

## 8. Handling Missing Values And Edge Cases

Use practical and transparent rules:

- Ignore rows where actual or forecast values are missing when calculating the forecast-error numerator, but report how many rows were used.
- Nonfinite forecast values should be treated as invalid unless explicitly handled and justified.
- If the denominator is zero or cannot be computed, return `NaN` or a clear flag rather than silently dividing by zero.
- Report `n_denominator_pairs` for each destination.
- Report `mase_available` and `denominator_warning` for denominator availability and zero-denominator edge cases.
- MAPE should handle zero actual values carefully, because division by zero is undefined.
- RMSE should be computed on the same evaluated rows used for MAE where possible.

### Missing values in the MASE denominator

When computing the MASE denominator, first sort the training actual table chronologically by `Date`, then use only valid lagged pairs from the training actual history.

For `naive_lag=1`, each denominator pair is:

```text
(training_actual_t, training_actual_{t-1})
```

A pair is valid only if both values are numeric and finite. If either value is missing, nonnumeric, or nonfinite, exclude that pair from the denominator calculation and report how many valid denominator pairs were used.

Do not fill missing denominator values with zero. Do not use validation actuals or assessment-period actual/result values, which are not provided to students, to repair the denominator. Do not use another destination's denominator.

Toy denominator example:

```text
Training actuals:
2022M10 = 80
2022M11 = missing
2022M12 = 95
2023M01 = 105
2023M02 = 100

For naive_lag=1, possible pairs are:
2022M11 - 2022M10: invalid because 2022M11 is missing
2022M12 - 2022M11: invalid because 2022M11 is missing
2023M01 - 2022M12: |105 - 95| = 10
2023M02 - 2023M01: |100 - 105| = 5

denominator = mean(10, 5) = 7.5
valid_denominator_pairs = 2
```

If there are no valid denominator pairs for a destination, MASE for that destination should be returned as `NaN` or clearly flagged as unavailable. The function must not silently divide by zero.

## 9. Recommended Python Pseudocode

This is pseudocode, not a complete evaluator.

In this pseudocode, `sort_by_month_label(...)` should sort with a real year-month representation such as `pandas.Period(..., freq="M")`, not ambiguous lexicographic string sorting.

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
    if required_destinations is None:
        required_destinations = [
            column for column in forecast_wide_df.columns if column != date_column
        ]

    aligned_forecast, aligned_actual = align_by_date_and_destination(
        forecast_wide_df,
        actual_wide_df,
        start_month,
        end_month,
        required_destinations,
        date_column,
    )

    destination_metrics = []
    training_actual_wide_df = sort_by_month_label(training_actual_wide_df, date_column)

    for destination in required_destinations:
        forecast = to_numeric_array(aligned_forecast[destination])
        actual = to_numeric_array(aligned_actual[destination])
        training_actual = to_numeric_array(training_actual_wide_df[destination])

        valid_error_rows = rows_where_actual_and_forecast_are_finite
        errors = actual[valid_error_rows] - forecast[valid_error_rows]
        mae = mean(abs(errors))
        rmse = sqrt(mean(errors ** 2))

        valid_mape_rows = valid_error_rows and actual != 0
        mape = mean(abs(errors[valid_mape_rows] / actual[valid_mape_rows])) * 100

        # Use all valid lagged pairs in the supplied training history,
        # not only a window matching the forecast horizon.
        left = training_actual[naive_lag:]
        right = training_actual[:-naive_lag]

        valid_denominator_pairs = rows_where_left_and_right_are_finite
        diffs = left[valid_denominator_pairs] - right[valid_denominator_pairs]
        denominator = mean(abs(diffs))
        n_denominator_pairs = len(diffs)

        if n_denominator_pairs == 0 or denominator <= 0 or denominator is missing:
            mase = NaN
            denominator_warning = True
            mase_available = False
        else:
            mase = mae / denominator
            denominator_warning = False
            mase_available = True

        destination_metrics.append({
            "destination": destination,
            "mase": mase,
            "mape": mape,
            "rmse": rmse,
            "mae": mae,
            "denominator": denominator,
            "n_denominator_pairs": n_denominator_pairs,
            "mase_available": mase_available,
            "denominator_warning": denominator_warning,
            "n": number_of_valid_error_rows,
        })

    aggregate_metrics = summarise_destination_metrics(destination_metrics)

    return {
        "destination_metrics": destination_metrics,
        "aggregate_metrics": aggregate_metrics,
    }
```

## 10. Common Mistakes

Before submitting, check that you are not:

- using the GitHub page URL in `read_csv` instead of a raw CSV URL;
- mixing destinations together before computing MASE;
- using validation actuals in the denominator;
- using assessment-period actual/result values in your forecast workflow;
- using `lag=12` as the official MASE denominator;
- restricting the MASE denominator to the forecast-horizon length when the specification asks for all valid training lagged pairs;
- submitting a final CSV with `model_label` or diagnostic columns;
- submitting a final CSV in the wrong table shape;
- computing MASE on misaligned forecast and actual/result tables;
- treating missing evaluation values as zero;
- reporting only MAPE/RMSE and omitting MASE.

## Reference

Hyndman, R.J. and Koehler, A.B. (2006). Another look at measures of forecast accuracy. International Journal of Forecasting, 22(4), 679-688. https://doi.org/10.1016/j.ijforecast.2006.03.001
