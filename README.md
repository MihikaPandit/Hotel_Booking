# Group 95 – Hotel Booking Analysis and Cancellation Prediction

## Project Overview

This project analyses the Hotel Booking dataset to understand booking patterns and predict hotel booking cancellations. It combines the work completed in Assignment 1, including dataset selection, data cleaning and exploratory data analysis (EDA), with the predictive modelling carried out in Assignment 2.

The project aims to identify factors associated with booking cancellations and evaluate how machine learning models can support hotel revenue managers in making informed decisions about room capacity, revenue management and booking policies.

## Dataset

The project uses the Hotel Booking dataset, which initially contained 119,987 observations and 32 variables. Following data cleaning and preparation, the final dataset contained 88,132 bookings and 33 columns.

The dataset includes numerical and categorical information about hotel bookings, such as lead time, hotel type, deposit type, market segment, customer type and previous cancellations.

The target variable is `is_canceled`, where:
- `1` indicates a cancelled booking.
- `0` indicates a booking that was not cancelled.

## Assignment 1: Dataset Selection, Data Cleaning and EDA

Assignment 1 focused on comparing potential datasets and selecting the Hotel Booking dataset for further analysis. The selected dataset was cleaned and prepared for analysis, with attention given to missing values, duplicates, inconsistent values and other data-quality issues.

Exploratory data analysis was then performed to understand the dataset, examine booking characteristics and identify patterns associated with cancellations. The findings helped guide the selection of relevant variables for predictive modelling.

## Assignment 2: Predictive Modelling

Assignment 2 builds on the findings from Assignment 1 by developing and evaluating models to predict booking cancellations.

The project includes a majority-class baseline and three classification models:
- Logistic Regression
- Decision Tree
- Random Forest

The models were evaluated using a stratified 80/20 training-test split and five-fold cross-validation. Model performance was compared using accuracy, precision, recall, F1-score, ROC-AUC and PR-AUC.

Random Forest achieved the highest test F1-score of 0.6594 and ROC-AUC of 0.8651 among the evaluated models. Decision Tree achieved slightly higher recall, at 0.8793 compared with Random Forest's 0.8708.

Based on the results, Random Forest is recommended as a candidate for identifying bookings that may require further review by hotel revenue managers. Further validation is required before practical implementation.

## Technologies and Tools

- Python
- Pandas and NumPy for data handling
- Matplotlib and Seaborn for data visualisation
- Scikit-learn for preprocessing, model development, hyperparameter tuning and evaluation
- Jupyter Notebook for implementation and analysis

## Project Files

The main project files include:

- `Group_95_A2.ipynb` – notebook containing the predictive modelling workflow, experiments and evaluation.
- `hotel_bookings_clean.csv` – cleaned dataset used for modelling.
- `outputs/experiment_log.csv` – experiment log recording model configurations and evaluation results, if included in the submitted project files.

Additional files may include the original dataset, EDA outputs, visualisations and the final report.

## How to Run the Project

1. Download or clone the project files.
2. Ensure Python and the required libraries are installed.
3. Place `hotel_bookings_clean.csv` in the same directory as `Group_95_A2.ipynb`, or update the dataset path in the notebook.
4. Open the notebook using Jupyter Notebook, JupyterLab or a compatible environment.
5. Run the notebook cells in order to reproduce the modelling and evaluation workflow.

## Limitations

The dataset may not contain every factor that influences booking cancellations, such as unexpected changes in travel plans or personal circumstances. In addition, models trained on historical bookings may not perform equally well on future bookings. Predictions indicate cancellation risk rather than certainty and should support human judgement rather than automatically determine booking restrictions or overbooking decisions.



## Conclusion

This project demonstrates how data analysis and predictive modelling can be used to study hotel booking cancellations. The findings identify useful patterns in booking data and show that classification models can improve cancellation detection compared with a simple majority-class baseline. Random Forest is the preferred candidate based on its overall test performance, subject to further validation and assessment of real business costs before operational use.
