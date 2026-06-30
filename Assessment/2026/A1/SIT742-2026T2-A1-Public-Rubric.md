# SIT742 2026T2 Assignment 1 Public Rubric

## Purpose

This student-facing rubric summarises the public assessment expectations for Assignment 1. It should be read together with the Assignment Specification and completed using the Starter Notebook.

## Mark summary

| Part | Questions | Focus | Marks |
| --- | --- | --- | ---: |
| Part I | Q1-Q6 | Python programming tasks | 30 |
| Part II | Q7-Q11 | HK2012-2018 data analysis and critical interpretation | 60 |
| Part III | Q12 | Transfer and reflection | 10 |
|  |  | Total | 100 |

## How to read this rubric

This rubric summarises the public assessment expectations for each question. It should be read together with the Assignment Specification and completed using the Starter Notebook.

The mark allocations below show how each question is broadly assessed. They summarise public expectations rather than listing every possible test case, assessment scenario, or partial-credit case.

Communication, reproducibility, notebook clarity, figure/table labelling, dataset acknowledgement, and avoidance of local absolute paths are assessed within the relevant questions. They are not a separate additional mark category.

## Part I: Python Programming Tasks [30 marks]

### Q1: `normalise_column_labels(labels)` [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and return structure | 1 | Preserves the required function name and returns `(cleaned_labels, label_map)`. | Function is mostly assessable but return structure is incomplete or hard to inspect. | Function name/signature is changed or output is only printed. |
| Label cleaning rules | 2 | Correctly strips whitespace, lowercases labels, and replaces internal whitespace with underscores. | Handles most cleaning rules but misses one important case. | Cleaning rules are mostly missing or inconsistent. |
| Duplicate handling and order preservation | 1 | Preserves input order and applies deterministic suffixes such as `_2`, `_3` for duplicate cleaned labels. | Preserves order but duplicate handling is incomplete. | Duplicate handling or ordering is not yet sufficiently consistent. |
| Generality and clarity | 1 | Works for valid inputs beyond the toy example and is easy to read. | Works for common cases but has limited generality or clarity. | Appears hard-coded to the example or is difficult to assess. |

### Q2: `validate_monthly_date_range(date_values, start, end)` [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and return dictionary | 1 | Preserves the required function name and returns a dictionary with the required keys. | Dictionary is useful but one key or field meaning is missing. | Function name/signature is changed or output is unstructured. |
| Monthly parsing and range construction | 1 | Parses valid date-like values to months and constructs the expected monthly range. | Handles common month formats but has minor parsing issues. | Monthly date parsing or comparison is not yet sufficiently clear or complete. |
| Missing and extra month detection | 2 | Correctly identifies missing expected months and extra months outside the required range. | Detects either missing or extra months but not both consistently. | Monthly coverage validation does not yet provide enough evidence. |
| `n_periods` and output formatting | 1 | Counts distinct parsed months consistently and formats month lists as `YYYY-MM` strings. | Output is mostly understandable but inconsistent in formatting or duplicate handling. | `n_periods` or month-list outputs are unclear or incorrect. |

### Q3: `make_tidy_arrival_table(market_frames)` [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and input handling | 1 | Accepts a dictionary of market DataFrames and preserves all provided markets. | Handles only simple cases or some markets inconsistently. | Does not accept the required input structure. |
| Required tidy columns | 2 | Returns a pandas DataFrame with clear `date`, `market`, and `arrival` columns. | Produces a usable table but column names or structure need adjustment. | Required tidy columns are missing or output is not a DataFrame. |
| Market labelling and reproducible order | 1 | Adds market names from dictionary keys and produces a reproducible row order. | Market labels or row order are partly inconsistent. | Market identifiers are missing or not sufficiently consistent. |
| Link to Part II workflow | 1 | Output can be used directly as, or as the basis for, the Part II `tidy_arrivals` object after cleaning. | Output is partly reusable but needs further restructuring. | Output is disconnected from the required Part II tidy-arrival workflow. |

### Q4: `summarise_numeric_columns(df, exclude=("date",))` [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and exclusion handling | 1 | Preserves the required function name and correctly excludes requested columns. | Exclusion works for common cases but is incomplete. | Required signature is changed or exclusions are ignored. |
| Numeric convertibility assessment | 2 | Correctly identifies numeric and safely convertible columns using column-level evidence. | Identifies obvious numeric columns but misses convertible or problematic cases. | Numeric assessment is not yet sufficiently clear or complete. |
| Summary fields | 1 | Includes clear fields such as `column`, `can_convert_numeric`, `n_missing`, `n_unique`, and `example_value`. | Some useful fields are present but required evidence is incomplete. | Output lacks the specified summary structure. |
| Reusability for schema/SII analysis | 1 | Output supports schema audit and SII feature identification. | Output is partly useful but needs manual interpretation. | Output is difficult to reuse in Part II. |

### Q5: `rolling_mean_manual(values, window)` [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and output length | 1 | Preserves the required function name and returns a list with the same length as the input. | Output is mostly usable but length or type is inconsistent. | Required signature is changed or no usable list is returned. |
| Trailing rolling-window logic | 2 | Correctly computes trailing means using the current value and previous `window - 1` values. | Computes a rolling-style value but alignment is partly wrong. | Rolling calculation is not implemented in a way that can be assessed reliably. |
| Early positions and invalid windows | 1 | Returns `None` before a full window and raises `ValueError` for non-positive windows. | Handles either early positions or invalid windows, but not both. | Early positions and invalid windows are not handled. |
| Manual implementation | 1 | Uses a manual implementation as required, rather than relying on pandas or NumPy rolling helpers. | Logic is mostly manual but partly unclear. | Relies mainly on pandas or NumPy rolling helpers rather than the required manual implementation. |

### Q6: `detect_local_extrema(values)` [5 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Function signature and return structure | 1 | Preserves the required function name and returns `{"peaks": [...], "valleys": [...]}`. | Return structure is mostly usable but incomplete. | Required signature or return format is missing. |
| Strict peak detection | 1 | Correctly identifies strict local peaks using immediate neighbours. | Detects some peaks but includes errors. | Peak detection is not yet sufficiently clear or complete. |
| Strict valley detection | 1 | Correctly identifies strict local valleys using immediate neighbours. | Detects some valleys but includes errors. | Valley detection is not yet sufficiently clear or complete. |
| Endpoint, plateau, and index handling | 2 | Excludes endpoints and plateaus, uses zero-based indices, and returns positions rather than values. | Handles some of these conditions but misses one or more edge cases. | Counts endpoints/plateaus incorrectly or returns unusable outputs. |

## Part II: HK2012-2018 Dataset Analysis and Critical Interpretation [60 marks]

### Q7: Data access and schema audit [10 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Data loading and required markets | 2 | Loads all three required market files using the approved raw URL base and clear market-file mapping. | Loads most required data or has minor reproducibility issues. | Required data is missing, loaded inconsistently, or depends on local absolute paths, private files, or undocumented files. |
| Required objects | 2 | Creates clear `raw_market_data` and `schema_audit` objects with the required structure. | Objects are present but incomplete or hard to inspect. | Required objects are missing, left as `None`, or have incorrect types. |
| Schema checks | 3 | Checks rows/columns, date range, date ordering, missingness, numeric parsing, and column issues for all required markets. | Includes some checks but misses important evidence or one market is less complete. | Schema audit is minimal or does not support later analysis. |
| Explanation, source acknowledgement, and reproducibility | 3 | Explains audit findings, acknowledges the dataset source, labels the audit clearly, and avoids local absolute paths, private files, or undocumented dependencies. | Explanation or acknowledgement is limited, or reproducibility evidence is incomplete. | Explanation is absent, source is not acknowledged, or the workflow depends on local absolute paths, private files, or undocumented dependencies. |

### Q8: Cleaning and reproducible analysis tables [10 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Label cleaning and traceability | 2 | Cleans labels consistently and documents original-to-cleaned mapping. | Label cleaning is present but traceability is limited. | Label cleaning is missing, unclear, or not reproducible. |
| Date parsing, sorting, and coverage | 2 | Parses and sorts dates and verifies monthly coverage from 2012-01 to 2018-12 for each required market. | Date handling is mostly correct but coverage evidence is incomplete. | Date handling or monthly coverage checking is not yet sufficiently clear or complete. |
| Numeric conversion and cleaning decisions | 2 | Converts numeric fields appropriately and explains key cleaning decisions. | Numeric conversion is attempted but documentation is limited. | Numeric conversion decisions are missing or unclear. |
| Required cleaned/tidy objects | 3 | Creates complete `clean_market_data` and `tidy_arrivals`, retaining arrival and original SII-related numeric columns where needed. | Objects are present but incomplete, hard to locate, or partly inconsistent. | Required objects are missing, left as `None`, or not suitable for later analysis. |
| Reproducible workflow | 1 | Cleaning workflow can be followed and rerun from the notebook. | Workflow is mostly reproducible but has minor gaps. | Workflow depends on unclear steps, undocumented dependencies, or files not provided through the approved workflow. |

### Q9: EDA and cross-market comparison [15 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Time-series comparison figure | 3 | Provides a clear, labelled time-series comparison figure for the three required markets. | Figure is present but labelling or comparison clarity is limited. | Figure is missing, unclear, or not comparative. |
| Annual summary and yearly comparison | 3 | Creates a useful `annual_summary` and compares yearly arrival patterns across markets. | Annual summary is present but comparison is limited. | Annual summary is missing or not usable for comparison. |
| Growth or first-to-last change | 2 | Provides a clear growth or first-to-last change comparison. | Growth/change evidence is present but limited. | Growth/change comparison is missing or unsupported. |
| Volatility and seasonality/month-level patterns | 3 | Compares volatility and seasonality or month-level patterns across markets. | Some volatility or seasonal evidence is present but incomplete. | Volatility/seasonality comparison is missing or unclear. |
| Evidence-based written comparison | 4 | Written comparison clearly links claims to figures, tables, or computed summaries and compares all three markets. | Written comparison is mostly descriptive or only partly linked to evidence. | Written comparison is generic, unsupported, or not meaningfully comparative. |

### Q10: Feature engineering and SII analysis [15 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Arrival-derived features | 4 | Creates all required derived fields and computes change, rolling, volatility, and local-extrema features within each market. | Most required features are present but one or more are incomplete or inconsistently grouped. | Required features are largely missing or computed across markets incorrectly. |
| SII feature identification and selection rule | 3 | Identifies original SII features from cleaned data and uses a transparent, reproducible selection rule. | SII features are identified but the rule is weak or only partly explained. | Feature selection is unclear, untraceable, or uses non-SII/derived fields. |
| `selected_sii_features` list | 2 | Provides at least three selected SII feature names that match columns in the cleaned original data. | List is present but incomplete or traceability is unclear. | List is missing, uses placeholders, or names do not match cleaned data columns. |
| `sii_summary` and cross-market relationship comparison | 3 | Creates a clear `sii_summary` covering selected features and required markets, and compares SII-arrival relationships across markets. | Summary is present but coverage of selected features, required markets, or relationship comparison is limited. | Summary is missing, incomplete, or not interpretable. |
| Interpretation and causality caution | 3 | Explains what SII relationships suggest, what they do not prove, and avoids causal or forecasting overclaims. | Interpretation is present but caution or evidence linkage is limited. | Treats correlation/co-movement as causation or makes unsupported claims. |

### Q11: Critical interpretation, limitations, and project-planning insight [10 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Main findings grounded in evidence | 3 | Summarises key findings using specific evidence from generated tables, figures, or features. | Findings are plausible but only partly tied to evidence. | Findings are generic or unsupported. |
| Interpretation of SII signals | 2 | Explains what SII signals can and cannot support. | Discusses SII signals but limitations are only partly clear. | Overstates SII evidence or ignores its limitations. |
| Limitations of data and descriptive analysis | 2 | Discusses concrete limitations tied to the dataset, descriptive methods, or generated evidence. | Limitations are present but general. | Limitations are missing or not relevant. |
| Project-planning insight | 2 | Identifies what data quality, feature design, validation, or modelling issues a later forecasting project would need to handle. | Future-work discussion is present but underdeveloped. | Project-planning insight is missing or not connected to the submitted analysis. |
| Clarity and evidence linkage | 1 | Written interpretation is clear and refers to relevant tables/figures/features. | Some links to evidence are present but unclear. | Writing is difficult to follow or not linked to submitted outputs. |

## Part III: Transfer and Reflection [10 marks]

### Q12: Transfer and reflection [10 marks]

| Criterion | Marks | High achievement | Partial achievement | Needs improvement |
| --- | ---: | --- | --- | --- |
| Transferability of Part I functions | 2 | Clearly identifies which functions transfer directly and which need adaptation. | Identifies some functions but adaptation reasoning is limited. | Part I function transfer is not yet discussed in enough detail. |
| Transferability of Part II workflow/objects | 2 | Explains how Part II objects or workflow stages could map to a multi-destination monthly tourism-demand dataset. | Mentions some workflow elements but links are incomplete. | Links between the Part II workflow and the new dataset are not yet clear. |
| Dataset differences | 2 | Discusses at least two relevant differences from the public ISF-TDF2023 README. | Mentions differences but with limited detail or relevance. | Dataset differences are missing or generic. |
| Pre-reuse checks | 2 | Explains sensible checks before reusing the A1 workflow on the new dataset. | Some checks are mentioned but not well connected to transfer. | Pre-reuse checking is absent or vague. |
| Transfer risk or limitation | 2 | States a clear risk or limitation of transferring without redesign. | Risk is present but underdeveloped. | Risk or limitation is missing. |

## Communication and reproducibility across the assignment

Communication and reproducibility are assessed within the relevant questions, especially Q7-Q11. They are not a separate additional mark category.

Across the assignment, stronger submissions generally:

- run from top to bottom in the submitted notebook;
- keep required object names and outputs easy to locate;
- use labelled figures and tables;
- connect written explanations to generated evidence;
- acknowledge the dataset source;
- avoid local absolute paths, private files, credentials, or undocumented dependencies.

## Video evidence

Video evidence is not a separate scored component and does not replace the notebook and exported PDF. It may support High Distinction-level evidence or verification where relevant, according to the Assignment Specification and unit-site instructions.
