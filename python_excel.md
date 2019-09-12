
**Project description:** 

In this project we obtained the Dow Jones Industrial Average data from 2014 to 2019. The Dow Jones Industrial Average (DJIA), or simply the Dow (/ˈdaʊ/), is a stock market index that indicates the value of 30 large, publicly owned companies based in the United States, and how they have traded in the stock market during various periods. (Wikipedia)

Our goal is to expand the dataset by calculating new metrics for each exisiting column. The new metrics are: Percent Change, Absolute Change, Three Period Compound Growth, Five Period Compound Growth, Three Period Moving Average, Five Period Moving Average. 

For example, for the column of Close Price in the raw data, we would calculate the following metrics: Close Price Percent Change, Close Price Absolute Change, Close Price Three Period Compound Growth, Close Price Five Period Compound Growth, Close Price Three Period Moving Average, Close Price Five Period Moving Average. 

The raw data contains 64 columns. Each row represent the stock transaction data recorded for a certain company at a specific date from 2014 to 2019. The Date, Symbol, Name, Sector, and Industry columns contains the basic information of each data record. 

The remaining 59 columns are stock transaction data and company cash flow data such as 'Close Price','Volume', 'Inventory', 'Long-Term Debt', 'Net Receivables','Property Plant Equipment', 'Short-Term Debt', 'Total Assets','Total Current Assets', 'Total Current Liabilities'...'4-Week Rolling Mean Minus Current Price','4-Week Standard Deviation', 'IndexDO', '3-Month IndexDO'. 

The goal of this project is to automate the dataset expansion process in Python and write to Excel through Python. 
<br/><br/>


### 1. Data Preparation

Importing data. Read in the data from the Excel worksheet as a dataframe. 

```
import pandas as pd
file = 'level3_data_DJIA.xlsx'
xl = pd.ExcelFile(file)
print(xl.sheet_names)
df = xl.parse('level3_data_DJIA.csv')
```

The following is the head of the dataset, the columns are not completely included due to the page limit. 

|   |Date|Symbol|Name|Sector|Industry|Close Price, $|Volume|Inventory, $|...56 More Columns|
|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
|0	|2014-08-29|MMM|3M Company|Industrials|Diversified Industrials|126.2318|1537800|3.945000e+09|...|
|1	|2014-09-05|MMM|3M Company|Industrials|Diversified Industrials|126.4072|1989100|3.945000e+09|...|
|2	|2014-09-12|MMM|3M Company|Industrials|Diversified Industrials|126.1793|2135400|3.945000e+09|...|
|3	|2014-09-19|MMM|3M Company|Industrials|Diversified Industrials|128.5899|8106200|3.945000e+09|...|
|4	|2014-09-26|MMM|3M Company|Industrials|Diversified Industrials|124.8468|3557700|3.945000e+09|...|

Sometimes, numeric data in Excel might exist as strings instead of floats or integers. Let's first make sure the data types of all columns are appropriate. 

```
df.info()
df['Net Profit Margin, %'] = pd.to_numeric(df1['Net Profit Margin, %'], errors = 'coerce')
```

Create a new dataframe to temporarily store all columns of variables that need to be expanded.

```
metrics = df1.loc[:,'Close Price, $':]
```

The idea is to define a helper function that takes in a single column and return a new calculation, then create a second function that takes in a dataframe and apply the helper function to each column of the entire dataframe.

<img src="images/python_excel_graphs/dataframe apply.jpg?raw=true"/>


To calculate a Percent Change, we used .shift() which allows us to shift all rows in a column to the positions of the next n rows. For example, dataframe.shift(2, axis = 0) 

<img src="images/python_excel_graphs/shift .png?raw=true"/>


```
def expand_single_column_percent_change(col):
  return (col - col.shift(1))/col.shift(1)
  
def expand_data_percent_change(df):
    return df.apply(expand_single_column_percent_change)
```


We can now create a new dataframe percent_change that contains the new calculations of percent change for all columns in metrics. 

```
percent_change = expand_data_percent_change(metrics)
```
<br/><br/>

We can add 'Percent Change' to the column names in percent_change. The new dataframe percent_change ends up with column names such as 'Close Price, $, Percent Change','Volume, Percent Change' ... ''3-Month IndexDO', Percent Change'. 

```
percent_change = percent_change.add_suffix(', Percent Change') 
```

Same procedures can be applied to 3 Period/5 Period Compound Growth Rate and 3 Period/5 Period Moving Average calculations. 

Compound growth rate is the growth rate from the initial period value to the ending period value, assuming the growth is compounding over time. Its calculation: 


<img src="images/python_excel_graphs/cagr.png?raw=true"/>
(https://www.gartner.com/it-glossary/cagr-compound-annual-growth-rate)

The new dataframe for Compound Growth Rate can be calculated through the followng way, using the 5 period Coumpound Growth as an example:

```
def expand_single_column_five_compound_growth(col):
    return (col/col.shift(5))**(1/5) - 1

def expand_data_five_compound_growth(df):
    return df.apply(expand_single_column_five_compound_growth)
    
#create new table
five_compound_growth = expand_data_five_compound_growth(metrics)
#change column name
five_compound_growth = five_compound_growth.add_suffix(', Five Period Compound Growth')
```

Similaryly for Moving Average, using the 5 Period Moving Average as an example, which is the average of the previous two periods, the current period, and the next two periods. 

```
def expand_single_column_five_moving_average(col):
    return (col.shift(-2) + col.shift(-1) + col.shift(0) + col.shift(1) + col.shift(2))/5
    
def expand_data_five_moving_average(df):
    return df.apply(expand_single_column_five_moving_average)
```

Now we have all the variables we want to create in separate dataframes we can potentially write them into an Excel workbook either by putting each dataframe in a tab or combined all dataframes in a single sheet. 

Use the following method to write each dataframe into an Excel workbook named "level3_data_DJIA_python_by_variable.xlsx", sheet_name will become tab names. 

```
datatoexcel = pd.ExcelWriter('level3_data_DJIA_python_by_variable.xlsx', engine = 'xlsxwriter')

#sheet_name will become Excel tab name
percent_change.to_excel(datatoexcel, sheet_name= 'Percent Change')
absolute_change.to_excel(datatoexcel, sheet_name= 'Absolute Change')
three_compound_growth.to_excel(datatoexcel, sheet_name= '3 Period Compound Growth')
five_compound_growth.to_excel(datatoexcel, sheet_name= '5 Period Compound Growth')
three_moving_average.to_excel(datatoexcel, sheet_name= '3 Period Moving Average')
five_moving_average.to_excel(datatoexcel, sheet_name= '5 Period Moving Average')

datatoexcel.save()
```

To combine dataframe into a full dataset that includes the original data, and order by the variables in the original dataset, first combine all dataframes. 

```
dfs = [metrics, percent_change, absolute_change, three_compound_growth, ranking_descending,
              five_compound_growth, three_moving_average, five_moving_average]
full_df = pd.concat(dfs, join = 'outer', axis = 1)
```

Create a list to contain new column order. Then reorder the full dataframe. 

```
#check the number of existing columns 
len(full_df.columns.values)

#create a list for new column order
column_reorder = []
for i in range(0, 59):
    for j in range(i, 472, 59):
        column_reorder.append(full_df.columns[j])
        
#reoder dataframe with the column_reorder
full_df_reordered = full_df[column_reorder)
```

Use the same method to write to Excel and name the tab "Expanded". A new Excel workbook with the name "level3_data_DJIA_python_full_expanded.xlsx" will show up in the working directory. 
```
datatoexcel = pd.ExcelWriter('level3_data_DJIA_python_full_expanded.xlsx', engine = 'xlsxwriter')

full_df_reordered.to_excel(datatoexcel, sheet_name= 'Expanded')

datatoexcel.save()
```

Now we have obtained two Excel workbooks in the working directory, one is the expanded data grouped by variables in different tabs, and the other is the full expanded data in just one tab, grouped by columns of the original data. 
