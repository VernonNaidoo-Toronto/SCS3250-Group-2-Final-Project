---

# GO Transit Usage and Weather Patterns
### Final Project: Group 2
### SCS3250 Data Science Fundamentals, Winter 2020 Term

---

### Contributors (Alphabetical Order): 

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

The first dataset was acquired from Metrolinx (operator of GO Transit in Toronto) by a group member. The dataset includes the number of boardings at each hour at each GO Train Station in the network from March 2017 to February 2020. To protect confidentiality, Metrolinx anonymized the data. Regardless, trends can still be extracted, analyzed and predicted from this dataset.

* Source: [Metrolinx](http://www.metrolinx.com/en/)
* Number of Records: 69,094 (one row per day for each of 68 stations)
* Number of Columns: 26
* Date Coverage: 2017-03-01 to 2020-02-29 (hourly summaries over a 36 month period)
* Known Issues: 
    * In order to maintain confidentiality, the boardings values were altered, while maintaining seasonality and trends.  Since the values provided do not represent the actual number of passengers boarding the trains, all planned analysis tasks were limited to those requiring only **relative** passenger behaviour trends (relative changes over time).  This condition for release of the data was acceptable, as we were not attempting to analyze profitability or tie back to publicly released statements.
* Challenges Encountered:
    * The shape of the data presented an initial challenge.  It was originally collected with each passenger boarding timestamped and the station of origin identified.  Before it was released to us, the data was summarized into 24 columns, one for each hourly total, and almost 70,000 rows (one row per day for each of 68 stations).

#### 2: GO Train Station Details

The second dataset provided additional GO station/stop information. The data set includes further information about the GO stations from the first dataset such as which GO Line it is on.

* Source: [Metrolinx](http://www.metrolinx.com/en/)
* Number of Records: 977
* Number of Columns: 18
* Date Coverage: None. (Snapshot of latest station details was used.)
* Known Issues: Latest station details were applied across a three-year period of activity.  This was deemed acceptable, as station detail changes are rare and infrequent.
* Challenges Encountered: File included too many rows.  These were determined to represent bus stops and "duplicates" (in fact, each station has two platforms) and filtered out.

#### 3: Historical Weather Data

The second dataset is weather data from the Government of Canada (acquired from: https://climate.weather.gc.ca/historical_data/search_historic_data_e.html). The data consists of daily Toronto weather data such as minimum temperature, maximum temperature, mean temperature, precipitation (mm) and snow on the ground (cm).
* Source: [Environment and Climate Change Canada](https://climate.weather.gc.ca/historical_data/search_historic_data_e.html)
* Number of Records: 1461
* Number of Columns: 31
* Date Coverage: 2017-01-01 to 2020-12-31
* Known Issues:
    * Some relevant columns, such as temperature and precipitation, were missing values.
    * Total rain and snow columns had no values.

* Challenges Encountered:
    * Since the dedicated columns for rain and snow totals were not populated, analysis was limited to total (combined) precipitation.
    * Several indicator columns were deemed unnecessary because they could be inferred from the main measurement columns.  These were dropped during data cleaning phase.

### Task Overview

* Transit data files
    * Load Excel and csv files into DataFrames.
    * Clean to substitute 0 for NaN when no passengers boarded, address errors introduced during anonymization (fractional boardings) and remove extraneous columns.
    * Reshape single boardings column to one column per line.
    * Reshape approximately 70,000 rows x 1 column (one row per day per station) to 1096 rows x 8 columns (one per day, one column per line).
    * Merge (boarding and station details) into a single DataFrame.
    * Enhance with new columns for weekdays, monthly means and differences between daily and monthly means for both precipitation and temperature.
    * Identify the categories of outliers: statutory holidays, end of year vacation periods and observations more than two standard deviations away from the mean (calculated separately for weekday subsets to account for seven-day seasonality patterns).
* Weather data files
    * Load, clean, reshape, merge, enhance
* File merge
    * Merge transit and weather into one consolidated DataFrame; export to csv for analysis and machine learning phases.

[DATA PREPARATION AND CLEANING (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/1_Preparation_and_Cleaning.ipynb)

---
## Phase 2: Analysis

- [ ] Correlation - BW/VN
- [ ] Autocorrelation - BW/VN

[ANALYSIS (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/2_Analysis.ipynb)

---
## Phase 3: Machine Learning

### Classification, Using K-Nearest Neighbours (kNN)

>The primary objective of this exercise was to understand if it was possible to identify a pattern between the weather data and daily ridership. Given a day with known weather data, i.e. precipitation, temperature, day and weekday, could we predict the expected ridership on that day? Knn provided us a simple and efficient way to do this prediction.

#### Pros

* kNN works best with a small number of attributes. After analyzing our data and whittling it down to the relevant columns, we had four attributes - weekday, month, delta temperature and delta precipitation - and one target class - ridership. This was ideal for kNN.
* kNN required a short computation time - we were able to run multiple prediction models very quickly to identify the best k value.
* Our dataset was already cleansed of all missing values - perfect to work with kNN. 
* kNN is a non-parametric technique and does not rely on underlyign data distribution; the model is determined solely from data and no assumptions were made. This ensured that the model only used the given data to create its model instead of relying on outside assumptions such as seasonality.
* To create our models we used the delta temperature and precipitation columns instead of the actual values; this normalized the data to an extent, which helped make the distance metric used by knn more meaningful.

#### Cons

* kNN can suffer from skewed class distributions. For example, if a certain class is very frequent in the training set, it will tend to dominate the majority voting of the new example (large number = more common). 
* The algorithm does not work with categorical features, so all weekdays and months had to be changed to numerical counterparts.

#### Summary

We used the final combined weather and transit datasets to create and train 2 kNN models.  We tuned the number of neighbours to find the optimal number needed for both models. 

One model was used to predict for our test data (June, 2019).  The model was trained using the rest of the dataset. The best model gave us an accuracy score of approximately 80% - considered good accuracy.

The other model, which was used to predict for a test day (Monday), was trained using ONLY Mondays. The best model we derived for this scenario gave us a score of approximately 62%, which is quite average. Hence, we can conclude that the larger and more varied the dataset, the better the model we can create with kNN. 

### Linear Regression

One of the questions our group wanted to tackle during the analysis was to know if it was possible to predict ridership volume using regression. Would we be able, by taking into account data from 2017 onwards, to predict how many people are likely to take the train in a specific station at a specific day? 

In the 1st part of our analysis (“Linear Regression with Weekdays - Part 1”), we used the popular machine learning library sklearn to try to predict daily riderships, leaving out weather data and focusing only on past riderships data. By studying the month of January over a period of 3 years, we plotted a model that although it was lacking precision, it already anticipated which days would be busier than others; more specifically Weekdays. 
Even though the first model we used offered some insights, it was clear that there was room for a more accurate prediction which we tried to obtain with the second part of our analysis.

In the 2nd part of our analysis (“Linear Regression with Weekdays - Part 2”), our goal was to obtain a more refined model by improving the methodology used in our first analysis. 
Instead of looking at the month of January only, we decided to work with the data provided for the entire year. We also decided to remove the outliers and the holidays. When we plotted the model, we could observe that we obtained a more accurate prediction using this methodology.

Although the analysis we completed on our second part was satisfactory, we decided to go one step further and create a regression model with two variables: day of the week and weather forecast. The purpose of this was to answer the following question: Can we accurately predict the ridership volume of any given day using the chosen variables?
The regression model created was able to predict that there would be a drop in ridership during weekends compared to weekdays. However, it was unclear whether the other variable we used, the weather forecast, had an actual impact on the predictions made. 

This deeper analysis of the data seemed to demonstrate that the model could be losing predictive accuracy when both weekdays and weekends data was analysed at the same time. Therefore the group decided to look at weekdays and weekends separately.

In the 4th part of the analysis, we started by exploring the correlation between weekday riderships and weekday temperatures. We did not find any clear correlation between those two sets of data. Our conclusion is that it is more than likely that people going to work on weekdays have the same routine and therefore use the same method of transportation regardless of the weather conditions. 
When we analyzed the data for the weekends, it was clear that there was a positive correlation between weekends temperatures and GO riderships. 
Using sklearn, the team built a reliable and good model demonstrating that there is an increase of riderships as temperatures arise only during weekends.


[MACHINE LEARNING (Jupyter Notebook Link)](https://github.com/Jenarth/SCS3250-Group-2-Final-Project/blob/master/3_Machine_Learning.ipynb)

---

## Conclusion

### Lessons Learned

#### Preparation and Cleaning
#### Analysis
#### Machine Learning - Classification
1. Harness useful pandas libraries like Scikit-Learn to create effective prediction models easily.
2. Analyzing and experimenting with a given dataset to extract the most relevant and effective columns for a knn predict model.
3. How to modify a given dataset to ensure it works with a knn predict model, e.g. categorical values cannot be used as attributes.
#### Machine Learning - Regression

### Ideas for Further Study / Next Steps

#### Preparation and Cleaning
#### Analysis
#### Machine Learning - Classification
1. Experiment with different distance methods besides Euclidean, which is what Scikit-Learn uses.
2. Tune the hyperparameters to improve the model performance
3. Rescale the attributes using StandardScalar.
#### Machine Learning - Regression
