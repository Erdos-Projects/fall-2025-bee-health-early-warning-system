# Study of National Quarterly Colony Collapse 


The cause of colony collapse is complex and not well understood. Leveraging data on colony collapse, the presence of pathogens and pests, and climate data we investigate patterns in colony collapse. 

## Goal
Develop a data-driven early warning system that will indicate when colonies are at a high risk of collapse 


## Data
* United States Department of Agriculture National Agricultural Statistics Service [(USDA NASS) survey data, ](/data/raw/F5B59C51-6C83-3F5B-8117-CD7D3D157B1C.csv) - county level counts of colony collapse on a quarterly basis. The survey also includes observations of some risk factors, such as Verroa mites or suspected pesticide harm. 
* [Animal and Plant Health Inspections Service inpection data](/data/raw/aphis_state_year_data.csv) - state-level results of hive inspections and testing for pathogens 
* [Bee Informed Project](/data/raw/LMS2022_States_Loss.csv) - state level colony loss data which supplements missing data from the USDA NASS

## Summary

** Exploratory Data Analysis** 
* [aphis_eda.ipnb](/notebooks/aphis_eda.ipynb) - APHIS data cleaning and  exploratory analysis
* [nass_eda.ipnb](/notebooks/nass_eda.ipynb) - NASS data cleaning and exploratory analysis 
* [combined_eda.ipnb](/notebooks/combined_eda.ipynb) - exploratory data analysis of the combined data set

** Feature Creation ** 
* [Varroa_Pathogen_data.ipynb](/notebooks/Varroa_Pathogen_data.ipynb) 
* []() 





![Deformed Wing Virus in September -  Map of the US](image.png)