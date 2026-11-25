# Exploratory Data Analysis of House Prices (Ames Housing)

An exploratory data analysis (EDA) of the Kaggle **House Prices: Advanced Regression Techniques**
dataset (`train.csv`, 1,460 houses, 81 columns). The goal is to understand which factors drive
sale price using statistics and visualisation only. **No machine learning is used.**

## Guiding questions

1. How is `SalePrice` distributed? Is it skewed?
2. Which numerical features correlate most strongly with `SalePrice`?
3. Does `OverallQual` change price in a clear, ordered way?
4. Do some neighborhoods consistently cost more than others?
5. Are there outliers, and are they errors or real but unusual sales?
6. Which columns have missing values, and which of those are not truly missing?

## Key findings

- `SalePrice` is right skewed (skewness 1.88; mean $180,921 vs median $163,000). A log transform
  brings skewness to 0.12.
- The strongest numeric correlates of price are `OverallQual` (r = 0.79) and `GrLivArea` (r = 0.71).
- Median price rises at every quality grade, from $50,150 (grade 1) to $432,390 (grade 10).
- NridgHt, NoRidge and StoneBr are clearly the most expensive neighborhoods.
- Two very large, low-priced houses (Id 524 and Id 1299, both "Partial" sales) were removed as
  non-typical sales. Other large, expensive houses are real and were kept.
- 15 of the 19 columns with missing values mean "feature absent" (no pool, alley, garage, ...) and
  were filled with `"None"` rather than dropped.

See the final report for the evidence and charts.

## Project files

| File | Description |
|------|-------------|
| `EDA_House_Prices_Final_Report.pdf` | Full report with results, figures and answers to the six questions |
| `EDA_House_Prices_Notebook.ipynb` | Jupyter notebook with all code, with explanation cells before and after each step |
| `eda_analysis.py` | Same analysis as a single script; saves figures and the cleaned data |
| `EDA_House_Prices_Explained_and_Implemented.pdf` | Step-by-step explanation of the method, with code |
| `eda_dashboard.png` | 2x2 summary dashboard |
| `train_cleaned.csv` | Cleaned dataset (1,458 rows, no missing values) |
| `requirements.txt` | Python dependencies |

`train.csv` is **not** included (see Data below).

## Data

Download `train.csv` from the Kaggle competition page
(<https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data>) and place it in the
same folder as the notebook and script. A free Kaggle account is required, and the data is subject
to Kaggle's competition terms.

## Setup

```bash
# 1. (Recommended) create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt
```

## How to run

**Option A: notebook**

```bash
jupyter notebook EDA_House_Prices_Notebook.ipynb
```

Choose **Run All**. The notebook has no saved outputs, so tables and charts appear when you run it.

**Option B: script**

```bash
python eda_analysis.py
```

This writes to the current folder:

- `figs/`: all figures as PNG files
- `eda_dashboard.png`: the summary dashboard
- `train_cleaned.csv`: the cleaned data
- `results.json`: the numbers used in the report

## Method (10 steps)

1. Setup and first look (shape, types, missing values)
2. Classify variables (numerical, categorical, ordinal)
3. Descriptive statistics (mean, median, mode, spread, skewness, kurtosis)
4. Distributions (histogram, KDE, boxplot, log transform)
5. Correlation and covariance (Pearson and Spearman)
6. Scatter plots and pair plots
7. Correlation heatmap
8. Outlier detection (IQR rule and z-score)
9. Data quality checks and missing-value handling
10. Final dashboard and conclusions

## Cleaning decisions and assumptions

- Duplicates: none found. Inconsistent category labels: none found.
- `PoolQC`, `MiscFeature`, `Alley`, `Fence`, `FireplaceQu`, the garage, basement and `MasVnrType`
  columns: missing means the feature is absent, so filled with `"None"`.
- `LotFrontage`: filled with the median of the same neighborhood.
- `MasVnrArea` filled with 0 and `GarageYrBlt` filled with `YearBuilt`. These are **assumptions**
  and affect only those two columns.
- `OverallQual` is stored as a number but treated as ordinal in interpretation.

## Limitations

- Correlation is not causation; size, quality and neighborhood are intertwined.
- Pearson correlation captures only linear relationships and is sensitive to outliers.
- Some groups are small (`OverallQual` 1, 2 and 10; neighborhoods such as Blueste), so their
  statistics are unstable.
- One city over a limited period, so results may not generalise.
- No predictive model was built, so no predictive accuracy is claimed.

## Tools

Python, pandas, NumPy, Matplotlib, Seaborn, SciPy, Jupyter Notebook.
