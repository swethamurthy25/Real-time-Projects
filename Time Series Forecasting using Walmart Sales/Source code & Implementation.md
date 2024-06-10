# Code Implementation and Results

### Reading the Walmart dataset:

```python
# Reading the Walmart dataset and displaying the first 10 rows
import pandas as pd
import numpy as np
walmart_sales = pd.read_csv('C:/Users/sweth/Desktop/ASU_IT/Fall 2022/IFT 512 - Advanced Big data Analysis/Project/walmart-sales-dataset-of-45stores.csv')
walmart_sales.head(10)
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/0fb759b6-fda3-4a29-a4b4-0b35879a9c19)

_____________________________________________________________________________________________________________________________________________________________


### Defining the basic function and metadata – to identify data frame shape and duplicate rows

```python
# Define Basic Functions
def get_metadata(dataframe):
    print("\nBASIC INFORMATION\n")
    print(dataframe.info())
    print("=" * 100)
    print("STATISTICAL INFORMATION")
    display(dataframe.describe(include='all'))
    print("=" * 100)
    print("Dataframe Shape\n", dataframe.shape)
    print("=" * 100)
    print("Number of Duplicate Rows\n", dataframe.duplicated().sum())
    print("=" * 100)
    print("NULL Values Check")
    for col in dataframe.columns:
        print(col, dataframe[col].isnull().sum())
    print("=" * 100)
    print("Sample Data")
    display(dataframe.head(3))
    print("=" * 100)
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/a9502500-d954-4f3a-ae17-1165ddd08dbf)

```python
get_metadata(walmart_sales)
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/fbe8bf32-1e54-479c-98d0-e1a2bc1a9b65)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/58308196-8de8-4f55-8f07-b17af96176b9)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/8d79e7e9-5257-4f89-84b4-ab12b93b5545)



_____________________________________________________________________________________________________________________________________________________________

## Value Distribution using seaborn and matplotlib

Here Each of the columns and its values is distributed in the form of a graph.

```python
# Value Distribution 
import seaborn as sns
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings('ignore')
pd.set_option('display.float_format', lambda x: '%.3f' % x)
walmart_sales.plot(subplots=True, grid=True, figsize=(15,15))
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/65e8e21e-8451-4392-9ba3-edee4f0952fa)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/9726b7ad-06fe-4fc6-9386-e508fd5a2c41)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/6f99acc6-9809-41f8-a8b6-1693461088d8)

________________________________________________________________________________________________________________________________________________________________

## Displaying all the 45 stores of Walmart and its weekly sales distribution

Here the x-axis of the graph contains the store details from 1 to 45 and the y-axis of the graph contains the weekly sales details.

```python
# Display stores and their weekly sales
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=walmart_sales, x="Store", y="Weekly_Sales", hue="Holiday_Flag", ax=ax, palette = "viridis")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/0dfc7a4d-21fb-4769-9b1b-01bc3fe4c1a6)

______________________________________________________________________________________________________________________________________________________________


## The tasks/questions that the project will address:

### Q1: Which stores have the maximum weekly sales?

Store 20 has the maximum weekly sales of 301397792.460

```python
#1. The store with Maximum Weekly Sales
store_sales_df=walmart_sales.groupby(['Store'], as_index=False).agg(Sum_of_Weekly_Sales=('Weekly_Sales','sum'))
store_sales_df[(store_sales_df['Sum_of_Weekly_Sales']==max(store_sales_df['Sum_of_Weekly_Sales']))]
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/5cf5c1fc-329d-40c0-b725-03d28a2ca804)

```python
# Visual representation of Weekly sales - Store 20 with max
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=store_sales_df, x="Store", y="Sum_of_Weekly_Sales", ax=ax)
#Conclusion : Store 20 has the Maximum Weekly Sales
print(" \n *******Que1 Results : Store 20 has the Maximum Weekly Sales*****")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/4699e228-1402-4fa2-8ae0-81e9e92a08da)


### Q2: In which stores do the sales vary a lot?

The weekly sales vary a lot in store 14

```python
#Q2: Store in which the Weekly Sales vary alot
# In this case, our objective is to find the Maximum Standard Deviation of Weekly Sales
std_stores=walmart_sales.groupby('Store',as_index=False).agg(Std_of_Weekly_Sales=('Weekly_Sales','std'))
std_stores[(std_stores['Std_of_Weekly_Sales'] == max(std_stores['Std_of_Weekly_Sales']))]
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/e8d96f81-b818-4359-b3f5-648e9919a119)

```python
# Visual representation of Weekly sales - Q2.Store 14 
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=std_stores, x="Store", y="Std_of_Weekly_Sales", ax=ax)
#Conclusion: The Weekly Sales vary a lot in Store 14
print(" \n *******Que2 Result : The Weekly Sales vary a lot in Store 14*****")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/5b175423-d989-41b8-85be-76f7f7240e10)


### Adding a column for the coefficient of variation

```python
#Add a column for the Coefficient of Variation
Stats_df=walmart_sales.groupby('Store', as_index=False).agg(Sales_sum=('Weekly_Sales','sum'), 
                                        Mean_Sales=('Weekly_Sales','mean'), 
                                        Std_Sales=('Weekly_Sales', 'std'),
                                        Sales_Variance=('Weekly_Sales','var'))
Stats_df
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/8204699a-d378-466c-a946-c8a57b9a0ef8)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/b987f69f-e28e-4a65-af82-0892264f3699)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/458658ea-5af1-4763-8e4e-30641d791761)

```python
stats_df['Coeff_of_Variation'] = (stats_df['Std_Sales'] / stats_df['Mean_Sales']) * 100
stats_df
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/5643d432-36f5-4fba-96ec-e07b93e04e66)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/091e430e-4021-41e9-93ae-0553a955ad71)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/bf543aba-be98-4c37-abba-5b74a4d557bb)


## Q3: Which stores have the maximum quarterly growth rate (Q3’2012)?

We must follow the steps to get the result:

* Assign Categories for every quarter in year Q1, Q2, Q3, Q4
* Filter out Rows for the year 2012
* Group By Store and Quarter, and aggregate with the sum for Weekly_Sales
* Find the Growth Rate for each quarter in a year using apply and Lambda Function
* Filter out Q3 category and find the Max % growth

```python
#Q3: Identify the Store with Good Quarterly Growth Rate in Q3'2012
walmart_sales['Year'] = pd.to_datetime(walmart_sales['Date'], format="%d-%m-%Y").dt.year
walmart_sales['Month'] = pd.to_datetime(walmart_sales['Date'], format="%d-%m-%Y").dt.month

def assign_quarter_category(month):
    if month <= 3:
        return 'Q1'
    elif month >= 4 and month <= 6:
        return 'Q2'
    elif month >=7 and month <= 9:
        return 'Q3'
    elif month >= 10 and month <= 12:
        return 'Q4'
    else:
        return np.nan

walmart_sales['Quarter'] = walmart_sales['Month'].apply(assign_quarter_category)
walmart_sales
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/dd3958ca-3ab2-406e-bfc0-9689694f118e)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/9856385d-5138-44b6-8e0b-805a94b805e9)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/463152e4-9831-4366-9a2f-bfffe71c9fcb)


```python
# Dropping the columns month and year. Display only the 2012'Quarter
walmart_sales_2012 = walmart_sales[(walmart_sales['Year']) == 2012]
walmart_sales_2012.drop(['Month', 'Year'], axis='columns', inplace=True)
walmart_sales_2012
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/76d15433-328b-4afb-a1e0-5ae785611e9b)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/73d837a0-dcb4-48ea-a2f9-58e98bb00fcb)


```python
walmart_sales_2012.describe(include='all')
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/d439d933-98ec-4472-a3e8-b62a03d4ea7d)


```python
# Quarter-wise sales-2012
q_sales=walmart_sales_2012.groupby(["Store", "Quarter"]).agg(Quarterwise_Sales=('Weekly_Sales', 'sum'))
q_sales
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/49ab6ef8-14f4-4217-af4c-5a7bb9b4dd5d)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/8673e88d-f8bb-4dd7-9a58-74779019b731)


```python
q_sales.plot(kind = 'bar', grid=True, figsize=(200, 30))
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/13b495c2-def6-4237-8fa8-ebec4029cc38)

```python
# Display only the data of Q3
quarterly_percentage = q_sales.groupby(level=0).apply(lambda x: 100 * x / float(x.sum()))
quarterly_percentage.reset_index(inplace=True)
Q3 = quarterly_percentage[(quarterly_percentage['Quarter']) == 'Q3']
Q3
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/f204689e-86eb-48c2-99dd-56e844cf92d0)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/8e6f8da0-2736-47a2-b18f-8846ffd973e5)

```python
#Which stores have the maximum quarterly growth rate (Q3’2012)?
Q3[(Q3['Quarterwise_Sales'] == max(Q3['Quarterwise_Sales']))]
# Visual Representation
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=Q3, x="Store", y="Quarterwise_Sales", ax=ax)
#Conclusion : Store 7 has Good Quarterly Growth Rate in Q3' 2012
print(" \n *******Que3 Results : Store 7 has Good Quarterly Growth Rate in Q3' 2012*****")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/037d1d82-b0c6-4607-8365-396a75a5d41e)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/c06302c3-a4c4-43e8-b13b-cd32e2c9193e)


## Some holidays have a negative impact on sales. Find out the holidays that have higher sales than the mean sales in the non-holiday season for all stores together.

```python
def assign_holiday(date):
    if date in ['12-02-2010', '11-02-2011', '10-02-2012', '08-02-2013']:
        return 'Super Bowl'
    elif date in ['10-09-2010', '09-09-2011', '07-09-2012', '06-09-2013']:
        return 'Labour Day'
    elif date in ['26-11-2010', '25-11-2011', '23-11-2012', '29-11-2013']:
        return 'Thanksgiving'
    elif date in ['31-12-2010', '30-12-2011', '28-12-2012', '27-12-2013']:
        return 'Christmas'
    else:
        return 'Non-Holiday' 
    
holiday_analysis_df = walmart_sales.drop(['Store', 'Year', 'Month'], axis='columns')
holiday_analysis_df['Holiday'] = holiday_analysis_df['Date'].apply(assign_holiday)
holiday_analysis_df
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/0d7c36fb-5900-433d-bdc2-6a708433b2e9)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/e3a197ef-6642-4e17-9e00-0e18806bec1e)


```python
#Finding the overall sales on holidays
mean_df = holiday_analysis_df.groupby('Holiday').agg(Mean_Weekly_Sales=('Weekly_Sales','mean'))
mean_df
#Que4 Results: Labour Day Week, Super Bowl Week, and Thanksgiving Week have a negative impact on Sales, 
#Which means they have higher sales than the Mean of Non-Holiday Week sales*****")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/46e3f2d5-664b-4c04-8328-46e95e5ad39d)


```python
mean_df.plot(kind='bar', y='Mean_Weekly_Sales', figsize=(8,8))
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/7c4d4cc4-26a2-4b49-9584-251f20019fc2)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/907ed632-a9de-4852-9dd5-dd0ca1b64a36)


```python
#Grouping the sales by Quarter and check for the sales on holidays
holiday_analysis_df.groupby('Quarter').agg({'Weekly_Sales':'std'})
pd.pivot_table(holiday_analysis_df, index=["Quarter",'Holiday'], aggfunc={'Weekly_Sales':np.mean})
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/73e01883-e0c6-4aff-adbb-2a030f915187)

```python
#SUM OF WEEKLY SALES IS HIGHER IN NON-HOLIDAY WEEKS
pd.pivot_table(holiday_analysis_df, index=["Holiday_Flag"], aggfunc={'Weekly_Sales':'sum', 'CPI':np.mean})
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/18b0d7b9-b225-4b97-88ed-306d8d6d4fbe)


## Q5: Which quarter has the highest and lowest unemployment rate?

Q1 has the highest unemployment rate.

```python
#Q5: Which quarter has the highest and lowest unemployment rate?
q_unemp=walmart_sales.groupby('Quarter', as_index=False).agg(Mean_Unemployment_Rate=('Unemployment','mean'))
q_unemp
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/82abf3a2-62ba-4fde-8f1c-7506a248bd2b)


```python
#Visual Representation
fig, ax = plt.subplots(figsize=(5,5))
sns.barplot(data=q_unemp, x="Quarter", y="Mean_Unemployment_Rate", ax=ax)
#Consultion/Results
print("Q1 has higher Unemployment Rate and Q3 has lower Unemployment Rate")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/f6723dfa-c7d8-4c58-a4ee-e5471bad769c)

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/12e1b04a-df7f-4694-a687-c35934fb0693)


____________________________________________________________________________________________________________________________________________________________
