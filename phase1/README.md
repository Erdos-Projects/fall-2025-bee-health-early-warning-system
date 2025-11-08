# Study of National Quarterly Colony Collapse 

The cause of colony collapse is complex and not well understood. Leveraging data on colony collapse, the presence of pathogens and pests, and climate data we investigate patterns in colony collapse on a national scale. 

## Goal
Develop a data-driven early warning system that will indicate when colonies are at a high risk of collapse 


## Data
We created a comprehensive dataset for analyzing bee health by integrating four sources from 2010-June 2025, with a trial period of 2015-2025. 

* United States Department of Agriculture National Agricultural Statistics Service [(USDA NASS) survey data, ](data/raw/F5B59C51-6C83-3F5B-8117-CD7D3D157B1C.csv) - county level counts of colony collapse on a quarterly basis. The survey also includes observations of some risk factors, such as Verroa mites or suspected pesticide harm. 
* [Animal and Plant Health Inspections Service inpection data](data/raw/aphis_state_year_data.csv) - state-level results of hive inspections and testing for pathogens 
* [Bee Informed Project](/data/raw/LMS2022_States_Loss.csv) - state level colony loss data which supplements missing data from the USDA NASS
* [National Oceanic and Atmospheric Administration (NOAA)](data/raw/HI_2015.csv) weather data for the United States for 2015-June 2025. 

![Deformed Wing Virus in September -  Map of the US](image.png)

## Repository Summary

**Exploratory Data Analysis** 
* [aphis_eda.ipnb](notebooks/aphis_eda.ipynb) - APHIS data cleaning and  exploratory analysis
* [nass_eda.ipnb](notebooks/nass_eda.ipynb) - NASS data cleaning and exploratory analysis 
* [combined_eda.ipnb](notebooks/combined_eda.ipynb) - exploratory data analysis of the combined data set

**Feature Creation and Data Aggregation** 
* [Varroa_Pathogen_data.ipynb](notebooks/Varroa_Pathogen_data.ipynb) 
* [Weather Related Features - Summarized Quarterly ](notebooks/weather_adv_data.ipynb)
* [Final Data Aggregation](notebooks/final_aggregation.ipynb)

**Modelling and Evaluation**
* [Modeling Baselines](notebooks/modeling_baselines.ipynb)
* [Modeling Experiments](notebooks/modeling_experiment.ipynb)

## Methodology and Modelling
Our Phase 1 objective was to predict future colony loss at the state level. To do this, we engineered a target variable by converting the raw quarterly loss percentage into a binary classification (High/Low Loss) based on the historical median loss for that quarter. This created a balanced target suitable for classification modeling.

Our validation strategy was critical. Because the data was a time series, we used TimeSeriesSplit cross-validation. This ensured our models were always trained on past data (e.g., 2015-2020) to predict future outcomes (e.g., 2021), preventing data leakage. We experimented with several models, including LogisticRegression, XGBClassifier, and LGBMClassifier, to see if a signal could be found. We also conducted rigorous feature engineering, including lagging features and testing various preprocessing pipelines (imputation, scaling) to ensure a fair comparison.

## Results and Conclusions
Our models conclusively demonstrated that predicting future state-level loss is not feasible with this data. Despite rigorous testing, our predictive models (forecasting Y(Q2) from X(Q1)) failed to find a signal, achieving an average ROC-AUC score of approximately 0.53—statistically indistinguishable from a random guess.

Our critical analysis revealed four root causes for this failure:
* **Rapid Signal Decay:** Our investigation showed that while a weak contemporaneous signal exists (linking Varroa in Q1 to loss in Q1), this signal decays too rapidly. The one-quarter lag was too long, and the health status from one quarter had no predictive power on the next quarter.
* **Extreme Feature Sparsity:** The public pathogen data, which we hypothesized was a key predictor, was too sparse. Our analysis showed an average of only 4-8 samples per state, per quarter. This is not enough data to create a stable, representative feature, and our models correctly learned to treat it as statistical noise.
* **Destructive Aggregation:** As stated in our objective, averaging weather and pathogen data over an entire state (like Texas) and a full quarter "dilutes" the signal. Localized events, which are the true drivers of colony health, are lost in this macro-level view.
* **Insufficient Data Depth:** With only 40 quarters of data across ~50 states (roughly 2,000 total data points), the dataset was too shallow for a model to learn the weak, complex patterns hidden in the noise. This limited sample size made it impossible to train a generalizable model.


This definitive failure was the primary motivation for our pivot to Phase 2. We concluded that a successful model must be built from the "bottom-up," using granular, hive-level health data linked directly to local weather, which is precisely what Phase 2 accomplishes.

