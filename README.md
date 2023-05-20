# Power Outages: Seasonal or Not? 
By Aditya Agrawal 

## Introduction:
Welcome to my exploratory data analysis project, where we dive into the world of power outages in the continental U.S. between January 2000 and July 2016. My main aim is to answer the question: 
> When do major power outages tend to occur? 
By understanding the spatial and temporal distribution of these outages, we can hope to shed light on the factors that might contribute to power disruptions, and inform strategies for improving the resilience of the power grid. By default, there are *1534 rows and 55 columns in this dataframe*. As you will see below we will clean, organize and analyze this data. There are many columns available to us but we will not use all of them. Here are the relevant ones and their meanings:
# TODO: 
`OUTAGE.START.DATE`,
`OUTAGE.START.TIME`,
`OUTAGE.RESTORATION.DATE`,
`OUTAGE.RESTORATION.TIME`, 


---

## Cleaning and Exploratory Data Analysis:
### Data Cleaning
The first step of our process to analyze this data is cleaning it and formatting it properly. Here are the detailed steps: 
 - Importing the data and removing certain extraneous columns and rows. 
 - Data was given as excel sheet so using pd.read_excel to read data
 - `OUTAGE.START.DATE`, and `OUTAGE.START.TIME` are columns that represent power outage start date and time. They are more helpful if made and and so they are combined into one new `pd.Timestamp` column called '`OUTAGE.START`'. 
 - The same is done for `OUTAGE.RESTORATION.DATE`, and `OUTAGE.RESTORATION.TIME` into `OUTAGE.RESTORATION`. For both these merges were achieved by covert the columns to datetime and timedelta objects, then simply adding them and assign to new column. The four merged columns were dropped, and with two being added we now have 53 columns total.  
 - New columns for the day of the week, and hour were created. They are called `DAY.OF.WEEK` and `HOUR`, respectively. They were extracted from `OUTAGE.START` by datetime attributes. These we used in univariate analysis in my personal analysis on my notebook. Now we have 55 columns again. 
Here is the head of the newly cleaned beautiful dataframe:

|   YEAR |   MONTH |   DAY.OF.WEEK |   HOUR | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |
|-------:|--------:|--------------:|-------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|
|   2011 |       7 |             4 |     17 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|   2014 |       5 |             6 |     18 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|   2010 |      10 |             1 |     20 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|   2012 |       6 |             1 |      4 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|   2015 |       7 |             5 |      2 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |

---

### Univariate Analysis
Now it is time for Univariate Analysis. Looking at the plots below we can see the distribution of data in a single column. Lets take a look: 

<iframe src="assets/month_only_chart.html" width=800 height=600 frameBorder=0></iframe>
The bar chart I created showcases the distribution of power outages by month, allowing me to observe the frequency of outages occurring in each month. It provides valuable insights into which months experience higher or lower outage counts, aiding in understanding seasonal patterns and potential factors influencing power disruptions. Seems high during months 6-8. 

<iframe src="assets/outage_chart.html" width=800 height=600 frameBorder=0></iframe>
For the Power Outage Duration Distribution histogram, I plotted the distribution of power outage durations in minutes. It gives me insights into the frequency of different duration ranges, allowing me to understand the typical duration of power outages. Most outages are small and there are a few outliers but I did my research to make sure they are legit. Not outliers but crazy long outages. 

<iframe src="assets/cause_chart.html" width=800 height=600 frameBorder=0></iframe>
In the Distribution of Power Outage Causes bar chart, I visualized the count of power outages categorized by their specific cause details. It helps me identify the most common causes of power outages, providing valuable information for improving grid resilience and addressing the identified issues. I did this plot for fun moslty and thought to share because it was intresting to see Vandalism as such a high cause. Maybe something investigate for another time!

---

### Bivariate Analysis
Now it is time for Bivariate Analysis. Looking at the plots below we can see the the relations and help identify trends early on. Lets take a look: 

<iframe src="assets/month_boxplot.html" width=800 height=600 frameBorder=0></iframe>
The box plot I created demonstrates the distribution of power outage durations across different months. It allows me to visualize the median, quartiles, and outliers, providing insights into the variation in outage durations throughout the year. This information can help identify months with consistently longer or shorter outages, aiding in further investigations for identify seasons with higher or lower frequencies of power outages.

---

### Interesting Aggregates

|   MONTH |    mean |   count |
|--------:|--------:|--------:|
|       1 | 3387.95 |     133 |
|       2 | 2497.14 |     132 |
|       3 | 3265.89 |      94 |
|       4 | 1493.86 |     107 |
|       5 | 2077.29 |     119 |
|       6 | 1948.4  |     186 |
|       7 | 2218.99 |     177 |
|       8 | 2428.48 |     151 |
|       9 | 4294.52 |      92 |
|      10 | 3600.94 |     108 |
|      11 | 1728.16 |      69 |
|      12 | 3293.79 |     108 |

Explanation: This table shows the average outage duration and count for each month. It can provide insights into whether outages tend to be longer or shorter in certain months.

---

## Missingness Analysis 
### NMAR Analysis
Here I am going to explain the NMAR analysis of my date. Not Missing At Random (NMAR) refers to a condition when the missingness of data is related to the actual missing values.

Based on the DataFrame's info: 
<details>
<summary>INFO</summary>

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1534 entries, 0 to 1533
Data columns (total 55 columns):
 #   Column                 Non-Null Count  Dtype         
---  ------                 --------------  -----         
 0   YEAR                   1534 non-null   int64         
 1   MONTH                  1525 non-null   float64       
 2   DAY_OF_WEEK            1525 non-null   float64       
 3   HOUR                   1525 non-null   float64       
 4   U.S._STATE             1534 non-null   object        
 5   POSTAL.CODE            1534 non-null   object        
 6   NERC.REGION            1534 non-null   object        
 7   CLIMATE.REGION         1528 non-null   object        
 8   ANOMALY.LEVEL          1525 non-null   float64       
 9   CLIMATE.CATEGORY       1525 non-null   object        
 10  CAUSE.CATEGORY         1534 non-null   object        
 11  CAUSE.CATEGORY.DETAIL  1063 non-null   object        
 12  HURRICANE.NAMES        72 non-null     object        
 13  OUTAGE.DURATION        1476 non-null   float64       
 14  DEMAND.LOSS.MW         829 non-null    float64       
 15  CUSTOMERS.AFFECTED     1091 non-null   float64       
 16  RES.PRICE              1512 non-null   float64       
 17  COM.PRICE              1512 non-null   float64       
 18  IND.PRICE              1512 non-null   float64       
 19  TOTAL.PRICE            1512 non-null   float64       
 20  RES.SALES              1512 non-null   float64       
 21  COM.SALES              1512 non-null   float64       
 22  IND.SALES              1512 non-null   float64       
 23  TOTAL.SALES            1512 non-null   float64       
 24  RES.PERCEN             1512 non-null   float64       
 25  COM.PERCEN             1512 non-null   float64       
 26  IND.PERCEN             1512 non-null   float64       
 27  RES.CUSTOMERS          1534 non-null   int64         
 28  COM.CUSTOMERS          1534 non-null   int64         
 29  IND.CUSTOMERS          1534 non-null   int64         
 30  TOTAL.CUSTOMERS        1534 non-null   int64         
 31  RES.CUST.PCT           1534 non-null   float64       
 32  COM.CUST.PCT           1534 non-null   float64       
 33  IND.CUST.PCT           1534 non-null   float64       
 34  PC.REALGSP.STATE       1534 non-null   int64         
 35  PC.REALGSP.USA         1534 non-null   int64         
 36  PC.REALGSP.REL         1534 non-null   float64       
 37  PC.REALGSP.CHANGE      1534 non-null   float64       
 38  UTIL.REALGSP           1534 non-null   int64         
 39  TOTAL.REALGSP          1534 non-null   int64         
 40  UTIL.CONTRI            1534 non-null   float64       
 41  PI.UTIL.OFUSA          1534 non-null   float64       
 42  POPULATION             1534 non-null   int64         
 43  POPPCT_URBAN           1534 non-null   float64       
 44  POPPCT_UC              1534 non-null   float64       
 45  POPDEN_URBAN           1534 non-null   float64       
 46  POPDEN_UC              1524 non-null   float64       
 47  POPDEN_RURAL           1524 non-null   float64       
 48  AREAPCT_URBAN          1534 non-null   float64       
 49  AREAPCT_UC             1534 non-null   float64       
 50  PCT_LAND               1534 non-null   float64       
 51  PCT_WATER_TOT          1534 non-null   float64       
 52  PCT_WATER_INLAND       1534 non-null   float64       
 53  OUTAGE.START           1525 non-null   datetime64[ns]
 54  OUTAGE.RESTORATION     1476 non-null   datetime64[ns]
dtypes: datetime64[ns](2), float64(35), int64(10), object(8)
memory usage: 659.3+ KB
```
</details>

there are several columns with missing data. Data like `HURRICANE.NAMES` is clearly MD becaues it is not reported if there is no hurricane. For other columns, it's hard to definitively say without more information about the data generating process and context, but let's consider a column for this NMAR discussion: `OUTAGE.DURATION`. 

Hypothetically, the `OUTAGE.DURATION` could be Not Missing At Random (NMAR) if the duration of an outage is not recorded because it was too long or too short. In this case, the fact that the data is missing (the absence of recorded duration) is related to the actual missing value (the actual duration of the outage). 

To verify or disprove this NMAR assumption, i might want to investigate further the context in which data was missing. For instance, talking to people who were involved in the data collection process could provide valuable insights. 

### Missingness Dependency
Based on the dataset, I chose the first looked at the Non-Null Count of my dataset and found the `DEMAND.LOSS.MW` with only *829 non-null* as a good column to analyze for missingness dependency. In my analysis of the missingness in the `DEMAND.LOSS.MW` column of our dataset, I conducted permutation tests with the `OUTAGE.DURATION` and `YEAR` columns. 

My results showed me that the missingness in DEMAND.LOSS.MW was not dependent on OUTAGE.DURATION. The p-value from our permutation test was approximately 0.329, indicating that there is insufficient evidence to reject the null hypothesis. This suggests that the duration of outages does not play a significant role in the recording of demand loss in megawatts.

However, when I analyzed the dependency of the missingness in DEMAND.LOSS.MW on YEAR, we found significant results. The p-value was 0, indicating strong evidence against the null hypothesis. This suggests that the year of the outage does affect the recording of demand loss in megawatts.

These findings have important implications for our understanding of the data. It indicates that the recording of demand loss is not random, but is instead influenced by certain variables.

In the figure below, I present the empirical distribution of the test statistic from our permutation test, along with the observed statistic. This visualization further illustrates the dependency of the missingness in DEMAND.LOSS.MW on YEAR.
<iframe src="assets/missing1.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/missing2.html" width=800 height=600 frameBorder=0></iframe>

---

## Hypothesis Testing
As per your main question - "where and when do major power outages tend to occur?", we can formulate a hypothesis related to the time of the occurrence of power outages.

Let's consider the hypothesis that power outages are more frequent in winter months (December, January, February) than in summer months (June, July, August).

H0 (Null Hypothesis): The number of power outages in winter is the same as in summer.
H1 (Alternative Hypothesis): The number of power outages in winter is higher than in summer.

We will use a permutation test to validate our hypothesis, and we'll set our significance level at 0.05.

