
# Using Python for Data Analysis

This first session of OpenRes will be focused on the exploration, cleaning and basic visualisation of a prepared set of sample data - the [pedestrian counts](https://github.com/resbaz/OpenRes/tree/master/Day%201%20Tutorials/Python) at various locations around Melbourne. If you have not already downloaded this data, please do so now, and place this data file in the same folder as this jupyter notebook. 

## Learning objectives

Thorughout this session we're going to be teaching you a range of tools and skills related to cleaning, manipulation and visualising large datasets. Using the pandas package, we can read in and manipulate large spreadsheets of data, and matplotlib lets you visualise these datasets in a useable, customisable format.

The sections I'll be taking you through today are:

- Data examination
- Dataframe manipulation
- Asking a research question
- Plotting your data
    - Seaborn (static images)
    - Bokeh (interactive images)

## Setting Up

First, we need to import packages we need to perform our data analysis.


```python
# loading the pandas and numpy libraries
import pandas as pd
import numpy as np
```


```python
#Try to run these - if unsuccessful, sit next to someone that it does work for
import matplotlib.pyplot as plt

# If this fails comment it out
import seaborn as sns

# If this fails comment it out
from palettable import colorbrewer as cb

```

We can also use a command called a *magic* with "%". These allow us to implement a variety of different commands both on the Jupyter notebook itself, and even on the system (your computer) itself.

This particualr "magic" command will allow inline plotting - i.e. all plots generated will appear within this notebook instead of in a new browser tab.


```python
# inline plotting magic command
# allows figures to be generated inside the notebook, instead of in a separate window
%matplotlib inline

#You can also choose to save these images to a local folder, which we will go through later
```

## A Basic Pandas Introduction

- creating your own dataframe
    - the "series" object
- subsetting columns
- subsetting rows
    - `head()` and `tail()`
    - slicing
    - `loc` vs `iloc`


### Understanding the Dataframe data-type

A Pandas dataframe can be thought of as a collection of lists (of equal length), where each list makes up a column inside your dateframe. 

The key difference between a list and a Pandas column though, is that every single item inside your column _must be of the same data type_. If you have a column of integers, and a single string value, like this, `[1,2,3,4,'seven']`, then every single value inside your column is going to be a string-type.

Instead of using a list - which can take any type of values -, Pandas performs this type-coercion by using a data type called a Series.


```python
df1 = pd.Series([1,2,3,4,5,6])

print(df1)
```

Each "Series" object can be thought of as it's own miniature dataframe. So our previous example would be a dataframe with one column, and 5 rows.

Therefore, when creating a dataframe, you actually have to create it as a collection of these "Series" objects


```python
df = pd.DataFrame(
        # Here we define the data that goes into the dataframe
            {'A':['a', 'b', 'c','d','e','f'], # Column "A"
             'B':[54, 67, 89, 100, None, 64], #Column "B"
             'C': np.random.randn(6), #Column "C"
             'D': [2.6,None, 8.0, 9.4, 3.3, None] #Column "D"
                },
        # And then we can specify other dataframe options
          index=[49, 48, 47, 1, 2, 3] # Lets you set your row names
            )
```


```python
df
```


```python
print("df1:")
print(df1)

df["E"] = df1

print("\n"+
        "new dataframe:")
print(df)

#What's wrong with this picture?
```


```python

```

As we can see, the two dataframes did not properly combine. 

If you take a clsoer look at the indexes though, you can see that indexes 1, 2 and 3 from *df1* combined with indexes 1, 2, and 3 from *df*.

When pandas/python tries to combine the two dataframes, it tries to do so by combining them at their indexes, instead of as a straight addition. 

This is why, when combining dataframes, we need to be *very careful* of what indexing/rownames we are using.


```python
pd.Series()
```


```python
df['E'] = pd.Series([1,2,3,4,5,6], index = df.index)

print(df)
```

## Reading in Data

The first step to any data exploration and manipulation is to open your data within your program. We are going to do this using the **pandas** package, which reads in spread-sheet style data and converts them into *dataframes*. 

These dataframes work with rows and columns, like a spreadsheet, except that all data within a single column has to be the same data type. 

For example, imagine you had a spreadsheet containing two columns - "labels" and "numbers", and that the rows in the "labels" column contains either a text or number sequence. Because you cannot turn text into a number, every single row in that "labels" column would need to be a string (text) type. Similarly, if some (but not all) of the rows in the "numbers" column contained decimals, **all** of the rows within this column would need to be of a decimal (float) data type.

To read in a comma-separated file, or \*.csv, you can use the pandas function read_csv()


```python
# Reading in a *.csv file
pdsn = pd.read_csv("pedestriancounts_melbourne.csv")
```

You can also open a variety of other file types using the "reader" functions found in this [IO tools documentation](http://pandas.pydata.org/pandas-docs/version/0.20/io.html "Pandas IO tools"). 

This includes file types such as excel (\*.xlsx) files, and text (\*.txt) files. You can use the parameters within these functions to specify file or data attributes such as column separators, whether there's column/row names, specifying your own column names. Importantly, for those working with data across different computers, or with alrge datasets, you can also specify the type of file encoding, or whether we want to save memory and open the data in "chunks". 

As a quick guide, here are some of the arguments you can use with pd.read_csv():

- `sep`: the type of separator between our columns
-  `header`: the row number to take as the start of the data. Useful if you have metadata attached at the beginning of your file
- `names`: You can also specify your column names. Takes a list of values. If your file contains no header, also use `header = none`. Otherwise, use `header = 0`.
* `parse_dates`: Treat one or more columns like dates.
* `dayfirst`: Use DD.MM.YYYY format, not month first.
* `infer_datetime_format`: Tell pandas to guess the date format.
- `na_values`: Specify values to be treated as empty.


```python
pdsn.head()
```


```python
pdsn.tail()
```

## Examining your Data

- `head()`, `tail()`
- `columns`
- `df.Column` vs `df['Column']`
- `dtypes`
- `describe()`
- `shape`; `shape[0]` vs. `shape[1]`
- `iloc` vs `loc`
- finding null data

One of the first steps in exploring your data is to see what it looks like, what data types are present, and how many rows/columns there are.


```python
pdsn.tail()
```

You can also specify how many rows you want `head` and `tail` to return


```python
# return 10 rows

```

The `shape` function gives you a tuple containing the dimensions of your data, in the form (rows, columns).

For those of you who have done a bit of math, the way we tend to index dataframes is the same as matrices - rows, then columns.

A handy pnemonic for remembering which order we need to index our data is **R**oman **C**atholic (rows, columns)

As we can see from the `shape` command, our pedestrian dataset has 656,823 rows, and 6 columns.


```python
pdsn.shape
```

As shape returns a tuple, this means we can also access the number of rows and columns just like we would with a list:


```python
# Calling the first or second element of the tuple can give you either the rows or the columns
#Gives you the rows (remember, 0 indexing!)
print("Rows:",pdsn.shape[0])

#Gives you the columns
print("Columns:",pdsn.shape[1])
```

You can also use `df.columns` to examine the column names.


```python
#what are the column names?

```

You can also examine the data types inside your columns using `dtypes`


```python
pdsn.dtypes
```


```python
# What's an "object" data type??

```

To select a particular column in your dataframe, you can use one of two options:

1) Calling the column as an "attribute"
    - `df.columnName`

2) Subsetting the dataframe
    - `df['columnName']`

The first option is useful for some functions, but the second form is essential if you want to call more than column at once. You do this by inserting a list, [], of column names, instead a single column.

For example: `df[["Column1", "Column2", ... , etc]]`


```python
# As an attribute
pdsn.SensorID.head() #you can also "chain" commands
```


```python
# As a subset
pdsn["SensorName"].head()
```


```python
# How do you think we would subset multiple columns at once?


```


```python
# What if we reversed the order?


```


```python
# What about a column that doesnt exist?
pdsn[1]
```

### Slicing

Sometimes you need to examine specific rows and columns in the middle of your data though, which aren't covered by `head()` or `tail()`. Instead, you can use index slicing.

Slicing works similarly to how you might slice a string, or a list. You simply call the indexes of the rows you want from the dataframe: `df[rowNumbers]`


```python
#Gives us rows 5 through 9

```


```python
#What about columns?
pdsn[5:10, 1:2]
```


```python

```

#### `loc` vs `iloc`

You can also use `loc` and `iloc` to slice rows and columns.

`df.iloc[]` is positional based, so takes integer values that correlate with the row and column **numbers** in your dataframe

`df.loc[]` is label based, and takes the row and column **names** as inputs

For this example we're going to use our toy dataframe, df, as an example


```python
df
```


```python
#Using iloc to get rows 0, 1 and 2

```


```python
# Using loc to get row labels 1, 2 and 3

```

If you only enter one set of values into `loc` and `iloc`, they will return the values for every column in your dataset.

By using a second integer though, you can choose which rows and columns you specifically want to subset. The first value corresponds to the row, and the second to the columns, or rows x columns. You can also think of this with the moniker *"Roman Catholic"*


```python
# Using loc to get row labels 1, 2 and 3 for Columns A and B

```


```python
# Using iloc to get rows 0, 1 and 2 for columns 0 and 1.

```

The differences between `loc` and `iloc` can seem minor, but they're very important, and which one you should use depends on what your needs are at the time.

`iloc` is based on dataframe position, so calling `iloc[:3]` would give you rows 0 through 3. 

`loc` however is based on the index label, so if you were to call `df.loc[:3]`, it would give you all rows UP TO the row labelled as index 3.

While the indexes are in order and all present, this isn't an issue. Consider what happens when the indexes are out of order though, with our dataframe 'df'


```python
# Can see that this only takes the first 2 rows
df.iloc[:2]
```


```python
# Whereas this takes all rows UP TO index label 2
df.loc[:2]
```

Since `loc` is based on labels, if you try to subset a row or column label that doesn't exist, even if it corresponds to a row number, python will throw you an error


```python
df.loc[0]
```

Due to this, it's important that you carefully consider which tool is appropriate for your needs

#### Challenge

Consider the following Python dictionary data and Python list labels:
```python
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```

1\. Create a DataFrame, `df_vet` from this dictionary data which has the index labels.


```python

```

2\. Display a summary of the basic information about this DataFrame and its data. Make sure it includes all of the columns.


```python
#Hint - using ?method will display the documentation for that method
?df.describe()
```


```python

```

3\. Return the first 3 rows of the DataFrame df


```python

```

4\. Select the data in rows [3, 4, 8] and in columns ['animal', 'age'].


```python

```

#### The "describe" method

As we saw during our challenge, one method which is handy for a quick summary fo your data is `df.describe()`.

By default, `describe` will only output the summary statistics for the numeric data in your dataframe, unless only text data is present. To include - or exclude - certain types of data from your summary you need to use the `include = "all"` or `exclude = <data type>` arguments. Remember to use the pandas column datatypes (int64, float64 and object) though.

We can also find the specific statistics seen in this summary table by using `max()`, `min()`, `count()`, `std()` {standard deviation}, `mean()` and `sum()`. 

Just remember though that many of these functions rely on numeric data types, and will cause errors if used on a str type. Calling `sum()` on a string however will concatentate those strings.


```python
# See the difference between the different data types?
df_vet.sum()
```


```python
#numeric
df_vet.age.max()
```


```python
#String
df_vet.animal.max() #this gives the 'highest' lexicographical string inside text columns
```


```python
print("Number of values in this column:",df_vet.visits.count())
print("Mean number of visits:",df_vet.visits.mean())
```


```python
# Unique will tell you how many unique values exist in your column
df_vet["animal"].unique()
```

### Subsetting with Conditionals

You can also subset using conditional statements, such as ==, !=, >, <, etc.

For example, if I want to find all of the rows where Hour > 20, I would type:


```python

```

Just as with lists and for loops, etc, you can also combine these conditionals using & {and} , or | {or}

**Important**: While Python accepts either `&`/`and` or `|`/`or`, pandas doesn't tend to like using `and/or`. When combining conditional statements try to use the amphersand (&) or the pipe (|)


```python
pdsn.head()
```


```python
#Find the rows where the SensorName is "Town Hall (West)" AND it's a Sunday

```


```python
#You can also get a list of the row indexes for your subset

```

Similarly, you can subset using boolean lists


```python
pdsn.count()
```


```python
# a boolean list. The 1st, 3rd and 5th values are True.
na = pdsn["PedestrianCount"].isnull()

#A Boolean list - contains only True/Falses
print(na[:10])

#The same length as the dataframe - 656,823 rows
print(len(na))

```


```python
#subsets all rows where PedestrianCounts = None
pdsn[na]
```


```python
# Could also subset directly - but it's less readable
pdsn[pdsn["PedestrianCount"].isnull()]
```

We can also use the `.sum()` and the `.any()` commands to find the number of nulls in our dataframes


```python
# Sums the number of NaNs in each column
pdsn.isnull().sum()
```


```python
# Sums the number of NaNs in total dataframe
pdsn.isnull().sum().sum()
```


```python
?df.any()
# axis means columns or rows. rows = 0, columns = 1
```


```python
df.isnull()
```

#### Challenge

Find how many rows belong to the sensor at Lygon St (West) between the hours of 12am and 2am?

*Hint 1: an entry of "3" in the Hour column means that the pedestrian counts have been monitored from 3am to 4am*

*Hint 2: remember that you find the length of a list using the "len()" function*


```python

```

#### Extension Challenge

Find the columns inside the dataframe `df` that contain na values, then return the indexes for all of the rows that contain NaN values

**Hint:** Remember the .any() command


```python

```

## Cleaning and Manipulating Data

- Dealing with duplicates
- Sorting data
- using the `apply()` function
    - `reset_index` for row names
- Adding and deleting columns/rows
    - renaming columns
- Data type conversion
    - Using the "timestamp" data type
- `groupby()`
- setting data frequency (maybe)

### Deleting duplicate values

We don't actually have any duplicates in our pedestrian dataset, so we're going to go back and create a new dataframe that does


```python
df["E"] = pd.Series([1,2,4,2,3,3], index = df.index)
print(df)
```


```python
?df.drop_duplicates
```


```python
df_noDups = df.drop_duplicates('E')
```


```python
df_noDups
```

### Resolving Null Data

There are 2 ways to resolve Null data - replace it with new data, or delete them.

For the purposes of this event, we are going to teach you how to delete them (though another section below this will teach you to replace it, for the purposes of your own data investigations).

Firstly, let's view the columns where SensorID, Location and Counts have null values.

We can do this using the `isnull()` function. As mentioned before, we can subset with a boolean list, which is what `isnull()` returns. Where the test evaluates to 'True', the row is output from the dataframe.


```python
# Preserving a copy of our data for later
pdsn_no_nulls = pdsn.copy()
```


```python
#Show the rows where SensorID has null values
pdsn[pdsn['SensorID'].isnull()]
```

Luckily, we have a function for doing this, `dropna()`!


```python
?df.dropna()
```

`dropna()` is a really powerful tool in your data cleaning arsenal - it lets you decide how to delete the null data, whether it should only remove it from rows or columns above a certain threshold, or whether to only delete from certain columns


```python
#What do you think would happen here
pdsn_no_dups = pdsn.dropna(axis = 0, how='all')
```


```python

```


```python

```

### Challenge

Create a new dataframe, df_no_dups, from the df dataframe, which contains no null values in any of the columns.


```python
df
```


```python

```


```python

```

### Sorting values


```python
# Sorting is ascending by default, or chronological order
sorted_df = df_noDups.sort_values("E")

sorted_df
```


```python
#Can also sort according to index number
sorted_df = df_noDups.sort_index()

sorted_df

```

You can also change the indexes in multiple ways - you can either reset your index to count from 0 (useful if you've deleted values), or you can set another columns values as your index.

Beware! If using column data, these values MUST be unique. This means it's ideally suited for things like customer IDs, timeseries data, or other unique identifiers.


```python
indexed_df = sorted_df.reset_index()

indexed_df
```


```python
# Use `Datetime` as our DataFrame index
indexed_df = sorted_df.set_index(sorted_df.A)

indexed_df
```

#### Challenge

Sort df_vet first by the values in the 'age' in decending order, then by the value in the 'visit' column in ascending order.


```python

```


```python

```


```python

```

### Replacing Nulls

###### Where one column is dependant on another
In the case of SensorID and Location, each of these values are dependent on the other. Where the SensorID is 13, the Location will always be 'Flagstaff Station', etc. In these cases, you can use previous values from your table - where you DON'T have nulls, to replace your missing data.

Where there are only a few points, you might choose to do this manually. This can be done using the `set_value(index,"Column", value)` function. For the index parameter, you can pass it either a single index, or a list, [], of indexes to be replaced. 

For example, if you noticed that there's a null value for SensorID at index 312.


```python
pdsn[pdsn.SensorID.isnull()]
```

You know that this SensorName, Flagstaff Station, corresponds to SensorID 9.0. If you wanted to replace this, you would type:


```python
print('Before:')
print(pdsn.SensorID.iloc[312])
print(pdsn_no_nulls.SensorID.iloc[312])


pdsn_no_nulls.set_value(312, "SensorID", 9.0)

print("After:")
print(pdsn.SensorID.iloc[312])
print(pdsn_no_nulls.SensorID.iloc[312])
```

Say that you noticed a repeated spelling error in some of the labels, and wanted to fix them, or change something for more clarity. For example, if you wanted to replace all instance of "Flagstaff Station" in the Location column with "Flagstaff", you might use:


```python
pdsn_no_nulls = pdsn_no_nulls["SensorName"].replace("Flagstaff Station", "Flagstaff")
```


```python
pdsn_no_nulls[pdsn_no_nulls.SensorName == "Flagstaff"].head()
```

That only works within a single column though. If you wanted to replace all instances where the SensorID was blank, but the SensorName was not, you could pass a list of indexes to set_value instead


```python
#Do not run this, this is only an example
pdsn_no_nulls = pdsn_no_nulls.set_value(null_index_list, "Location", "Flagstaff Station")
```

###### Replacing all null values with a single value
In the PedestrianCounts column for example, you can see that there are 14 missing values. You might choose to just replace all of these with 0. You can do this using the `fillna()` function, and specifying the value you want to replace it with.


```python
pdsn_no_nulls['PedestrianCount'] = pdsn_no_nulls['PedestrianCount'].fillna(value=0)
```


```python
pdsn_no_nulls.isnull().any(0)
```

### Converting Data Types, Adding and Deleting Columns

While we currently have a "Timestamp" column, if you go into dtypes you can see that pandas has read this in as string, rather than as a datetime type. If we left it as a string, to get the Month, Day or Year we would need to perform a series of string manipulations every time we wanted to access these values - a costly, redundant and time consuming exercise if trying to use the whole dataframe. 

Instead, we can convert this column to a datetime data type, using the `to_datetime()` function.


```python
pdsn.columns
```


```python
pdsn['Date'] = pd.to_datetime(pdsn['Date'])
```


```python
pdsn.dtypes
```


```python
# Adding a Column with the Day
pdsn['Day'] = pd.DatetimeIndex(pdsn['Date']).day

# Adding a Column with the Month (from 1-12)
pdsn["Month"] = pd.DatetimeIndex(pdsn["Date"]).month

# Adding a Column with the Year
pdsn["yer"] = pd.DatetimeIndex(pdsn["Date"]).year
```


```python
# Add another Year column

```

Oops, I've accidentally added two columns with the year in them, and one is named incorrectly.

To delete the misnamed column is actually quite simple


```python

```

You can also convert other columns by using the df[column].astype(<datatype>) function. This includes 'str', 'int64' and 'float64' data types, amongst others.

You must be careful **not to use `astype()` on a column with null values**. `astype()` does not preserve the null values, and makes resolving them later more difficult.

Let's use our dataframe, df, in an example of converting types


```python
df.D = df.D.astype(str)
```


```python

```


```python

```


```python

```

#### Challenge

Delete all of the null values currently contained within the pdsn dataset. Then, re-order the indexes.

How many rows were deleted from the dataset?


```python

```


```python

```


```python

```

### Grouping Data

It's often useful to be able to group the data within a column according to the data in another column. 

For example, you might wish to take the mean pedestrian counts for each month, or each year. We could do this is a complicated and time consuming way where you subset the data for each month, and then take the mean of the Counts column...or we could just use the `groupby` function. `groupby` outputs a reformatted version of your data where all values associated with your "grouping factor" are taken together. You can then perform mathematical and statistical tests on this grouped output, and the tests will be performed within each of the "groups" you defined.

The documentation for this is quite good, so I would recommend [reading it](http://pandas.pydata.org/pandas-docs/stable/groupby.html) if you'd like to learn more than what we learn here. There is also another tutorial [here](https://chrisalbon.com/python/pandas_apply_operations_to_groups.html) that covers how to apply functions to your groups in more depth.

Say that we wanted to find the mean pedestrian counts seen in each month:


```python
# Group by Month

```

In this case, pdsn_no_nulls.groupby('Month').aggregate(mean) means "Group the rows by month and then take the mean of all of the values with the same month.


```python
month_means = pdsn_no_nulls.groupby('Month').aggregate(mean)
month_means
```


```python
# You can see a summary of your grouped data
pdsn_no_nulls[["Day","Month","PedestrianCount"]].groupby('Month').describe()
```


```python
month_means.index = ["January", "February","March","April","May","June",\
                     "July","August","September","October","November","December"]
del month_means["index"]

month_means
```


```python
month_means.describe()
```


```python
month_means["PedestrianCount"].plot(kind='bar')
```


```python
# Can also specificy multiple columns to be output
    # the mean "Hour" and pedestrian counts within each month
pdsn.groupby(by=["Month"]).mean()[["PedestrianCount","Hour"]]
```

You can also choose to group over multiple factors, such as by Month and Year together, by specifying a list for the `by` parameter within `groupby()`. The order of the values in this list matters, as `groupby` will first group your data based on the first factor, and THEN the second factor.


```python
# Grouping by Year, then by month
#The average number of pedestrians 
pdsn.groupby(by=["Year","Month"]).mean()
```


```python
# What if we reverse it?

```


```python

```

By chaining more commands together you can also find the maximum, minimum, etc, values for your groups


```python
# Minimum
pdsn.groupby(by=["Month"]).PedestrianCount.sum().min()
```

But which month is this associated with?


```python
# Finding the Month associated with the minimum value
countMin = pdsn.groupby(by=["Month"]).PedestrianCount.sum().min()

pdsn.groupby(by=["Month"]).PedestrianCount.sum()[pdsn.groupby(by=["Month"]).PedestrianCount.sum() == countMin]
```

You can also iterate over your groups using a `for` loop, just as if you were using a dictionary:


```python

```

####  Challenge
Which sensor has the greatest average Pedestrian traffic? 


```python

```

#### Optional Challenge

Count the number of each type of animal in df_vet.


```python

```


```python

```


```python

```

## Plotting Your Data

This section will teach you the basics of plotting within Seaborn. Seaborn builds upon matplotlib, and so you'll be learning how to use both Seaborn and matplotlib functions at the same time.

This is hardly comprehensive, and there's a wide array of customisability and control with matplotlib and plotly. Feel free to delve into some of user guides and tutorials and questions available on Google and Stack Overflow to learn more about these tools and how to make them work for you.

*Seaborn tutorials*:  
- https://seaborn.pydata.org/
- https://seaborn.pydata.org/examples/index.html#example-gallery
- https://elitedatascience.com/python-seaborn-tutorial

*Matpotlib*:  
- https://matplotlib.org/users/pyplot_tutorial.html
- The [matplotlib API](https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.html) contains all of the functions you can call within matplotlib, along with detailed information about how you can use them and their parameters
- A handy "quick guide" to the different kinds of plotting functions within pyplot can also be found at the [pyplot API](https://matplotlib.org/api/pyplot_api.html)

Let's begin by trying to plot the data


```python
pdsn.groupby(by = "Month").plot()
```

But that's not particularly informative, and much of the graph is completely unreadable.

Matplotlib.pyplot has great customisability, so let's play around with trying to plot different types of graphs with our data

### Line Plot

Line plots are great for plotting series data, usually a time series of some description.

Line plots are the default plot drawn when calling `.plot()`.

Within the plot function you can also specify the Figure title and the figure size


```python
# Line plot
pdsn.groupby(by = ['Year', 'Month']).mean().Counts.plot( # Drawing a pot from the grouped Year, Month data
                                                figsize = (8,6), # The figure size
                                                title = 'Mean Pedestrian Counts per Month, per Year') #Figure Title

#specifying the x-axis label
plt.xlabel('Pedestrian Counts per Year, Month')

# the y-axis label
plt.ylabel('Counts (mean)')

#Rotating the x-axis labels
plt.xticks(rotation=90)

#If you want to SAVE your plot, make sure you do it BEFORE you show it in the notebook
plt.savefig('PedestrianHistogram.png', dpi=300)

#Generating an image
plt.show()
```


```python

```


```python

```


```python

```

### Simple scatter plots
Seaborn doesn't have a dedicated scatter plot function. We actually used Seaborn's function for fitting and plotting a regression line, so we see a regression trend line.

Unfortunately our pedestrian data doesn't have many continuous variables, so our scatter plots will look rather sparse.


```python
pdsn.columns
```


```python
# lmplot = Linear model plot
sns.lmplot(x='Month', y='PedestrianCount', data=pdsn,
         fit_reg=False, # No regression line
         hue='Day_of_week',
           
          )


# # Tweak using Matplotlib
plt.ylim(0, None)
plt.xlim(0, None)
```

### Boxplot

The humble boxplot is often extremely useful for conveying large amounts of information all at once


```python
# Set theme
sns.set_style('whitegrid')

#Change the size
plt.figure(figsize=(6,15))

# sns.boxplot(data = df)
sns.boxplot(data=pdsn[['PedestrianCount']]) #Can also add more boxes
```

### Violin Plot

These are like boxplots, but allows the audience to visualise exactly where the most data is occuring within your plots. Unlike boxplots, these are also much easier to plot your categorical and continuous variables against one another


```python
plt.figure(figsize=(15,6))

# Violin plot
sns.violinplot(x='Month', y='PedestrianCount', data=pdsn)
```

### Histograms

We also cannot forget the humble histogram - these are perfect for trying to count the frequency or density of certain occurences

Be aware though! This is one of those functions that **will not** allow NaN values while plotting


```python
#Distribution Plot (Histogram)

sns.distplot(pdsn_no_nulls.PedestrianCount)
```

### Bar Plots




```python
# Count Plot (Bar Plot)
# when you want to show the number of observations in each category

# plt.figure(figsize=(15,6))

sns.countplot(x=pdsn[pdsn.PedestrianCount > 2000].Day_of_week, data=pdsn)

plt.title("Pedestrians Per Day")
plt.xlabel("Day")
plt.ylabel("Total Pedestrians")

# Rotate x-labels
plt.xticks(rotation=+45)

plt.show()
```


```python
#bar plots will automatically calculate a statistic for you
plt.figure(figsize=(15,6))

sns.barplot(x='Day_of_week', y="PedestrianCount", data=pdsn)

plt.xticks(rotation=-45)
```

### Heatmaps

Plotting correlation calculations


```python
# Calculate correlations
corr = pdsn.corr()
 
# Heatmap
sns.heatmap(corr, 
            annot = True,
            linewidths=.5,
            cmap="YlGnBu"
           )
```

### Density Plot


```python
# Density Plot - this one requires two continuous variables
sns.kdeplot(x = , y = )
```


```python

```

### Pairplots

Allows you to plot many different types of plots and correlations between your different variables in a single image


```python
#This one uses density plots within it, so also requires continuous data
sns.pairplot(pdsn, hue="Day_of_week", size=2.5)
```


```python
# We can make pairplots but change what we want to see in the upper, lower and diagonal quadrants

g = sns.PairGrid(pdsn)
g.map_upper(plt.scatter)
g.map_lower(sns.kdeplot, cmap="Blues_d")
g.map_diag(sns.kdeplot, lw=3, legend=False);
```


```python

```

This is only a taster of what's available, and the level of customisability you're able to use for your plotting tools. 

Along with those tutorials seen previously, you can also find a range of different plots - along with the accompanying code - at this [python graph gallery](https://python-graph-gallery.com). 

In addition, pandas itself contains a whole range of plotting tools that are designed to work specifically with dataframes. You can see exampes of these and many more inside the pandas documentation, https://pandas.pydata.org/pandas-docs/stable/visualization.html

I've also included a link here to some bokeh tutorials. Bokeh is another python plotting package, except that this one allows you to create interactive iamges which are easily embedded into web applications.

http://nbviewer.jupyter.org/github/bokeh/bokeh-notebooks/blob/master/index.ipynb#Tutorial


```python

```

## Summary

This session has taken you through the beginner's guide of how to visualise, clean and investigate your data, and given you the basic tools to answer your own research questions about your data. 

Python is a very versatile tool, and can go much further than what we've shown you today. Many packages are open source and being developed for a range of purposes all the time. Scipy allows you to perform basic math and statistical tests for example, there are a host of packages related to investigating biological data.

Using a programming language allows you to investigate much larger data than you can do by hand or by eye, and the customisability of the plotting tools makes it ideal for generating useful and professional images  ideal for publication.

Follow us on Twitter to keep up-to-date on new and up-coming trainings
- [@ResBaz](https://twitter.com/resbaz)
- [@ResPlat](https://twitter.com/resplat)

There's also a reasonably new facebook group, called "Data Wrangling with Python" where you can post your python problems, cool things you've done, and stay apprised of new trainings coming up as well.

Also remember that if you're having problems, Research Platforms runs a weekly Hacky Hour, where anybody and everybody is able to enquire about coding and programming problems in a variety of tools. Every Thursday from 3-4pm, at the large table in Tsubu Bar.


```python

```
