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

### Dataset 1: GO Train Rider Volumes

The first dataset was acquired from Metrolinx (operator of GO Transit in Toronto) by a group member. The dataset includes the number of boardings at each hour at each GO Train Station in the network from March 2017 to February 2020. To protect confidentiality, Metrolinx multiplied the data entries by a coefficient. Regardless, trends can still be extracted, analyzed and predicted from this dataset.
* Number of Records:
* Number of Columns:
* Date Coverage:
* Known Issues:
* Challenges Encountered:

### Dataset 2: GO Train Station Details

The second dataset provided additional GO station/stop information. The data set includes further information about the GO stations from the first dataset such as which GO Line it is on.
- [ ] Number of Records:
- [ ] Number of Columns:
- [ ] Date Coverage:
- [ ] Known Issues:
- [ ] Challenges Encountered:

### Dataset 3: 

The second dataset is weather data from the Government of Canada (acquired from: https://climate.weather.gc.ca/historical_data/search_historic_data_e.html). The data consists of daily Toronto weather data such as minimum temperature, maximum temperature, mean temperature, precipitation (mm) and snow on the ground (cm).
- [ ] Number of Records:
- [ ] Number of Columns:
- [ ] Date Coverage:
- [ ] Known Issues:
- [ ] Challenges Encountered:
    - [ ] xxx
* Load transit data files; merge; reshape; identify outliers -> transit.csv
* Load weather data files; merge with transit -> weather.csv
* Merge transit and weather -> Final Data.csv
* [DATA PREPARATION AND CLEANING (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/1_Preparation_and_Cleaning.ipynb)

---
## Phase 2: Analysis

- [ ] Correlation
- [ ] Autocorrelation
- [x] [ANALYSIS (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/2_Analysis.ipynb)
---
## Phase 3: Machine Learning

- [ ] Classification (kNN)
- [ ] Regression
- [x] [MACHINE LEARNING (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/3_Machine_Learning.ipynb)
---
## Conclusion

- [ ] Overall thoughts
- [ ] Successes, failures, dead ends
- [ ] Ideas for further study
