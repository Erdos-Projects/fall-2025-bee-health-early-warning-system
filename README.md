# Bee Health Early Warning System 

Honeybees are vital to crop pollination and therefore to the security of our food system. However, beekeepers have recently faced an alarming rate of colony collapse. For example, according to the Apiary Inspectors of America, in 2023-24 they lost approximately [55% of their managed colonies. ](https://apiaryinspectors.org/US-beekeeping-survey-23-24?) and between June 2024 and March 2025, commercial beekeepers reported losing an average of [62% of their colonies.](https://beehealthcollective.org/wp-content/uploads/2025/04/04_03_2025UpdatedColonyLossPR.pdf) The causes of colony collapse are complex and not well understood.

Motivated to build an early warning system that could warn beekepers of a likely collapse, this project was taken on in two separate phases with two different combined datasets. 

## [Phase 1: Study of Nationwide Quarterly Collapse](/phase1/)

Statewide quarterly data from the United States Department of National Agricultural Statistics Service (USDA NASS) was combined with weather data from the National Oceanic and Atmospheric Administration (NOAA), and the Bee Informed Partnership inventory of pathogens and pests. 

After some extensive data exploration and modeling attempts, it was determined that while the data contained valuable information about the national distribution of pathogens and pests on a monthly scale, combined with the quarterly loss data, which was incomplete, the data was too coarse both temporally (in 3 month increments) and spatially (state level data) to be able to clearly categorize hive-level risk of collapse.  Therefore, a new dataset was procured and the goals were refined. 

See the [Phase 1](/phase1/) portion of this repository for the full investigation. 

## [Phase 2: Study of Hive Health Markers](/phase2/)
In this phase, we take a "bottom-up" approach to solve the problem at the scale where interventions actually happen—the individual hive. Using localized weather data and a standardized checklist of health indicators, we model the risk of a healthy hive becoming unhealthy.  

See the [Phase 2](/phase2/) portion of this repository for the full investigation. 
