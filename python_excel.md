In this project we obtained the data of .... Our goal is to expand the dataset by calculating new metrics for each exisiting column. The new metrics are: Percent Change, Absolute Change, Three Period Compound Growth, Five Period Compound Growth, Three Period Moving Average, Five Period Moving Average. 

For example, for the column of Close Price, we would calculate the following metrics: Close Price Percent Change, Close Price Absolute Change, Close Price Three Period Compound Growth, Close Price Five Period Compound Growth, Close Price Three Period Moving Average, Close Price Five Period Moving Average. 

The raw data contains 64 columns. Each row represent the stock transaction data recorded for a certain company at a specific date within 2014 to 2019. The Date, Symbol, Name, Sector, and Industry columns contains the basic information of each data record. 

The remaining 59 columns are stock transaction data and company cash flow data such as 'Close Price','Volume', 'Inventory', 'Long-Term Debt', 'Net Receivables','Property Plant Equipment', 'Short-Term Debt', 'Total Assets','Total Current Assets', 'Total Current Liabilities'...'4-Week Rolling Mean Minus Current Price','4-Week Standard Deviation', 'IndexDO', '3-Month IndexDO'. 

The goal of this project is to automate the dataset expansion process in Python and write to Excel through Python. 


Importing data. Read in the data in Excel worksheet as a dataframe. 

```
import pandas as pd
file = 'level3_data_DJIA.xlsx'
xl = pd.ExcelFile(file)
print(xl.sheet_names)
df = xl.parse('level3_data_DJIA.csv')
```

Sometimes, numeric data in Excel might exist as strings and floats. Let's first make sure the data types of all columns are appropriate. 

```
df.info()
df['Net Profit Margin, %'] = pd.to_numeric(df1['Net Profit Margin, %'], errors = 'coerce')
```

Create a new dataframe to temporarily store all columns of variables that need to be expanded.

```
metrics = df1.loc[:,'Close Price, $':]
```

The idea is to define a helper function that takes in a single column and return a new calculation, then create a second function that takes in a dataframe and apply the helper function to each column of the entire dataframe.

<Maybe insert a graph?>

To calculate a Percent Change, we used .shift() which allows us to shift all rows in a column to the positions of next n rows. 

<Insert a graph that explains .shift()>

```
def expand_single_column_percent_change(col):
  return (col - col.shift(1))/col.shift(1)
  
def expand_data_percent_change(df):
    return df.apply(expand_single_column_percent_change)
```

