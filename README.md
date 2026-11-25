# Imbalanced Clinical Disease Predictor

This project evaluates imbalanced binary disease-prediction models using SVM, Random Forest, and XGBoost combined with class weighting or SMOTE.

## Project Structure

```text
imbalanced clinical disease predictor/
├── best_clinical_disease_predictor.joblib
├── task4_confusion_matrix.png
├── task4_cross_validation_results.csv
├── task4_test_model_comparison.csv
├── Project_Report.pdf
└── README.md
```

## Goal

Select a classifier that identifies disease-positive cases while balancing:

- False negatives: positive cases that the model misses
- False positives: negative cases incorrectly labeled positive
- Precision: Reliability of positive predictions
- Specificity: Ability to identify negative cases
- F1: balance between precision and recall

Recall is particularly important because missed clinical cases can delay diagnosis or treatment.

## Methods Compared

- SVM with class weights
- SVM with SMOTE
- Random Forest with class weights
- Random Forest with SMOTE
- XGBoost with class weighting
- XGBoost with SMOTE

SMOTE and class weighting are used to address imbalance in the target classes.

## Cross-Validation Results

| Model | Mean CV Recall | Std CV Recall |
|---|---:|---:|
| SVM + class weights | 0.775858 | 0.033892 |
| XGBoost + SMOTE | 0.766224 | 0.016701 |
| Random Forest + SMOTE | 0.761683 | 0.030764 |
| XGBoost + class weighting | 0.756811 | 0.039163 |
| SVM + SMOTE | 0.747841 | 0.044204 |
| Random Forest + class weights | 0.733555 | 0.013188 |

The full tuned parameters are recorded in `task4_cross_validation_results.csv`.

## Held-Out Test Results

| Model | Test Recall | Precision | Specificity | F1 |
|---|---:|---:|---:|---:|
| SVM + class weights | 0.7407 | 0.5797 | 0.71 | 0.6504 |
| XGBoost + SMOTE | 0.8333 | 0.6000 | 0.70 | 0.6977 |
| Random Forest + SMOTE | 0.7963 | 0.5890 | 0.70 | 0.6772 |
| XGBoost + class weighting | 0.8148 | 0.6286 | 0.74 | 0.7097 |
| SVM + SMOTE | 0.7407 | 0.6557 | 0.79 | 0.6957 |
| Random Forest + class weights | 0.7037 | 0.5938 | 0.74 | 0.6441 |

## Recommendation

**XGBoost with class weighting** provides the strongest test F1-score (`0.7097`) and high recall (`0.8148`). It offers the best balance among the test metrics shown.

**SVM with class weights** has the highest mean cross-validation recall (`0.775858`) and is shown in the available confusion-matrix artifact.

## Confusion-Matrix Interpretation

The SVM confusion matrix contains:

|  | Predicted Negative | Predicted Positive |
|---|---:|---:|
| Actual Negative | 71 (true negatives) | 29 (false positives) |
| Actual Positive | 14 (false negatives) | 40 (true positives) |

This corresponds to:

- Recall: 74.07%
- Precision: 57.97%
- Specificity: 71.00%
- F1: approximately 65.04%


## Saved Artifact

[best_clinical_disease_predictor.joblib](best_clinical_disease_predictor.joblib) stores the serialized predictor pipeline.

## Loading the Predictor

```python
import joblib

model = joblib.load("best_clinical_disease_predictor.joblib")
predictions = model.predict(new_patient_data)
```

The input schema must match the features used during training. Install `imblearn` in environments that need to deserialize pipelines containing SMOTE components.

## Outputs

- [Cross-validation and tuning results](task4_cross_validation_results.csv)
- [Test model comparison](task4_test_model_comparison.csv)
- [SVM confusion matrix](task4_confusion_matrix.png)
- [Serialized predictor](best_clinical_disease_predictor.joblib)
- [Complete PDF report](Project_Report.pdf)

## Limitations

- The cohort size, data source, feature names, and split strategy are not included in the repository artifacts.
- Cross-validation and test metrics do not establish real-world clinical performance.
- Calibration and probability thresholds require clinical-cost analysis.
- External validation on independent patient data is needed before deployment.

## Safety

This project is a machine-learning experiment, not a medical diagnosis system. It must not be used as a replacement for qualified clinical judgment.

