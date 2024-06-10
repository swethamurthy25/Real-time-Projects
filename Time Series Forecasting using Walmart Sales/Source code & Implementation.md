# Code Implementation and Results

### Reading the Walmart dataset:

```python
# Reading the Walmart dataset and displaying the first 10 rows
import pandas as pd
import numpy as np
walmart_sales = pd.read_csv('C:/Users/sweth/Desktop/ASU_IT/Fall 2022/IFT 512 - Advanced Big data Analysis/Project/walmart-sales-dataset-of-45stores.csv')
walmart_sales.head(10)
```

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

```python
get_metadata(walmart_sales)
```

## Value Distribution using seaborn and matplotlib

```python
# Value Distribution 
import seaborn as sns
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings('ignore')
pd.set_option('display.float_format', lambda x: '%.3f' % x)
walmart_sales.plot(subplots=True, grid=True, figsize=(15,15))
```

## Displaying all the 45 stores of Walmart and its weekly sales distribution

Here the x-axis of the graph contains the store details from 1 to 45 and the y-axis of the graph contains the weekly sales details.

```python
# Display stores and their weekly sales
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=walmart_sales, x="Store", y="Weekly_Sales", hue="Holiday_Flag", ax=ax, palette = "viridis")
```

## The tasks/questions that the project will address:

### Q1: Which stores have the maximum weekly sales?

Store 20 has the maximum weekly sales of 301397792.460

```python
#1. The store with Maximum Weekly Sales
store_sales_df=walmart_sales.groupby(['Store'], as_index=False).agg(Sum_of_Weekly_Sales=('Weekly_Sales','sum'))
store_sales_df[(store_sales_df['Sum_of_Weekly_Sales']==max(store_sales_df['Sum_of_Weekly_Sales']))]
```

```python
# Visual representation of Weekly sales - Store 20 with max
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=store_sales_df, x="Store", y="Sum_of_Weekly_Sales", ax=ax)
#Conclusion : Store 20 has the Maximum Weekly Sales
print(" \n *******Que1 Results : Store 20 has the Maximum Weekly Sales*****")
```

### Q2: In which stores do the sales vary a lot?

The weekly sales vary a lot in store 14

```python
#Q2: Store in which the Weekly Sales vary alot
# In this case, our objective is to find the Maximum Standard Deviation of Weekly Sales
std_stores=walmart_sales.groupby('Store',as_index=False).agg(Std_of_Weekly_Sales=('Weekly_Sales','std'))
std_stores[(std_stores['Std_of_Weekly_Sales'] == max(std_stores['Std_of_Weekly_Sales']))]
```

```python
# Visual representation of Weekly sales - Q2.Store 14 
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=std_stores, x="Store", y="Std_of_Weekly_Sales", ax=ax)
#Conclusion: The Weekly Sales vary a lot in Store 14
print(" \n *******Que2 Result : The Weekly Sales vary a lot in Store 14*****")
```

### Adding a column for the coefficient of variation

```python
#Add a column for the Coefficient of Variation
Stats_df=walmart_sales.groupby('Store', as_index=False).agg(Sales_sum=('Weekly_Sales','sum'), 
                                        Mean_Sales=('Weekly_Sales','mean'), 
                                        Std_Sales=('Weekly_Sales', 'std'),
                                        Sales_Variance=('Weekly_Sales','var'))
Stats_df
```



```python
stats_df['Coeff_of_Variation'] = (stats_df['Std_Sales'] / stats_df['Mean_Sales']) * 100
stats_df
```


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

```python
# Dropping the columns month and year. Display only the 2012'Quarter
walmart_sales_2012 = walmart_sales[(walmart_sales['Year']) == 2012]
walmart_sales_2012.drop(['Month', 'Year'], axis='columns', inplace=True)
walmart_sales_2012
```

```python
walmart_sales_2012.describe(include='all')
```

```python
# Quarter-wise sales-2012
q_sales=walmart_sales_2012.groupby(["Store", "Quarter"]).agg(Quarterwise_Sales=('Weekly_Sales', 'sum'))
q_sales
```

```python
q_sales.plot(kind = 'bar', grid=True, figsize=(200, 30))
```

```python
# Display only the data of Q3
quarterly_percentage = q_sales.groupby(level=0).apply(lambda x: 100 * x / float(x.sum()))
quarterly_percentage.reset_index(inplace=True)
Q3 = quarterly_percentage[(quarterly_percentage['Quarter']) == 'Q3']
Q3
```


```python
#Which stores have the maximum quarterly growth rate (Q3’2012)?
Q3[(Q3['Quarterwise_Sales'] == max(Q3['Quarterwise_Sales']))]
# Visual Representation
fig, ax = plt.subplots(figsize=(20,5))
sns.barplot(data=Q3, x="Store", y="Quarterwise_Sales", ax=ax)
#Conclusion : Store 7 has Good Quarterly Growth Rate in Q3' 2012
print(" \n *******Que3 Results : Store 7 has Good Quarterly Growth Rate in Q3' 2012*****")
```
## Some holidays hurt sales. Find out the holidays that have higher sales than the mean sales in the non-holiday season for all stores together.

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

```python
#Finding the overall sales on holidays
mean_df = holiday_analysis_df.groupby('Holiday').agg(Mean_Weekly_Sales=('Weekly_Sales','mean'))
mean_df
#Que4 Results: Labour Day Week, Super Bowl Week, and Thanksgiving Week hurt Sales, 
#Which means they have higher sales than the Mean of Non-Holiday Week sales*****")
```

![image](https://github.com/swethamurthy25/-Time-Series-Forecasting-using-Walmart-Dataset/assets/112581595/46e3f2d5-664b-4c04-8328-46e95e5ad39d)


```python
mean_df.plot(kind='bar', y='Mean_Weekly_Sales', figsize=(8,8))
```

```python
#Grouping the sales by Quarter and check for the sales on holidays
holiday_analysis_df.groupby('Quarter').agg({'Weekly_Sales':'std'})
pd.pivot_table(holiday_analysis_df, index=["Quarter",'Holiday'], aggfunc={'Weekly_Sales':np.mean})
```

```python
#SUM OF WEEKLY SALES IS HIGHER IN NON-HOLIDAY WEEKS
pd.pivot_table(holiday_analysis_df, index=["Holiday_Flag"], aggfunc={'Weekly_Sales':'sum', 'CPI':np.mean})
```

## Q5: Which quarter has the highest and lowest unemployment rate?

Q1 has the highest unemployment rate.

```python
#Q5: Which quarter has the highest and lowest unemployment rate?
q_unemp=walmart_sales.groupby('Quarter', as_index=False).agg(Mean_Unemployment_Rate=('Unemployment','mean'))
q_unemp
```

```python
#Visual Representation
fig, ax = plt.subplots(figsize=(5,5))
sns.barplot(data=q_unemp, x="Quarter", y="Mean_Unemployment_Rate", ax=ax)
#Consultion/Results
print("Q1 has higher Unemployment Rate and Q3 has lower Unemployment Rate")
```

