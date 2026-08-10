# SIT742 Practical Dataset Catalogue

This directory contains data and supporting artefacts used across SIT742 practical and self-learning notebooks.

## Current Dependency Snapshot

The static dependency scan recorded in [dataset-manifest.json](dataset-manifest.json) checks whether each filename appears in the 35 current notebooks.

| Classification | Files |
|---|---:|
| Filename referenced by at least one current notebook | 37 |
| No current filename reference detected | 29 |
| Files at least 5 MiB | 11 |
| Large files with no current filename reference detected | 10 |

A missing filename reference does not prove that a file is unused: notebooks may load a renamed download, extract a member from an archive, or describe an optional activity. Such files are review candidates, not deletion candidates.

All 66 files in this snapshot are intentionally retained in their current paths. This catalogue classifies them only; it does not authorise deletion, deduplication, or externalisation. Any later disposition requires source, licence, and dependency review for the individual file.

## Large Files

| File | Approximate size | Current filename reference |
|---|---:|---|
| `review.csv` | 93.67 MiB | none detected |
| `business_review_submission.zip` | 34.59 MiB | none detected |
| `item_listing_category.zip` | 24.88 MiB | none detected |
| `sf_crime_train.csv` | 21.88 MiB | none detected |
| `mnist_saved_model.zip` | 20.62 MiB | none detected |
| `kddcup.data.gz` | 17.28 MiB | none detected |
| `emnist.digits_letters.small.csv` | 16.73 MiB | none detected |
| `meta-review-business.csv` | 9.35 MiB | none detected |
| `names.zip` | 6.95 MiB | none detected |
| `2020T2Data.csv` | 6.70 MiB | none detected |
| `shakespeare.txt` | 5.08 MiB | M04 Spark and WordCount notebooks |

## Duplicate Files

The current byte-level scan found:

- `Generic_Transactions_db.txt` and `Transactions_db.txt` are identical;
- `baby_name_2008.csv` and `baby_name_2008.txt` are identical;
- `kddcup.data_10_percent.gz` and `kddcup.gz` are identical;
- `stopwords_en.txt` is identical to `Assessment/2019/data/stopwords.txt`.

Paths remain unchanged because notebook and historical dependencies require review before deduplication.

## Provenance and Licence Status

Dataset provenance and licensing are not yet complete at file level. A repository licence must not be assumed to override a source dataset's terms.

Before reuse or redistribution:

1. identify the original source;
2. verify the source licence and attribution requirements;
3. check whether the file contains restricted, personal, or platform-derived data;
4. record the result in the manifest;
5. use the notebook's public URL or approved local path.

See [LICENSING.md](../../LICENSING.md) for repository-wide guidance.
