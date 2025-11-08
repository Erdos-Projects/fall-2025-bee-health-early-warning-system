# Study of Hive Level Health Indicators 

The challenges from Phase 1 directly motivated our pivot to Phase 2: a "bottom-up" approach to solve the problem at the scale where interventions actually happen—the individual hive. Our new objective is to create a tactical, hive-level model that predicts hive health (healthy/unhealthy) using highly localized weather data from the past one to two weeks. This approach is more robust for two key reasons: first, it aligns directly with the granular, local data that is available, and second, it allows us to use a standardized definition of "hive health" as established in recent research (Lower et al., 2024). This model provides a more immediate, actionable tool for beekeepers and serves as a more solid foundation for eventually understanding the broader, national-level crisis.

## Data 
See [Data Inventory](data/raw/data_inventory.md) for full details on data sets. 
 
* [Honey Colony Checklist - Inspection Data](data/raw/HCC_Inspections.csv)
* [Apiary Information](data/raw/Apiary_Information.csv)
* [Hive Information](data/raw/Hive_Information.csv)
* Weather data for [North Carolina](data/raw/weather_NC.csv) and [Utah](data/raw/weather_UT.csv)

**Engineered  Features**
Our primary feature engineering task was to prevent data leakage by creating time-series "lag" features (e.g., Prev_Health_Status) to capture the hive's state from its previous inspection, as well as [7-day trailing weather features](notebooks/data_engineering_extraction/weather_features.ipynb) (e.g., Avg_tmax, Num_frost_days) to represent the environmental conditions leading up to the current inspection.

See [Feature Engineering Summary](notebooks/data_engineering_extraction/feature_engineering_summary.md) for full details. 

Final combined dataset is available [here](notebooks/data_engineering_extraction/final_dataset.ipynb). 

## Repository Summary 

**Exploratory Analysis** 
* [Exploratory Data Analysis](notebooks/data_engineering_extraction/EDA.ipynb)

**Modeling and Evaluation**
* [Generate test/Train Split](notebooks/data_engineering_extraction/Generate_TrainTestSplit.ipynb)
* [Modeling Baselines](notebooks/modeling/modeling_baselines.ipynb)
* [Modeling Experiments](notebooks/modeling/modeling_experiments.ipynb)
* [Final Results](notebooks/modeling/final_results.ipynb)

## Modeling

The nature of our data, which contains multiple inspections for the same hive over time, means that individual data points are not independent. A standard random split would cause data leakage, as the model could "memorize" a hive's history from the training set. To simulate a real-world scenario, predicting health for a new, unseen hive, we used a GroupShuffleSplit on HiveID. This ensures all inspections for a single hive are confined to either the training or test set. We established a baseline with LogisticRegression and then experimented with ensemble models (RandomForest, LGBMClassifier, XGBClassifier). After hyperparameter tuning with RandomizedSearchCV, we selected the XGBClassifier as our final model. It yielded the strongest cross-validation ROC-AUC score (0.75), confirming it had the best ability to distinguish between 'healthy' and 'unhealthy' hives.

See the [Modeling Evaluation Summery](notebooks/modeling/evaluation_modeling_summary.md) for full detail on the modeling approach. 

## Conclusion 
Our final tuned XGBClassifier generalized well to the unseen holdout data, achieving a Test ROC-AUC of 0.78 and an Accuracy of 0.72. This strong predictive signal significantly outperformed our LogisticRegression baseline, which only achieved a Test ROC-AUC of 0.65. The model's real-world value was confirmed by its Macro Average F1-score of 0.71, demonstrating a robust and balanced ability to identify both 'healthy' and 'unhealthy' hives. Feature importance analysis revealed that the model's predictions were primarily driven by the hive's recent history. The top three most important predictors were the Previous Queen Status (Prev_Queen_Status), whether it was the First Inspection (Is_First_Inspection), and the Previous Stressor Status (Prev_Stressors_Status). Key environmental factors, specifically the Number of Frost Days (Num_frost_days) and 7-day Average Temperature (Avg_tavg), also proved to be significant predictors of hive health. 
