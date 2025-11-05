## Data Inventory

This document outlines the data sources, access methods, licensing, and limitations for the datasets used in this project.

### 1. NOAA Daily Weather Summaries

This dataset includes daily weather summaries for two locations, North Carolina (NC) and Utah (UT), for the years 2016-2019.

##### Data Sources:

weather_NC.csv: Daily summaries for Raleigh Durham International Airport, NC (USW00013722).

weather_UT.csv: Daily summaries for Salt Lake City International Airport, UT (USW00024127).

##### Access Method:

The data was provided as local CSV files. The original source is the NOAA National Centers for Environmental Information (NCEI) data portal: ncei.noaa.gov

##### Licensing:

Data produced by NOAA is considered a work of the U.S. Government and is in the Public Domain in the United States. NOAA formally dedicates its data to the public domain using the Creative Commons 1.0 Universal Public Domain Dedication (CC0 1.0).

##### Dataset Description & Columns:

STATION: Unique identifier for the weather station.

NAME: Common name of the weather station.

DATE: The date of the observation.

AWND: Average daily wind speed (miles per hour).

PRCP: Precipitation (inches).

SNOW: Snowfall (inches).

TAVG: Average daily temperature (Fahrenheit).

TMAX: Maximum daily temperature (Fahrenheit).

TMIN: Minimum daily temperature (Fahrenheit).

TSUN: Amount of sunshine (hours).

##### Limitations:

a. The TSUN (sunshine) column is almost entirely null in both datasets and is not usable for analysis.

b. The provided files also contain data from 2015, though the period of interest is 2016-2019.

c. As with any observational data, it may be subject to occasional measurement or recording errors, though it has undergone NOAA quality control.

### 2. Honeybee Colony Health Data

This dataset collection provides information on honeybee apiaries, individual hives, and health inspections based on the "Healthy Colony Checklist" (HCC).

##### Data Source:

The data was obtained from the research article: "Predicting Honeybee Health: The Healthy Colony Checklist"

Publication URL: https://data-for-good.pubpub.org/pub/rg3364dl/release/1

##### Access Method:

The data was provided as three local CSV files: Apiary_Information.csv, Hive_Information.csv, and HCC_Inspections.csv.

##### Licensing:

Creative Commons Attribution 4.0 International License (CC BY 4.0). This means the data can be shared and adapted for any purpose, provided appropriate credit is given to the original authors.

##### Dataset Descriptions & Columns:

Apiary_Information.csv

1. A lookup table describing the bee apiaries (locations).

2. ApiaryID: (Primary Key) Unique identifier for the apiary.

3. Apiary: The common name of the apiary.

4. City: The city where the apiary is located.

5. State: The state (NC or UT) where the apiary is located.

Hive_Information.csv

1. A lookup table describing the individual beehives.

2. HiveID: (Primary Key) Unique identifier for the hive.

3. Hive_Tag: A human-readable tag or name for the hive.

4. ApiaryID: (Foreign Key) Links the hive to a location in the Apiary_Information.csv file.

HCC_Inspections.csv

1. A table of inspection records for individual hives, based on the Healthy Colony Checklist.

2. InpsectionID: (Primary Key) Unique identifier for the inspection event.

3. HiveID: (Foreign Key) Links the inspection to a hive in the Hive_Information.csv file.

4. InsptDate: The date the inspection was performed.

5. Brood: (Checklist Item) Binary flag (1=Pass, 0=Fail) indicating healthy brood presence.

6. Bees: (Checklist Item) Binary flag (1=Pass, 0=Fail) indicating a healthy bee population.

7. Queen: (Checklist Item) Binary flag (1=Pass, 0=Fail) indicating the presence of a healthy queen.

8. Food: (Checklist Item) Binary flag (1=Pass, 0=Fail) indicating sufficient food stores.

9. Stressors: (Checklist Item) Binary flag (1=Pass, 0=Fail) indicating the absence of significant pests or diseases.

10. Space: (Checklist Item) Binary flag (1=Pass, 0=Fail) indicating adequate space in the hive.

11. Percent_Met: The percentage of the 6 checklist items (from Brood to Space) that passed (scored '1').

12. Healthy: A derived label ("Yes" or "No") indicating overall colony health. "Yes" if all six health parameters are 1 and "No" otherwise. 

##### Limitations:

A small number of null values exist in the bee datasets (e.g., InsptDate, Hive_Tag, and some checklist columns).