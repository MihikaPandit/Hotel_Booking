# Group_<95>_A1_Code — README

## What this package contains
```
Group_<95>_A1_Code/
├── 01_dataset_audition.ipynb   # Lightweight audit and comparison of all 3 datasets
├── 02_data_readiness.ipynb     # Cleaning + stakeholder framing + EDA for the selected dataset
├── hotel_bookings_clean.csv    # Final cleaned dataset (output of 02_data_readiness.ipynb)
└── README.md                   # This file
```

Per the assignment instructions, the raw datasets supplied via Canvas
(`hotel_bookings.csv`, `airline_delay.csv`, `train_occupancy.csv`) are **not**
duplicated in this ZIP.

## Dependencies
- Python 3.10+
- `pandas >= 2.0`
- `numpy`
- `matplotlib`

Install with:
```
pip install pandas numpy matplotlib
```

## File locations / setup
Place all three raw CSVs (`hotel_bookings.csv`, `airline_delay.csv`,
`train_occupancy.csv`) in the **same folder** as the two notebooks before
running (or update the `RAW_DATA_PATH` variables at the top of each
notebook if your paths differ) which is present in cell 2. Both notebooks use relative paths only.

## Run order
Run notebooks in numeric order, each top to bottom (`Run All`):

### 1. `01_dataset_audition.ipynb`
Profiles all three raw datasets at a comparable, lightweight level (size,
variable types, missingness) and produces the 3-way comparison table with
the evidence-based justification for selecting `hotel_bookings.csv`.
**Output:** `hotel_bookings_clean.csv`.

### 2. `02_data_readiness.ipynb`
The detailed pipeline for the selected dataset only:
- Loads `hotel_bookings.csv`
- Initial profiling (missingness, dtypes)
- Cleaning: duplicates -> missing values -> category standardisation ->
  data types -> implausible values -> feature engineering
- Validation checks (automated `assert` statements)
- Saves `hotel_bookings_clean.csv` and `cleaning_log.csv`
- Stakeholder problem and success criteria
- Exploratory Data Analysis on the cleaned data, including 3 purposeful
  visualisations (cancellation rate by deposit type, Distribution by ADR,
  cancellation rate by market segment) and target-leakage identification
- Assignment 2 plan (target, features, modelling directions, risks)

**Outputs:** `hotel_bookings_clean.csv`, `cleaning_log.csv`.

Both notebooks are idempotent: re-running from scratch on the raw data
always reproduces the same cleaned CSV, log, and charts.

## Expected outputs (when both notebooks are run in order)
| File | Produced by | Description |
|---|---|---|
| `eda_comparison_table.csv` | `01_dataset_audition.ipynb` | 3-dataset comparison table |
| `hotel_bookings_clean.csv` | `02_data_readiness.ipynb` | Cleaned dataset — 88,492 rows x 33 columns (from 119,987 x 32 raw) |
| `cleaning_log.csv` | `02_data_readiness.ipynb` | Row/column counts before and after each cleaning step |
| `Distribution by ADR ` | `02_data_readiness.ipynb` | Bar Chart : Distribution by ADR|
| `Cancellation rate chart by hotel type` | `02_data_readiness.ipynb` | Boxplot: lead time by hotel type |
| `Cancellation rate chart by market segment` | `02_data_readiness.ipynb` | Bar chart: cancellation rate by market segment |

## Random seeds
The Assignment 1 used `random_state=42` for reproducibility.

## Generative AI use
Claude (Anthropic, Sonnet 5) and ChatGPT (GPT-5.6, OpenAI)  was used to assist with drafting notebook code, cleaning
logic, and report text. All outputs were reviewed and verified against the
actual dataset by both group members before submission.
