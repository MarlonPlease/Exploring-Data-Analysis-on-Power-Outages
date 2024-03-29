# Exploring Data Analysis on Power Outages
by Marlon



---

## Introduction

We will analyze and explore data from an Excel dataset regarding major outages in the continental U.S. between January 2000 and July 2016. The dataset includes information on outage locations, dates and times, regional climate, land-use characteristics, electricity consumption patterns, and economic factors.


Objective (put something after this)



The data used in this analysis originates from the article "A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S." (Mukherjee et al., 2018). It explores major power outages that occurred in different states across the continental United States between January 2000 and July 2016. Major outages are defined by the Department of Energy as those impacting a minimum of 50,000 customers or causing an unplanned firm load loss of at least 300 MW.

The dataset contains details such as outage location, date, and time. Additionally, it includes regional climate data, land-use characteristics, electricity consumption patterns, and economic factors of the affected states. This comprehensive dataset enables the identification and analysis of historical trends and patterns in major outages, as well as the assessment of risk predictors associated with prolonged power disruptions in the continental United States.
Data Acquisition

To obtain the necessary data for this study, we utilized various publicly available datasets, including:

- OE-417 form Schedule 1 published by DOE's Office of Electricity Delivery and Energy Reliability
- U.S. Energy Information Administration (EIA) datasets, specifically form EIA-826 and EIA-861
- National Oceanic and Atmospheric Administration (NOAA) and National Climatic Data Center (NCDC)
- U.S. Department of Labor, Bureau of Labor Statistics
- U.S. Census Bureau

By combining these datasets, we were able to gather comprehensive information for our power outage analysis.

Please refer to the project files for the implementation details and analysis of the data.

For more background information about the dataset, please refer to the provided link [here]([https://www.sciencedirect.com/science/article/pii/S2352340918307182]), the link to the excel file is [here](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks)

***Question Identification***

Upon looking at the dataset, we noticed that we wanted to predict the severity (in terms of duration,) of a major power outage; with that question in mind we formed a Null and Alternative hypothesis. This is a multivariable classification problem.
For the metric, we decided on using 'accuracy'. In this scenario, where the risk of false positives and false negatives is approximately equal, accuracy would be the preferred metric. Accuracy measures the overall correctness of the predictions by considering both true positives and true negatives.

***Null hypothesis:*** Our model is fair.
Its precision for years before and after 2010 are roughly the same, and any differences are due to random chance.

***Alternative hypoethsis:*** Our model is unfair.
Its precision for year prior to 2010 is lower than its precision for tear after 2010


---
## Framing the Problem
***Cleaning the Dataset***
This is the original formatted `powerDF` from a previous project, as one can see, there were many 'null' values, however, there seemed to be interesting data from the columns:

|   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |
|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|
|   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |         0.4112 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 05:00:00 | 2011-07-03 08:00:00  |
|   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |         0.3748 |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 06:38:00 | 2014-05-11 06:39:00  |
|   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |         0.3924 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 08:00:00 | 2010-10-28 10:00:00  |
|   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |         0.4224 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 11:00:00  |
|   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |         0.367  |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |

-We converted the "categorical" string columns such as `'CAUSE.CATEGORY'`, `'CLIMATE.CATEGORY'`, `'U.S._STATE'`  into non-ordered "category" integers just in case we needed int values for processing.

-We felt the most important for analyzing were `'ANOMALY.LEVEL'`,`'CAUSE.CATEGORY'`, `'CLIMATE.CATEGORY'`
`powerDF_model.head()` as shown(but we kept our options open with keeping other columns we think we would find useful later on):

|   YEAR |   MONTH |   CLIMATE.REGION |   ANOMALY.LEVEL |   CLIMATE.CATEGORY |   U.S._STATE |   CAUSE.CATEGORY |   OUTAGE.DURATION |   PI.UTIL.OFUSA |   UTIL.CONTRI |
|-------:|--------:|-----------------:|----------------:|-------------------:|-------------:|-----------------:|------------------:|----------------:|--------------:|
|   2011 |       7 |                1 |            -0.3 |                  1 |           21 |                5 |              3060 |             2.2 |       1.75139 |
|   2014 |       5 |                1 |            -0.1 |                  1 |           21 |                2 |                 1 |             2.2 |       1.79    |
|   2010 |      10 |                1 |            -1.5 |                  0 |           21 |                5 |              3000 |             2.1 |       1.70627 |
|   2012 |       6 |                1 |            -0.1 |                  1 |           21 |                5 |              2550 |             2.2 |       1.93209 |
|   2015 |       7 |                1 |             1.2 |                  2 |           21 |                5 |              1740 |             2.2 |       1.6687  |


Dictionary for `'CLIMATE.CATEGORY'`:

{
    'normal': 1,
    'cold': 2,
    'warm': 3
}

Dictionary for `'CAUSE.CATEGORY'`:

{
    'severe weather': 1,
    'intentional attack': 2,
    'system operability disruption': 3,
    'equipment failure': 4,
    'public appeal': 5,
    'fuel supply emergency': 6,
    'islanding': 7
}

`U.S._STATE` with index bieng n+1(Example: Minnesota is 1 in the following data):

['Minnesota' 'Tennessee' 'Wisconsin' 'West Virginia' 'Michigan' 'Texas'
 'Indiana' 'Alabama' 'Mississippi' 'Illinois' 'Washington' 'Arizona'
 'Maryland' 'Pennsylvania' 'Kentucky' 'Utah' 'Ohio' 'North Carolina'
 'New Jersey' 'South Carolina' 'Oregon' 'District of Columbia' 'Missouri'
 'Delaware' 'Iowa' 'Montana' 'New York' 'Louisiana' 'Florida' 'California'
 'Arkansas' 'Virginia' 'Nebraska' 'Wyoming' 'New Mexico' 'Vermont'
 'Georgia' 'Connecticut' 'Oklahoma' 'Massachusetts' 'Maine'
 'New Hampshire' 'Nevada' 'Colorado' 'Kansas' 'Hawaii' 'Idaho'
 'North Dakota' 'South Dakota' 'Alaska']

---
## Baseline Model
***Pipeline***
Upon creating the pipeline, we used a column transformer called `OneHotEncoder` on  the `'CLIMATE.CATEGORY'`, and the `'CLIMATE.REGION'` column. We felt a OneHotEncoder was approrpriate simply because the columns used were representation of categories; there items in the column were not meant to be ranked.

***Performance***
To get the performance we used `pl_base.score(X_test_base, y_test_base)` which ended up giving us a value of:
`0.07572815533980583`

We felt that this was a decent score considering it was our first moddel.

---
## Final Model
***Pipeline***
For our updated pipeline, we started off with a 'QuantileTransformer()' on the `'ANOMALY.LEVEL'`, and the `'CAUSE.CATEGORY'` column.
We also  re-used a column transformer called `OneHotEncoder` on  the `'CLIMATE.CATEGORY'`, and the `'CLIMATE.REGION'` column; this was present in the original pipeline.

***Performance***
To get the performance of our final model, we used `pl_base.score(X_test_base, y_test_base)` which ended up giving us a value of:
`0.0854368932038835`

We felt it was a slight improvement. Similar to the baseline model, the results obtained can be generalized to unseen data because the model does not utilize any data that is not known at the time of prediction. This means that the model's performance and predictions can be considered applicable to new, unseen instances.

---
## Fairness Analysis

Here is a visualization of the false negative rates for `"ANAMOLY.LEVEL"`
It is a False Negative rate by weather anamoly as we felt it was important.
<iframe src="assets/fair1.html" width=800 height=600 frameBorder=0></iframe>


After completeing out Baseline and Final Model, we continued onto setting up our code for viewing our prediction with this dataframe:

| prior_2010   |   prediction |
|:-------------|-------------:|
| after_2010   |      2574.112211  |
| before_2010  |      3377.037736 |

Afterwards, it led us to another dataframe, this time for "accuracy": 

| prior_2010   |   accuracy |
|:-------------|-------------:|
| after_2010   |      0.135314  |
| before_2010  |      0.014151 |

Then next step was to find the "observed" value to find a signicant difference using this code:
`obs = results.groupby('prior_2010').apply(lambda x: metrics.accuracy_score(x['CAUSE.CATEGORY'], x['prediction'])).diff().iloc[-1]
obs`
after running, our `"obs"` value is `0.02527818093855827`

Afterwards, we do a hypothesis test. This is the output:
<iframe src="assets/difference.html" width=800 height=600 frameBorder=0></iframe>

Observed difference is not statistically significant and could be due to random chance. 
We "accept" the null hypothesis, indicating that there is no evidence of an unfair model.
