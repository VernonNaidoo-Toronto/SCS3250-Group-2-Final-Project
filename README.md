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

### Table of Contents:

* [Introduction](#introduction)
    * [Overview](#overview)
    * [Goals](#goals)
    * [Hypotheses](#hypotheses)
    
* [Phase 1: Data Preparation and Cleaning](#phase-1-data-preparation-and-cleaning)
    * [Data Sources](#data-sources)
    * [Task Overview](#task-overview)
    
* [Phase 2: Analysis](#phase-2-analysis)
    * [Challenges Encountered](#challenges-encountered)
    * [Correlation](#correlation)
    * [Autocorrelation](#autocorrelation) 
    
* [Phase 3: Machine Learning](#phase-3-machine-learning)
    * [Classification, using k-Nearest Neighbours (kNN)](#classification-using-k-nearest-neighbours-knn)
    * [Linear Regression](#linear-regression)
    
* [Conclusion](#conclusion)
    * [Lessons Learned](#lessons-learned)
        * [Preparation and Cleaning](#preparation-and-cleaning)
        * [Analysis](#analysis)
        * [Machine Learning - Classification](#machine-learning---classification)
        * [Machine Learning - Regression](#machine-learning---regression)
        
    * [Ideas for Further Study / Next Steps](#ideas-for-further-study--next-steps)
        * [Preparation and Cleaning](#preparation-and-cleaning-1)
        * [Analysis](#analysis-1)
        * [Machine Learning - Classification](#machine-learning---classification-1)
        * [Machine Learning - Regression](#machine-learning---regression-1)
        
---

## Introduction

### Overview

The Greater Toronto Area relies on Metrolinx's GO Transit as a major transportation network to move its population throughout the GTA. The backbone of this network is the GO train system that moves people to and from Union Station in downtown Toronto. In this project, GO train ridership is observed and studied to look for trends, correlations and patterns. With the addition of weather data such as temperature, precipitation and amount of snow in the City of Toronto, the project aims to find the relationship between GO ridership and weather. Using these relationships, the project attempts to build machine learning models that can be used to predict GO ridership using weather.

### Goals

- Combine and clean GO data and weather data
    - The GO data is in hourly format
    - The weather data is in daily format
    - The datasets need to be combined and use the same format
    - Missing data needs to be addressed
    - GO data is slightly altered to protect confidentiality
    - GO data is per station, it should be in per line or overall sum for analysis
- Identify, explain and handle outliers in GO ridership
    - Outliers are present in the data that can skew results
    - Weekends have significantly different trends than weekdays
    - Holidays affect GO ridership
    - Major events in Toronto have effects on GO ridership
    - These outliers need to be addressed before doing analysis
- Find trends in the GO ridership data
    - Visualizations will be created to better understand the data and trends
    - The autocorrelation of the data will be studied
    - Find trends in time of year and GO ridership
- Find correlations between the GO ridership data and weather data
     - Visualizations will be created to better understand the data and trends
     - Factors that are correlated with each other will be identified  
- Build machine learning models to predict GO ridership using appropriate methods
    - Use trends and correlations identified to build machine learning models
    - Use classification methods to predict range of ridership
    - Use linear regression methods to predict ridership totals

### Hypotheses

- GO ridership is affected by the day of the week
    - *Possible weekly cycles of seasonality*
- The time of year has an impact on GO ridership
    - *Possible annual cycles of seasonality due to weather and non-business days*
- The daily temperature has an effect on GO ridership
    - *Look for correlation between variables*
- GO ridership can be predicted using weather patterns such as temperature, precipitation and snow
    - *Potential applications for machine learning* 

---
## Phase 1: Data Preparation and Cleaning

### Data Sources

#### 1: GO Train Rider Volumes

The first dataset was acquired from Metrolinx (operator of GO Transit in Toronto) by a group member. The dataset includes the number of boardings at each hour at each GO Train Station in the network from March 2017 to February 2020. To protect confidentiality, Metrolinx anonymized the data. Regardless, trends can still be extracted, analyzed and predicted from this dataset.

* Source: [Metrolinx](http://www.metrolinx.com/en/)
* Number of Records: 69,094 (one row per day for each of 68 stations)
* Number of Columns: 26
* Date Coverage: 2017-03-01 to 2020-02-29 (hourly summaries over a 36-month period)
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
      * Monday Trend - All Data
    ![Monday Trend - All Data](https://github.com/VernonNaidoo-Toronto/SCS3250-Group-2-Final-Project/raw/master/Monday%20trend%20-%20all%20data.png)
      * Monday Trend - Holidays and Outliers Excluded
    ![Monday Trend - Holidays and Outliers Excluded](https://github.com/VernonNaidoo-Toronto/SCS3250-Group-2-Final-Project/raw/master/Monday%20trend%20-%20holidays%20and%20outliers%20removed.png)
* Weather data files
    * Load, clean, reshape, merge, enhance
* File merge
    * Merge transit and weather into one consolidated DataFrame; export to csv for analysis and machine learning phases.

[DATA PREPARATION AND CLEANING (Jupyter Notebook Link)](https://github.com/VernonNaidoo-Toronto/SCS3250-Group-2-Final-Project/blob/master/1_Preparation_and_Cleaning.ipynb)

---
## Phase 2: Analysis

### Challenges Encountered
There are 29 variables out of those variables to choose the right ones to analyze correlations and autocorrelations.
Analyzing the relationships among the same variables in different time frames. For this to create many different time frames.

### Correlation

* The primary objective of this part was to understand if there were any association between weather data and ridership. 
* Our dataset was already cleaned and is extracted outliers and public holidays. 
* After analyzing our dataset, the most interesting feature is when we can see a positive relationship between the mean temperature and the number of passengers using Union Station just on the weekend, however, there is not any relationship between these two variables on weekdays. 
* Also, total precipitation and average temperature have weak positive correlations.
 
### Autocorrelation

* The primary objective is to understand whether patterns exist within a single variable as it changes over time.
* For input data, the previously cleaned dataset with holidays and outliers identified was used. 
* After analyzing our dataset, the graph shows us that variables with daily frequency are autocorrelated. However, when selecting monthly summaries, such as monthly mean temperature, the autocorrelation measurement is far below the accepted confidence interval, indicating that no significant autocorrelation can be detected.
* Also, the graphs show us when time moves further away it can be seen less of autocorrelation.

[ANALYSIS (Jupyter Notebook Link)](https://github.com/VernonNaidoo-Toronto/SCS3250-Group-2-Final-Project/blob/master/2_Analysis.ipynb)

---
## Phase 3: Machine Learning

### Classification, using k-Nearest Neighbours (kNN)

The primary objective of this exercise was to understand if it was possible to identify a pattern between the weather data and daily ridership. Given a day with known weather data, i.e. precipitation, temperature, day and weekday, could we predict the expected ridership on that day? kNN provided us a simple and efficient way to do this prediction.

#### Pros

* kNN works best with a small number of attributes. After analyzing our data and whittling it down to the relevant columns, we had four attributes - weekday, month, delta temperature and delta precipitation - and one target class - ridership. This was ideal for kNN.
* kNN required a short computation time - we were able to run multiple prediction models very quickly to identify the best k value.
* Our dataset was already cleansed of all missing values - perfect to work with kNN. 
* kNN is a non-parametric technique and does not rely on underlying data distribution; the model is determined solely from data and no assumptions were made. This ensured that the model only used the given data to create its model instead of relying on outside assumptions such as seasonality.
* To create our models, we used the delta temperature and precipitation columns instead of the actual values; this normalized the data to an extent, which helped make the distance metric used by kNN more meaningful.

#### Cons

* kNN can suffer from skewed class distributions. For example, if a certain class is very frequent in the training set, it will tend to dominate the majority voting of the new example (large number = more common). 
* The algorithm does not work with categorical features, so all weekdays and months had to be changed to numerical counterparts.

#### Summary

We used the final combined weather and transit datasets to create and train 2 kNN models.  We tuned the number of neighbours to find the optimal number needed for both models. 

One model was used to predict for our test data (June, 2019).  The model was trained using the rest of the dataset. The best model gave us an accuracy score of approximately 80% - considered good accuracy.

The other model, which was used to predict for a test day (Monday), was trained using ONLY Mondays. The best model we derived for this scenario gave us a score of approximately 62%, which is quite average. Hence, we can conclude that the larger and more varied the dataset, the better the model we can create with kNN. 

### Linear Regression

One of the questions our group wanted to tackle during the analysis was to know if it was possible to predict ridership volume using regression. Would we be able, by taking into account data from 2017 onwards, to predict how many people are likely to take the train in a specific station at a specific day? 

In the 1st part of our analysis (“Linear Regression with Weekdays - Part 1”), we used the popular machine learning library sklearn to try to predict daily ridership, leaving out weather data and focusing only on past ridership data. By studying the month of January over a period of 3 years, we plotted a model that although it was lacking precision, it already anticipated which days would be busier than others; more specifically Weekdays. 
Even though the first model we used offered some insights, it was clear that there was room for a more accurate prediction which we tried to obtain with the second part of our analysis.

In the 2nd part of our analysis (“Linear Regression with Weekdays - Part 2”), our goal was to obtain a more refined model by improving the methodology used in our first analysis. 
Instead of looking at the month of January only, we decided to work with the data provided for the entire year. We also decided to remove the outliers and the holidays. When we plotted the model, we could observe that we obtained a more accurate prediction using this methodology.

Although the analysis we completed on our second part was satisfactory, we decided to go one step further and create a regression model with two variables: day of the week and weather forecast. The purpose of this was to answer the following question: Can we accurately predict the ridership volume of any given day using the chosen variables?
The regression model created was able to predict that there would be a drop in weekend ridership compared to weekdays. However, it was unclear whether the other variable we used, the weather forecast, had an actual impact on the predictions made. 

This deeper analysis of the data seemed to demonstrate that the model could be losing predictive accuracy when both weekdays and weekends data was analysed at the same time. Therefore, the group decided to look at weekdays and weekends separately.

In the 4th part of the analysis, we started by exploring the correlation between weekday ridership and weekday temperatures. We did not find any clear correlation between those two sets of data. Our conclusion is that it is more than likely that people going to work on weekdays have the same routine and therefore use the same method of transportation regardless of the weather conditions. 
When we analyzed the data for the weekends, it was clear that there was a positive correlation between weekends temperatures and GO ridership. 
Using sklearn, the team built a good, reliable model, demonstrating that passenger volumes increase as temperatures rise, but only during weekends.


[MACHINE LEARNING (Jupyter Notebook Link)](https://github.com/VernonNaidoo-Toronto/SCS3250-Group-2-Final-Project/blob/master/3_Machine_Learning.ipynb)

---

## Conclusion

### Lessons Learned

#### Preparation and Cleaning

Significant time and effort must be devoted to the work of preparing, cleaning, merging and enhancing the data in advance of further analysis.  Failure to dedicate adequate resources  (early in the project and all the way through) can cause considerable rework and refactoring.  Part way through the project, we came to the realization that each of us had modified the dataset for our own dedicated work (autocorrelation, classification, regression, etc.) in similar ways.  We agreed to refactor and standardize the enhancements, such as:
* Addition of "Holidays" boolean column (statutory holidays and common vacation periods)
* Addition of "Outliers" boolean column (data points more than two standard deviations from the mean, as calculated within seasonality subsets)
* Consolidation of the 68 train stations into eight train lines
* Creation of a central "Final Dataset.csv" as the interface point between Preparation and Cleaning and all other work

Benefits included:
* Improved reliability for all downstream analysis results
* Reduction in the complexity and volume of code
* Centralization of all preparation and cleaning work, which had become fragmented
* Consistency of downstream analysis results, thanks to common consolidated and cleansed data file with flags for optional exclusion of unwanted data (holidays and outliers)

#### Analysis

* Correlation and autocorrelation are good tools to help us show the quality and characteristics of data.  They also help to drive the choice of machine learning algorithms to use.
* Effective use of the pandas library for building a scatter matrix
* Efficient techniques for generating complex statistical analysis visualiations of data, using matplotlib, autocorrelation plots and pcolor pseudocolor plots
* Time series analysis, using the autocorrelation function
* Visual approaches for using scatter plots to detect and quantify correlations between pairs of time series variables in a matrix

#### Machine Learning - Classification

* Harness useful pandas libraries like Scikit-Learn to create effective prediction models easily.
* Analyzing and experimenting with a given dataset to extract the most relevant and effective columns for a knn predict model.
* How to modify a given dataset to ensure it works with a kNN predict model, e.g. categorical values cannot be used as attributes.
#### Machine Learning - Regression
* Our first model, using only a month of data and without relying on the outliers/holidays (January having 1 bank holiday) was lacking precision. Even though removing outliers/holidays was assiduous, it was essential in order to obtain a better model.
* When our team started this project, our initial assumption was that ridership during weekdays would be greatly impacted by weather conditions. We assumed that people going to work would adapt their behaviour to the weather, and we were wrong. It was great to see that analyzing data can contradict initial thought and lead to more informed decisions.


### Ideas for Further Study / Next Steps

#### Preparation and Cleaning
* Secure agreement (or employment) with Metrolinx to gain access to unaltered datasets with true values and additional features.
* Create a second "final" dataset with an alternate shape to enable intraday analysis.  Transit data would need to retain hourly detail.  New weather data files with hourly granularity would need to be sourced.  The analysis would expose an additional level of "hourly seasonality" and enable classification and regression to identify peak periods.
* Compare actual hourly passenger volumes to **scheduled** train capacity (**planned** arrival times, train length, seat count) to identify service planning optimization opportunities.
* Comparing actual hourly passenger volumes to **actual** train capacity (**recorded** arrival times, train length, seat count) could reveal opportunities for optimizing service delivery.

#### Analysis
* Recreate the autocorrelation analysis from first principles to gain a deeper understanding of the result.
* Assess the real-world relevance of correlation between variables.  (Is there value in correlating temperature and precipitation or are we exposing simple laws of physics?  How can these be filtered out?)
* Find a way to identify the exact intersection between the autocorrelation line and the confidence interval boundaries.

#### Machine Learning - Classification
* Experiment with different distance methods besides Euclidean, which is what Scikit-Learn uses.
* Tune the hyperparameters to improve the model performance.
* Rescale the attributes using StandardScalar.

#### Machine Learning - Regression
* The entire team was curious to know how passenger volumes could be affected during a weather disruption: if it suddenly rains in a summer weekend, would we be able to observe a peak in ridership? To do this analysis, an additional output would be required from the Preparation and Cleaning phase.  We would need to retain the hourly details for both transit and weather and devise a new structure for the analysis.
* With more granular data, we would also have been able to look at the impact some cultural/social events can have on passenger volumes: Can we predict the peak in ridership the nights the Toronto Raptors are playing?
