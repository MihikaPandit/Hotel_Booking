# Group_95_A2_Code README

Hotel booking cancellation prediction.
This package supports the report `Group_95_A2_Report.pdf`. It builds a
majority-class baseline and two required supervised models (Logistic Regression and Decision Tree), plus a Random Forest extension, and compares them on the same held-out test set.

## What this package contains
```
Group_95_A2_Code/
├── 01_data_pipeline.ipynb          # Stage 1 cleaning pipeline (from Assignment 1) -> hotel_bookings_clean.csv
├── 02_modelling_evaluation.ipynb   # Modelling, evaluation, threshold analysis, bootstrap, experiment log
├── hotel_bookings_clean.csv        # Cleaned dataset (88,132 rows x 33 columns)
├── requirements.txt                # Python dependencies
├── README.md                       # This file
└── outputs/                        # Created automatically when the modelling notebook runs
```

The raw Canvas dataset (`hotel_bookings.csv`) is **not** duplicated in this ZIP. It is only needed if you want to regenerate the cleaned CSV from scratch.

## Dependencies
- Python 3.10+
- `pandas >= 2.0`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn >= 1.2` (required for `OneHotEncoder(min_frequency=..., handle_unknown="infrequent_if_exist", sparse_output=...)`)
- `jupyter` / `ipykernel`

Install with:
```
pip install -r requirements.txt
```
or
```
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## File locations / setup
All paths are relative. Place `hotel_bookings_clean.csv` in the **same folder**
as the modelling notebook. All results are written to an `outputs/` folder,
which the notebook creates if it does not exist.

## Run order
Run each notebook top to bottom (`Run All`).

### 1. `01_data_pipeline.ipynb` (optional if the cleaned CSV is included)
Reproduces the Stage 1 cleaning of `hotel_bookings.csv` (119,987 x 32 raw) into
`hotel_bookings_clean.csv` (88,132 x 33). The modelling notebook only needs the
cleaned CSV, so this step can be skipped when the CSV is already present.

### 2. `02_modelling_evaluation.ipynb`
| Section | Content |
|---|---|
| 1. Setup | Imports, global constants (seed 42, 20% test size, F1 as primary metric) |
| 2. Load data | Loads `hotel_bookings_clean.csv` |
| 3. Feature selection | Leakage control: 9 excluded columns with reasons; 11 numeric + 11 categorical features retained; `assert` checks that every column is accounted for |
| 4. Train/test split | Stratified 80/20 split (70,505 train / 17,627 test) |
| 5. Helper functions | Preprocessing pipeline, metric function, experiment logger, 5-fold stratified CV |
| 6. Baseline | Majority-class `DummyClassifier` |
| 7. Logistic Regression | `GridSearchCV` over `C` and `class_weight`; confusion matrix; coefficients |
| 8. Decision Tree | `GridSearchCV` over criterion, depth, leaf size, class weight |
| 9. Random Forest (extension) | `RandomizedSearchCV`, `n_iter=8` |
| 10. Model comparison | Test metrics and CV-vs-test F1 gap (overfitting check) |
| 11. Bootstrap | Paired bootstrap (500 resamples), Random Forest minus Decision Tree |
| 12. Confusion matrices, ROC and PR curves | Per-model and combined plots |
| 13. Feature importance | Top 10 for Decision Tree and Random Forest |
| 14. Threshold tuning | Thresholds chosen on out-of-fold training predictions only, then applied once to the test set |
| 15. Final comparison | All three models side by side, combined ROC curve |
| 16. Experiment log | Every run saved to `outputs/experiment_log.csv` |

The table above reflects the actual
headings in the notebook.

## Design summary
- **Target:** `is_canceled` (1 = cancelled). About 27.9% cancelled in the cleaned data.
- **Prediction time:** when the booking is created; only information available then is used.
- **Primary metric:** F1-score. Secondary: precision, recall, accuracy, ROC-AUC, PR-AUC.
- **Baseline:** always predict "not cancelled".
- **Validation:** stratified 80/20 split; `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)` on the training set only. The test set is used once for the final comparison.
- **Preprocessing (inside a scikit-learn `Pipeline`, so fitted on training folds only):**
  numeric features passed through for tree models and standardised for Logistic Regression;
  categorical features one-hot encoded with levels under 1% frequency pooled into an "infrequent" bucket.
- **Leakage controls:** `reservation_status` and `reservation_status_date` (direct leakage);
  `booking_changes`, `days_in_waiting_list`, `assigned_room_type`, `total_of_special_requests`,
  `required_car_parking_spaces` (post-booking information); plus `arrival_date_year`,
  `total_stay_nights`, `arrival_date` (generalisation or redundancy).

## Expected outputs (written to `outputs/`)
| File | Description |
|---|---|
| `experiment_log.csv` | One row per run: model, features, split, seed, settings, CV F1 mean/std, all test metrics |
| `all_models_comparison.csv` | Test metrics for Logistic Regression, Decision Tree, Random Forest |
| `model_comparison.csv` | Test metrics for baseline, Decision Tree, Random Forest (with TP/FP/FN/TN) |
| `bootstrap_rf_vs_dt.csv` | Paired bootstrap 95% intervals (F1 and ROC-AUC) |
| `lr_metrics.csv`, `dt_metrics.csv` | Per-model metric tables |
| `lr_coefficients.csv` | Logistic Regression coefficients |
| `dt_feature_importance.csv`, `rf_feature_importance.csv` | Impurity-based importances |
| `lr_confusion_matrix.png`, `dt_confusion_matrix.png`, `confusion_matrices.png` | Confusion matrices |
| `roc_separate.png`, `roc_combined.png`, `roc_all_models.png`, `roc_pr_curves.png` | ROC and precision-recall curves |
| `feature_importance.png` | Top-10 feature importance plot |

## Key results (test set, threshold 0.50)
| Metric | Baseline | Logistic Regression | Decision Tree | Random Forest |
|---|---|---|---|---|
| F1 | 0.0000 | 0.5970 | 0.6450 | 0.6594 |
| Precision | 0.0000 | 0.4863 | 0.5093 | 0.5306 |
| Recall | 0.0000 | 0.7728 | 0.8793 | 0.8708 |
| Accuracy | 0.7213 | 0.7091 | 0.7302 | 0.7492 |
| ROC-AUC | 0.5000 | 0.7991 | 0.8360 | 0.8651 |
| PR-AUC | – | 0.5680 | 0.6314 | 0.7077 |

Selected hyperparameters: Logistic Regression `C=0.01`, `class_weight="balanced"`;
Decision Tree `criterion="entropy"`, `max_depth=12`, `min_samples_leaf=1`,
`class_weight="balanced"`; Random Forest `n_estimators=200`, `max_depth=20`,
`min_samples_leaf=5`, `class_weight="balanced"`.

## Random seeds
`RANDOM_STATE = 42` is used for the train/test split, the cross-validation
folds, the `DecisionTreeClassifier` and `RandomForestClassifier`, the
`RandomizedSearchCV` sampling, and the bootstrap resampling.

## Expected runtime
The full modelling notebook typically runs in roughly 5–15 minutes on a
standard laptop. Most of the time is spent in the Random Forest search and the
out-of-fold predictions for threshold tuning (both use `n_jobs=-1`). Exact time
depends on CPU cores.

## Reproducibility notes
- Results should match the report when run with the same library versions;
  small differences in the last decimal places can occur across scikit-learn versions.
- The Random Forest search samples 8 of the 24 possible combinations, so the
  selected configuration depends on the seed and scikit-learn version.
- Numbers in the report (Sections 3–5) are taken from the notebook outputs
  and the files in `outputs/`.

## Generative AI use
<!-- Update the tool names, purpose and checks below to match what your group actually used. -->
Generative AI tools (Claude by Anthropic and ChatGPT by OpenAI) were used to
assist with drafting notebook code, documentation and report text. All code,
numbers and statements were reviewed and verified against the notebook outputs
and the dataset by both group members before submission. The group remains
responsible for all submitted content.

## Group
| Member | SID | UniKey | Main responsibility |
|---|---|---|---|
| Member 1 | 560777246 | mpan0574 | Logistic Regression, report writing, documentation |
| Member 2 | 560802076 | nabr1027 | Decision Tree, report writing, documentation |

