# Evaluation and Modeling Summary

This document outlines the evaluation strategy, model selection process, and final model choice for the honeybee health prediction project.

## A. The Evaluation Plan

The evaluation plan was designed to prevent the most common pitfall: data leakage. The entire strategy is built around simulating a real-world deployment scenario.

* **Unit of Analysis & Data Splits:**

  * The unit of analysis (what we want to predict) is a *single hive inspection*.

  * However, multiple inspections belong to the same hive, meaning these rows are **not independent**. A model could easily memorize a hive's typical behavior.

  * To solve this, a **Grouped Split Strategy** (`GroupShuffleSplit` from `Generate_TrainTestSplit.ipynb`) was used.

  * **`HiveID`** was used as the grouping variable. This ensures that all inspections for a given hive are placed entirely in either the training set OR the test set, but never in both.

* **Justification for Split Strategy:**

  * This strategy directly reflects a real-world deployment. The model must be able to predict the health of a hive it has not seen before, rather than just recalling the history of a hive it saw in training.

  * This method prevents data leakage and provides a much more realistic estimate of the model's performance on new, unseen data.

  * This same `GroupKFold` logic (grouped by `HiveID`) was also used during cross-validation (`modeling_experiments.ipynb`) to ensure that hyperparameter tuning was also free from leakage.

* **Stress Tests:**

  * **Leakage Test:** The `GroupShuffleSplit` on `HiveID` is the primary and most important stress test, as it validates the model's ability to generalize to new subjects (hives).

  * **Distribution Shift Check:** By splitting on `HiveID`, we are inherently testing the model's ability to handle a shift in the data distribution (i.e., new hives that may have different baseline characteristics).

  * **Geographic Robustness:** The dataset contains hives from two distinct locations (NC and UT). By training on a mix and testing on unseen hives from both, the model is implicitly tested for its robustness across different geographies and climates.

## B. Modeling and Selection Summary

A "baseline-then-experiment" approach was used, starting with a simple model and increasing complexity to find the best performance.

* **Model Families Tried:**

  1. **`LogisticRegression`** (`modeling_baselines.ipynb`): A simple, interpretable linear model was used to establish a fast "common sense" baseline.

  2. **`RandomForestClassifier`** (`modeling_experiments.ipynb`): A bagging ensemble model, tested for its robustness and ability to handle non-linear relationships without overfitting.

  3. **`XGBClassifier` & `LGBMClassifier`** (`modeling_experiments.ipynb`): Gradient-boosted tree models, tested for their high predictive power and ability to capture complex interactions between weather and lagged features.

* **Alignment with Project Goals:**

  * The project's goal is to accurately identify at-risk (unhealthy) hives.

  * The primary Key Performance Indicator (KPI) for model selection and tuning was **ROC-AUC**, as it provides the best measure of a model's ability to distinguish between the 'Healthy' and 'Unhealthy' classes.

  * The **F1-Score** was also used as a key metric in the final evaluation, as it balances precision and recall for the positive 'Healthy' class.

  * The plain **accuracy** was used as an additional metric in the final evaluation as it provides a general overall performance of the model.

* **Justification for Final Chosen Model:**

  * The **`XGBClassifier`** (`final_results.ipynb`) was selected as the final model.

  * During the hyperparameter tuning phase (`modeling_experiments.ipynb`), the `XGBClassifier` (tuned with `RandomizedSearchCV`) yielded the strongest `ROC-AUC`, `F1-score`, and`Accuracy` and  scores in cross-validation.

  * This indicates it was the most effective model at separating the two classes, which directly aligns with the project goal. It provided the best balance of performance and ability to generalize to unseen hives.

* **Notes on Discarded Approaches:**

  * **`LogisticRegression`** served its purpose as a baseline but was discarded as its linear nature was (as expected) outperformed by the more complex tree-based models.

  * **`RandomForest`** and **`LGBM`** were strong competitors, but the tuned `XGBoost` model was selected as the "winner" from the experimentation phase for final evaluation.