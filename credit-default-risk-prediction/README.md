# Credit Default Risk Prediction

## Problem Statement
Develop a machine learning system to predict whether a borrower will default on a loan using historical credit data. The system helps financial institutions make informed lending decisions, reduce credit risk and financial losses, and improve overall credit risk management.

## Project Objectives

- Understand the data, target distribution, and repayment-status patterns relevant to credit default.
- Establish an interpretable rule-based benchmark.
- Build a reproducible preprocessing and Logistic Regression pipeline.
- Evaluate the trade-off between identifying defaulters and avoiding false alarms.
- Examine false-negative predictions and assess a class-weighted model for the imbalanced target.

## Dataset
- Source: UCI Machine Learning Repository
- Dataset: Default of Credit Card Clients
- Dataset URL: https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients

The dataset contains 30,000 credit card customers described by 25 variables. The target variable, default payment next month, indicates whether a customer defaulted in the following month. The dataset is imbalanced, with 22.12% defaults and 77.88% non-defaults. Input features include customer attributes, credit limit, six months of repayment status (`PAY_0` to `PAY_6`), bill statement amounts, and payment amounts.

## Machine Learning Workflow

1. Load the Excel dataset using its second row as the column header and inspect its dimensions, data types, summary statistics, missing values, and target distribution.
2. Establish a baseline rule: predict default when the latest repayment status, `PAY_0`, is at least one month overdue.
3. Split the data into 80% training and 20% test sets using stratified sampling with `random_state=42`.
4. Remove the `ID` identifier column. No missing values were found, so imputation was not required.
5. One-hot encode `SEX`, `EDUCATION`, and `MARRIAGE`; standardize continuous features including credit limit, age, bill amounts, and payment amounts. Keep ordinal repayment-status features unchanged.
6. Combine preprocessing and Logistic Regression in a scikit-learn `Pipeline`, then evaluate accuracy, precision, recall, F1-score, and the confusion matrix on the held-out test set.
7. Compare decision thresholds of 0.3, 0.5, and 0.7, perform false-negative analysis, retrain and evaluate a Logistic Regression model with `class_weight="balanced"`.

## Project Structure

```text
credit-default-risk-prediction/
|-- credit-default-risk-prediction.ipynb  # End-to-end analysis and modelling workflow
|-- data/
|   `-- raw/
|       `-- default of credit card clients.xls
|-- requirements.txt
`-- README.md
```

## Technologies Used

- Python
- pandas and NumPy for data handling
- scikit-learn for splitting, preprocessing, pipelines, Logistic Regression, and evaluation
- Matplotlib for the confusion-matrix visualization
- Jupyter Notebook for experimentation and documentation

## Results

The rule-based `PAY_0 >= 1` baseline was evaluated on the full dataset and achieved 78.01% accuracy, 50.29% precision, 51.67% recall, and a 0.5097 F1-score.

The primary Logistic Regression pipeline was evaluated on the 6,000-record test set:

| Model | Accuracy | Precision | Recall | F1-score |
| --- | ---: | ---: | ---: | ---: |
| Logistic Regression | 0.8095 | 0.6966 | 0.2457 | 0.3632 |
| Logistic Regression (`class_weight="balanced"`) | 0.6780 | 0.3674 | 0.6315 | 0.4645 |

The unweighted model was more precise but missed many actual defaulters. Lowering its decision threshold from 0.5 to 0.3 increased recall from 24.57% to 45.82%, while precision decreased from 69.66% to 55.17%. The class-weighted model further increased recall to 63.15% and F1-score to 0.4645, with lower accuracy and precision.

## Key Learnings

- The target imbalance makes accuracy insufficient on its own: missing a true defaulter can carry meaningful lending risk.
- Repayment status is a useful interpretable signal, as reflected in the rule-based baseline, but a simple `PAY_0` rule is not sufficient for reliable classification.
- Threshold selection and class weighting meaningfully change the precision-recall balance and should be chosen according to business costs.
- The unweighted model generated 1,001 false negatives. Many had `PAY_0` values from -1 to 1, especially 0, showing that default prediction is harder when recent repayment behavior shows little or no delay.

## Future Improvements

- Select the classification threshold using explicit business costs for false positives and false negatives.
- Retrain when material drift, performance degradation, business-rule changes, or sufficient new data warrant it.

## How to Run

1. Clone the repository and move into the project directory.

   ```bash
   cd credit-default-risk-prediction
   ```

2. Create and activate a Python virtual environment (optional but recommended), then install dependencies.

   ```bash
   python -m venv .venv
   # Windows PowerShell
   .\.venv\Scripts\Activate.ps1
   pip install -r requirements.txt
   ```

3. Ensure the raw dataset is available at `data/raw/default of credit card clients.xls`.

4. Start Jupyter and run `credit-default-risk-prediction.ipynb` from top to bottom.

   ```bash
   jupyter notebook
   ```

