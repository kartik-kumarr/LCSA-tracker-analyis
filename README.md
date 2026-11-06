# LCSA Tracker — survey analysis

Analysis code for the user evaluation of the Life Cycle Sustainability Assessment
(LCSA) Tracker prototypes (Tracker, Mapping and Geolocation). The survey was run in
Qualtrics with respondents from Australia, India and Vietnam.

This repository contains the notebook used to produce all tables and figures in the
paper and its supplementary material.

## Contents

| File | Description |
|---|---|
| `LCSA_tracker_supplementary.ipynb` | Full analysis, from the raw Qualtrics export to every table and figure |
| `tables/` | Output tables (CSV), created when the notebook runs |
| `figures/` | Output figures, each saved as SVG (vector) and PNG (600 dpi), created when the notebook runs |
| `cache/` | Cached model outputs (sentiment and feature coding), created on the first run |

## What the notebook does

1. **Loading and cleaning:** removes the Qualtrics header rows, then derives
   region, age band, expertise (expert / non-expert), gender and prior exposure.
2. **Sample description and missing data:** describes the sample composition,
   missingness per item and missingness by subgroup.
3. **Sentiment scoring:** converts open-text answers to a 0–100 score using an
   ensemble of three pre-trained transformer models, and checks how well the
   models agree.
4. **Missing-data treatment:** analyses the answers people actually gave
   (available-case) as the primary analysis, and uses multiple imputation
   (m = 20, pooled with the D2 rule) as a sensitivity check.
5. **Inferential statistics:**
   - assumption checks and reliability (Cronbach's α);
   - group comparisons: Kruskal–Wallis or Mann–Whitney tests with Cliff's δ or
     ε², and Holm correction;
   - Dunn post-hoc tests, Spearman correlations and sensitivity checks.
6. **Dimension reduction (exploratory):** PCA and factor analysis.
7. **Qualitative analysis:** keyword and zero-shot transformer coding of
   suggested new components, comparing experts and non-experts (Fisher's exact,
   permutation test, Cohen's κ and Cohen's h).
8. **Tracker, Mapping and Geolocation:**
   - ratings of each feature (Friedman and Wilcoxon tests);
   - coding of the suggested improvements;
   - how the prototypes help respondents understand the LCSA indicators;
   - box plots and density plots of the eight metrics reported in the paper.

## Requirements

We ran the analysis with Python 3.13 and the following packages:

```
pandas==2.2.3
numpy==2.1.3
scipy==1.16.3
scikit-learn==1.6.1
statsmodels==0.15.0
matplotlib==3.10.0
seaborn==0.13.2
transformers==5.20.1
torch==2.11.0
```

Optional packages: `scikit-posthocs`, `factor_analyzer`, `plotly`, `spacy` and
`wordcloud`. If one is missing, the cells that need it are skipped.

```bash
pip install -r requirements.txt
```

## How to run

1. Place the Qualtrics CSV export next to the notebook, or set
   `CONFIG["data_file"]` in Part 0.
2. Open the notebook and run all cells from top to bottom (*Kernel → Restart &
   Run All*).
3. On the first run, the sentiment and zero-shot models are downloaded from
   Hugging Face (about 2 GB) and their scores are cached in `cache/`. Later
   runs load the cache. Delete `cache/` to recompute the scores.

All random steps use `CONFIG["random_seed"]`, so the results are the same on
every run. The last cell records the software versions used.

## Data

The raw survey data are not included in this repository because they contain
participants' free-text answers. <!-- Edit: e.g. "Data are available from the
authors on reasonable request." -->

## Citation

If you use this code, please cite:

<!-- Add the paper reference once published -->

## Licence

<!-- e.g. MIT -->
