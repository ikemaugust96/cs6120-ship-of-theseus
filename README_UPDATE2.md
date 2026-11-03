# Update 2: Environment and Execution

## Scope

Notebook: `notebooks/02_update2.ipynb`

- Human-origin POS and dependency-depth changes across T0-T3.
- PCA trajectories and full-feature style dispersion.
- Seven-class original-source attribution.
- Four-family paraphraser identification.
- Bootstrap uncertainty and selected sensitivity analyses.

Source labels identify original source categories, not individual authors.

## Recorded environment

- Python: 3.13.1
- Recorded platform: Darwin arm64
- spaCy: 3.8.16
- English pipeline: en_core_web_sm 3.8.0
- scikit-learn: 1.9.1
- Random seed: 6120
- Bootstrap replicates: 2,000

`requirements-update2.txt` pins installed direct dependencies and the
spaCy pipeline. It is not a complete transitive dependency lockfile.
Keep the existing repository dependency specifications for other notebooks.

## Installation

Run the following commands from the project root in a Python environment
matching the recorded Python version:

    python -m pip install -r requirements-update2.txt
    python -m pip check
    python -m spacy validate
    python -m ipykernel install --user --name cs6120-update2 --display-name "CS6120 Update 2"

Select **CS6120 Update 2** as the notebook kernel.

The requirements file installs the exact spaCy pipeline.
To install that pipeline separately in the same environment:

    python -m pip install "https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl"

## Raw data and directory layout

The project directory and corpus directory are siblings:

    cs6120-group-project/
        cs6120-ship-of-theseus/
            pyproject.toml
            notebooks/
            data/
            experiments/
        Ship_of_theseus_paraphrased_copus/
            paraphrased_datasets/

The raw-data directory must contain:

- `cmv_paraphrased.csv`
- `eli5_paraphrased.csv`
- `sci_gen_paraphrased.csv`
- `tldr_paraphrased.csv`
- `wp_paraphrased.csv`
- `xsum_paraphrased.csv`
- `yelp_paraphrased.csv`

Corpus repository:
https://github.com/tripto03/Ship_of_theseus_paraphrased_copus

Update 2 trains classifiers using splits of the paraphrased files.
It does not use the corpus's separately supplied training CSVs.

Authoritative clean-chain identity manifest:

`data/metadata/shared_sample_manifest.csv`

This manifest contains 16,659 originals. Each has 22 versions:
one T0 plus seven configurations times three rewrite rounds.
The complete feature table therefore contains 366,498 records.

## Feature caches

Complete feature table:

`experiments/update2/style_features_full_clean_v1/full_style_features.csv`

Resumable extraction checkpoints:

`experiments/update2/style_features_full_clean_v1/chunks/`

Reusable Update 1 per-text feature caches:

`experiments/style_features_spacy_v1/<domain>/`

Cell 2 loads and validates the complete table when available.
Otherwise, it reads raw data, reuses eligible cached records and
checkpoints, and extracts missing features. Preserve these caches
to avoid repeating expensive extraction.

## Current splits

- `experiments/update2/group_split_full_clean_v1.csv`
- `experiments/update2/original_split_full_clean_v1.csv`
- `experiments/update2/split_summary_full_clean_v1.csv`

All source variants and rewrites of a `(domain, key)` group stay together.
Groups are assigned within domains using approximately 70/15/15 splits.

| Split      | Groups | Originals | Human originals |
| ---------- | -----: | --------: | --------------: |
| Train      |  2,092 |    11,620 |           1,637 |
| Validation |    447 |     2,496 |             359 |
| Test       |    455 |     2,543 |             355 |

Historical split location:

`experiments/update2/archive/legacy_12523_split/`

Historical manifests are excluded from the current analysis and bundle.

## Saved models

Original-source classifier:

`experiments/update2/source_attribution/source_model.joblib`

Stores the fitted pipeline, features, labels, selected C and random seed.
Training and validation use T0 only. The fixed model evaluates held-out
T0-T3 texts.

Paraphraser-family classifier:

`experiments/update2/paraphraser_fingerprints/syntax_fingerprint_model.joblib`

Stores the fitted pipeline, features, family mapping, selected C and
weighting description. Training uses rewritten training records.
Evaluation balances families and configurations within each family.

## PCA artifacts

Files under `experiments/update2/identity_trajectory/`:

- `pca_feature_settings.csv`: training means, scales and PC1/PC2 loadings.
- `pca_explained_variance.csv`: component variance ratios.
- `human_test_pca_coordinates.csv`: held-out two-dimensional coordinates.
- `human_test_standardized_features.npz`: standardized features, identities
  and fitted transformation arrays.

Standardization and PCA use training Human T0 only.
PC1 and PC2 explain approximately 37.4% of training variance.
Dispersion uses the selected standardized feature space, not just PC1/PC2.

## Execution

1. Select the correct notebook kernel.
2. Confirm the manifest and raw data or complete feature cache exist.
3. Run analysis cells 1-10 in order.
4. Run cell 11 for fingerprint uncertainty and feature associations.
5. Run cell 12 for refined PCA figures.
6. Run documentation and historical-file archival cells, if present.
7. Save the notebook before running the packaging cell.
8. Save again after packaging; rebuild if the saved notebook changed.

A full run loads existing features but still fits classifiers and
computes bootstrap analyses. Cells 11 and 12 reuse saved artifacts
without retraining or refitting when their required inputs are available.

Saved outputs from multiple sessions do not establish a clean-kernel run.

## Outputs

Analysis directories under `experiments/update2/`:

- `stylometry/`
- `identity_trajectory/`
- `source_attribution/`
- `paraphraser_fingerprints/`

Figures: `figures/update2/`

Review bundle: `update2_review_bundle.zip`

Include this README and `requirements-update2.txt` in the packaging code.
The review bundle otherwise selects notebook, CSV tables and figures.
Model joblib files, NPZ arrays, raw data and the complete feature cache
are not automatically included. Preserve them separately.
The review bundle alone is not a complete execution environment.

## Interpretation limits

- POS distance is not calibrated semantic loss or author-identity loss.
- PCA centroid movement does not measure distribution contraction.
- Bootstrap intervals are pointwise without multiplicity correction.
- Classifier intervals condition on the fitted model.
- Dispersion intervals condition on the training transformation.
- The domain-conditioned independence reference is descriptive,
  not a retrained permutation test.
- Coefficients describe model associations, not causal linguistic rules.
- Background-normalized POS change exceeds SBERT change in departure and
  average T1-to-T3 growth across all seven configurations. This supports
  faster relative change under the selected measures and references,
  not a universal difference between stylistic and semantic decay.
