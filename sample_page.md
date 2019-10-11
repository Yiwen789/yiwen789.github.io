
**Project description:** 

This project is a DataSquad teaching and research support project for Carleton professor Annette Nierobisz to use in her Fall 2019 class SOAN314 Contemporary Issues in Critical Criminology. This project we would like to explore the data-cleaning options in Tableau. This project cleans up the raw datasets of 1996-2005 and 2008-2017 Uniform Crime Reporting and creates visualizations using Tableau. 
<br/><br/>
Uniform Crime Reporting(UCR) compiles official data on crime in the United States, published by the Federal Bureau of Investigation (FBI). (Wikipedia)
<br/><br/>

### 1. Data Preparation
The raw data, using the 1996-2005 UCR report as an example, is in the following Excel format. The data is currently not in an ideal form because: 1. Not all rows in the Excel sheet are data inputs. 2.The dataset is in a wide-format, whereas all measurements of one type of Offense Charged is in one row; however, we want to only see one measurement of an Offense Charged in one row in long-form. 
<br/><br/>
<img src="images/Excel Raw Data.png?raw=true"/>
<br/><br/>

#### A. Tableau Data Interpreter 
We can fix the first issues above with the Tableau **Data Interpreter.**
<br/><br/>
Let's see connect the raw data to Tableau and see how Tableau would interpret it. After the data is connected, on the left bar under **Sheets** check **Cleaned with Data Interpreter.**
<br/><br/>
<insert data interpreter gif>
<img src="images/data interpreter.gif?raw=true"/>
<br/><br/>
Under the box of **Cleaned with Data Interpreter.**, click the blue underlined text **Review the results** and Tableau will prompt you to an Excel workbook that indicates how the data is interpreted in Tableau. When the Excel sheet pops up, go to the second tab of the workbook to review results. 
<br/><br/>
As we can see from the information in the second tab, Tableau is smart enough to ignore the headnotes and the footnotes. It has also identified the headers and the data rows correctly. 
<br/><br/>
<insert Tableau generated Excel screenshot>
<img src="images/Tableau Generated Excel.png?raw=true"/>
<br/><br/>
Since we don't want the total value (Row 8) to be included in the data, we will exclude this value. 
<br/><br/>
We also noticed from the footnotes that "Violent crimes are offenses of murder, forcible rape, robbery, and aggravated assault." and "Property crimes are offenses of burglary, larceny-theft, motor vehicle theft, and arson." This shows us that 'Violent Crime'(Row 17) and 'Property Crime'(Row 18) are categories of Offense Charged, rather than an actual offense. Let's change it by creating a separate column 'Crime Type' in the raw data. 
<br/><br/>
<insert clean Excel sheet data>
<img src="images/Cleaned Excel Sheet.png?raw=true"/>
<br/><br/>
Review data in the tableau **Data Interpreter** again until there is no mistake and move on to the next step. 
<br/><br/>
 
#### B. Data Pivoting: Changing Wide-format Data to Long-format
Before we get started, let's first delete every column of percentage change because these columns are derivable from the rest of the data. To do this, double click on the column head and choose **Hide**. 
<br/><br/>
Select only the columns containing numerical data and and the top right drop down menu, select **Pivot**.
<br/><br/>
<insert pivoting gif>
<img src="images/Pitvoting.gif?raw=true"/>
<br/><br/>

Notice the **Pivot Field Names** column contains information of 'Gender', 'Age Group', and 'Year'. We can break this down and create separate columns by double-clicking on this column, and in the drop-down menu go to **Custom Split**.
<br/><br/>
<insert custom split>
<img src="images/Custom Split.png?raw=true"/>
<br/><br/>

For example, we can use space as a separator and split off the first column, thus creating a 'Gender' column. 
<br/><br/>
<insert new column>
<img src="images/Split New Column.png?raw=true"/>
<br/><br/>
Rename columns as necessary. The final version of the dataset looks like the following. 
<br/><br/>
<insert full columns graph> 
<img src="images/Full Columns.png?raw=true"/>
<br/><br/>
 
### 2. Data Visualization
We created the following visuals as some examples to test the cleaned up data. We made Offense Charged a sub-filter as 'Crime Type'. The first visual is a side-by-side year comparison barchart, color-coded by gender. 
<br/><br/>
<insert first visual>
<img src="images/example viz 1.png?raw=true"/>
<br/><br/>
 
The second visual is a side-by-side Offense Charged Comparison, color-coded by Offense Charged. 
<br/><br/>
<insert first visual>
<img src="images/example viz 2.png?raw=true"/>
<br/><br/>

