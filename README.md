# Group 2 Final Project: GO Transit Usage and Weather Patterns
## SCS3250 Data Science Fundamentals, Winter 2020 Term

### Members (Alphabetical Order): 
* Jenarth Jegatheeswaran
* Vernon Naidoo
* Mayisha Sabia
* Louise Wyon
* Bercin Yildirim
---
## Introduction

- [ ]   Overview
- [ ]   Goals
- [ ]   Hypotheses
---
## Phase 1: Data Preparation and Cleaning

### Datasets:

#### 1: GO Train Rider Volumes

The first dataset was acquired from Metrolinx (operator of GO Transit in Toronto) by a group member. The dataset includes the number of boardings at each hour at each GO Train Station in the network from March 2017 to February 2020. To protect confidentiality, Metrolinx multiplied the data entries by a coefficient. Regardless, trends can still be extracted, analyzed and predicted from this dataset.
- [x] Source: [Metrolinx](http://www.metrolinx.com/en/)
- [x] Number of Records: 69,094 (one row per day for each of 68 stations)
- [x] Number of Columns: 26
- [x] Date Coverage: 2017-03-01 to 2020-02-29 (hourly summaries over a 36 month period)
- [x] Known Issues: 
    * In order to maintain confidentiality, the boardings values were altered, while maintaining seasonality and trends.  Since the values provided do not represent the actual number of passengers boarding the trains, all planned analysis tasks were limited to those requiring only **relative** passenger behaviour trends (relative changes over time).  This condition for release of the data was acceptable, as we were not attempting to analyze profitability or tie back to publically released statements.
- [x] Challenges Encountered:
    * The shape of the data presented an intial challenge.  It was originally collected with a timestamp for each passenger, with the station of origin identified.  Before it was released to us, the data was summarized into 24 columns, one for each hourly total, and almost 70,000 rows (one row per day for each of 68 stations).

#### 2: GO Train Station Details

The second dataset provided additional GO station/stop information. The data set includes further information about the GO stations from the first dataset such as which GO Line it is on.
- [x] Source: [Metrolinx](http://www.metrolinx.com/en/)
- [x] Number of Records: 977
- [x] Number of Columns: 18
- [x] Date Coverage: None. (Snapshot of latest station details was used.)
- [x] Known Issues: Latest station details were applied across a three-year period of activity.  This was deemed acceptable, as station detail changes are rare and infrequent.
- [x] Challenges Encountered: File included too many rows.  These were determined to represent bus stops and "duplicates" (in fact, each station has two platforms) and filtered out.

#### 3: Historical Weather Data

The second dataset is weather data from the Government of Canada (acquired from: https://climate.weather.gc.ca/historical_data/search_historic_data_e.html). The data consists of daily Toronto weather data such as minimum temperature, maximum temperature, mean temperature, precipitation (mm) and snow on the ground (cm).
- [ ] Source:
- [ ] Number of Records:
- [ ] Number of Columns:
- [ ] Date Coverage:
- [ ] Known Issues:
- [ ] Challenges Encountered:
    - [ ] xxx

### Task Overview:
1. Load transit data files; merge; reshape; identify outliers -> transit.csv
2. Load weather data files; merge with transit -> weather.csv
3. Merge transit and weather -> Final Data.csv

[DATA PREPARATION AND CLEANING (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/1_Preparation_and_Cleaning.ipynb)

---
## Phase 2: Analysis

- [ ] Correlation - BW/VN
- [ ] Autocorrelation - BW/VN

[ANALYSIS (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/2_Analysis.ipynb)

---
## Phase 3: Machine Learning

- [ ] Classification (kNN) - MS
- [ ] Regression - LW

[MACHINE LEARNING (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/3_Machine_Learning.ipynb)

---
## Conclusion
- Lessons Learned
    - [ ] Preparation and Cleaning
    - [ ] 2
    - [ ] 3
- [ ] Ideas for further study
    - [ ] Preparation and Cleaning
    - [ ] 2
    - [ ] 3
