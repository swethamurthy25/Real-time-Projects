# Time-Series-Forecasting-using-Walmart-Dataset

## Project Description

Time has been an important factor ever since we first started collecting data. A specific method of studying a collection of data points over a period is called a time series analysis. Organizations can fully comprehend the underlying causes of long-term trends or systemic patterns by applying time series analysis. When they examine data at regular intervals, they can use time series forecasting to estimate the probability of future events and it is also used to highlight likely variations in the data, such as seasonality or cyclical activity, which enhances forecasting by enabling a better understanding of the data variables. 

####  About Walmart Dataset
To accomplish this, we have chosen the Walmart dataset, which contains data on 45 stores’ weekly sales for the years 2010–2012. This dataset also includes information on factors that affect sales, such as holidays, weather, fuel prices, CPI, and unemployment. By performing a predictive analysis on the Walmart dataset, we can better comprehend the data and trends, which in turn allows us to forecast future events.

As of July 31, 2022, Walmart has 10,585 stores and clubs in 24 countries operating under 46 different names. Among them, 45 stores were picked up for fundamental analysis. According to the May 2022 Fortune Global 500 list, Walmart is the world's top-grossing company with annual sales of approximately $570 billion.

* The Walmart dataset we are using consists of 8 columns and 6,436 rows.
* Each of the columns represents different sales aspects of the Walmart dataset.

The 8 columns of the Walmart dataset are explained briefly: 

* Store numbers: Since we have considered a dataset of 45 stores, we now use the dataset from 45 stores to be the total number of stores.
* Date: It is the week of sales; it is in the format of dd-mm-yyyy.
* Weekly sales: the sales of the entire store in a week.
* Holiday flag: We consider the holidays for each week, if it is a holiday, then it is represented by a 1, if not then it is represented as a 0.
* Temperature: Average temperature of the week of sales.
* Fuel price: Fuel price in the region of the store
* CPI: Customer price index
* Unemployment: An unemployment rate of that specific store in that specific region 

___________________________________________________________________________________________________________________________________

## Actionable decision(s) that we can anticipate from our project

We will be analyzing the sales trends of the Walmart dataset and will address the below questions:

* Which stores have the maximum weekly sales?
* Among the 45 stores, in Which store do the sales vary a lot?
* Which stores have the maximum quarterly growth rate (Q3’2012)?
* Which stores have the highest sales fluctuation over the considered period?
* Which holiday week has the highest negative impact on sales?
* Which quarter has the highest and lowest unemployment rate?
_________________________________________________________________________________________________________________________________________

## Proposed Job pipeline flow

The CRISP-DM steps are popularly known as the cross-industry standard process for data mining. It is one type of process model that acts as a base for any data science process. There are six sequential phases namely:

### Business Understanding

The first phase is about understanding the project requirements and objectives from a business perspective. In our project, we identified the business problem (sales churn), evaluated the situation, and then determined the questions we wanted to address after conducting the data analysis. The Problem statement here is about Walmart sales forecasting and predictions using time series analysis.

### Data Understanding

The second step is data understanding where any quality issues pertaining to the data are identified. It is also the phase in which an analyst goes on to find interesting subsets to form hypotheses for hidden information. Here, we have chosen the appropriate dataset relevant to sales forecasting. The dataset also includes information on factors that affect sales, such as holidays, weather, fuel prices, CPI, and unemployment. 

### Data Preparation

The data preparation phase covers all the activities to build the final dataset from raw data. To perform the sales forecasts on our dataset, we would need to eliminate the null values to change the data into a processing format. This project will identify the three most important aspects - seasonality, autocorrelation, and stationarity.

### Data Modelling

It is the phase in which an analyst decides on the appropriate modeling techniques.  In this project, we are going to use descriptive analysis which is used to identify the patterns in time series data like trends, cycles, or seasonal variations. Additionally, we are going to use a forecasting model that uses historical data as a model for future data, predicting scenarios that could happen along the future plot points. Exploratory analysis is used to highlight the main characteristics of time series data in the visual format.

### Model Evaluation

The analyst here selects the models that fit the best for the business objectives. The steps - assess the outcomes, examine the process, and choose the future steps are iterative, and we will go back and forth until the below-mentioned questions are satisfied. At last, we must ensure that the modeling strategy resolves the initial business requirement.

### Deployment

It is the final stage where the chosen models are put into use to predict the outcome. Based on the predictors, this phase primarily aims to determine if a certain store's sales will increase or not.
_______________________________________________________________________________________________________________________________________

## Proposed estimators and models to be employed

To analyze current data trends and forecast future events, there are numerous time series analysis models and techniques available. Time series analysis involves a wide range of data categories and variances; thus, analysts occasionally must create intricate models. We cannot generalize a particular model to all samples or account for all variations. A lack of fit may result from models that are overly complicated or that attempt to perform too many things. Models that are poorly fitted or overfit fail to discern between real relationships and random error, skewing analysis and producing inaccurate forecasts.

We are using the combinations of the below models in our project:

### Classification: 
We have identified and classified the data into categories.

### Descriptive Analysis:
We identified the patterns in time series data like trends, cycles, or seasonal variations.

### Explanative Analysis: 
We tried to comprehend the data, the links between the data, and cause and effect.

### Exploratory Analysis: 
We highlighted the main characteristics of the time series data in a visual format.

_______________________________________________________________________________________________________________________________________

## Summary

This project will focus on understanding a specific way of analyzing a sequence of data points gathered over a period. The time series analysis deals with data points at consistent intervals over a specific period rather than just recording the data points randomly. This project utilizes data visualization techniques for users to see seasonal trends and the reasons why these trends occur. It also resonates with analyzing data over consistent intervals for time series forecasting to predict the occurrence of future events. This is also a part of predictive analytics. Further, this project goes on to find the three most important aspects seasonality, autocorrelation, and stationarity, where seasonality is the periodic fluctuations, autocorrelation is the similarity between observations as a function of the time lag between them, and stationarity checks for constant mean and variance and whether covariance is independent of time or not. 
_______________________________________________________________________________________________________________________________

## Infrastructure components (Technologies used)

* The data analysis will be carried out in a Jupyter Notebook using Python.
* Effective libraries are available in Jupyter for creating regression models.
* Data processing is also simple and fast using Python.
* Pre-processing of the data will be required, and Python can make it simple to clean and process the data.
* With the aid of libraries like Seaborn that use Matplotlib, Jupiter allows visualizations and provides rendering for some data 
  sets, including visuals and charts that are generated from codes.
* With Jupyter, users may share their code and data sets with others and narrate their visualizations, allowing for interactive 
  improvements.

_________________________________________________________________________________________________________________________

## References:

Varsharam. (2022, September 20). Timeseries-analysis. Kaggle. Retrieved December 3, 2022, from https://www.kaggle.com/code/varsharam/timeseries-analysis

Time series analysis: Definition, types, techniques, and when it's used. Tableau. (n.d.). Retrieved December 3, 2022, from https://www.tableau.com/learn/articles/time-series-analysis

Sign in. RPubs. (n.d.). Retrieved December 3, 2022, from https://rpubs.com/maupatel7/WalmartTimeSeries

6.4. introduction to time series analysis. (n.d.). Retrieved December 3, 2022, from https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm


