# Data Science (Python & SQL)

## Jupyer Notes
```
# start jupyter
jupyter notebook
```

## Pandas

### Load Datasets

``` python
# load datasets
import pandas as pd

df_08 = pd.read_csv('data_08.csv')
df_18 = pd.read_csv('data_18.csv')
```

### Poke Around
``` python
import pandas as pd

df = pd.read_csv('winequality-red.csv', sep=';') # use a separater or delimiter.
df.head()  # header & top 5 rows
df.head(10) # top 10
df.tail(10) # bottom 10 - default 5
df.shape   # number of rows & columns
df.dtypes  # data types of columns
df.info()  # show missing values
df.nunique() # unique values in columns
df.describe() # default stats.

### Advanced: get location ###
df.iloc[196:, 1:].sum().plot(kind='bar'); # index location

# average sales
df.mean().plot(kind='pie');

# sales for the week of March 13th, 2016
sales = df[df['week'] == '2016-03-13']
sales.iloc[0, 1:].plot(kind='bar');

# sales for the lastest 3-month periods
last_three_months = df[df['week'] >= '2017-12-01']
last_three_months.iloc[:, 1:].sum().plot(kind='pie')
```

### Wrangle & Clean Data

#### Headers

``` python
# Specify own column labels
labels = ['id', 'name', 'attendance', 'hw', 'test1', 'project1', 'test2', 'project2', 'final']
df = pd.read_csv('student_scores.csv', names=labels)
df.head()

# Specify the header line number.
labels = ['id', 'name', 'attendance', 'hw', 'test1', 'project1', 'test2', 'project2', 'final']
df = pd.read_csv('student_scores.csv', header=0, names=labels)
df.head()
```

#### Query

``` python
# selecting malignant records in cancer data
df_m = df[df['diagnosis'] == 'M']
df_m = df.query('diagnosis == "M"')

# selecting records of people making over $50K
df_a = df[df['income'] == ' >50K']
df_a = df.query('income == " >50K"')

# query Null values
df.isnull().sum()

# checks if any of columns in 2008 have null values - should print False
df.isnull().sum().any()

# value counts
df['cyl'].value_counts()
```

#### Index

``` python
# Instead of using the default index (integers incrementing by 1 from 0), you can specify one or more of your columns to be the index of your dataframe.
df = pd.read_csv('student_scores.csv', index_col='Name')
df.head()

# OR multiple index

df = pd.read_csv('student_scores.csv', index_col=['Name', 'ID'])
df.head()
```

#### Cleaning Data

``` python
# Fill in missing values (using means)
column_name = df['column_name'].mean()
df['column_name'].fillna(column_name, inplace=True)
df.info()

# check for duplicates in the data
sum(df.duplicated())

# drop duplicates
df.drop_duplicates(inplace=True)
df # see data
sum(df.duplicated()) # check to see dups were removed.

#convert data types
df['B'].astype(int)
# extract numbers and type
df['B'].str.extract('(\d+)').astype(int)
```

#### Rename Columns (pandas)

``` python
df.rename(columns = {'old column':'new_column'}, inplace = True)

# rename but preserving the first 10 characters.
df_08.rename(columns=lambda x: x[:10] + "_2008", inplace=True)
```

#### Merge Datasets

``` python
# merge datasets
df_combined = df_08.merge(df_18, left_on='model_2008', right_on='model', how='inner')
```

#### Replace Characters (Spaces with Underscores)

``` python
# replace spaces with underscores and lowercase labels for 2008 dataset
df_08.rename(columns=lambda x: x.strip().lower().replace(" ", "_"), inplace=True)

# confirm changes
df_08.head(1)
```

#### Mass Rename Columns

``` python
# remove "_mean" from column names
new_labels = []
for col in df.columns:
    if '_mean' in col:
        new_labels.append(col[:-5])  # exclude last 6 characters
    else:
        new_labels.append(col)

# new labels for columns
new_labels

# assign new labels to columns in dataframe
df.columns = new_labels

# display first few rows of dataframe to confirm changes
df.head()

# save this for later
df.to_csv('cancer_data_edited.csv', index=False)
```

#### Pandas Groupby (pivot table-ish)

``` python
df.mean() # default way
df.groupby('column').mean() # kinda like left pivot rows
df.groupby(['column1', 'column2']).mean() # multiple groupby
df.groupby(['column1', 'column2'], as_index=False)['show_only_this_column'].mean() # show only specific columns



```

## Matplotlib

### Plot charts/graphs

``` python
import pandas as pd
% matplotlib inline # Display inline (Jupyter notebook)

df.hist(figsize=(8,8)); # All histograms

 # Specific column
df['column_name'].hist();
df['column_name'].plot(kind='hist');
df['column_name'].plot(kind='box');

# Explore shapes (all scatters)
pd.plotting.scatter_matrix(df, figsize=(15,15));

# Plot relationships (x, y values)
df.plot(x='Energy Output', y='Average Temperature', kind='scatter');

# Compare 2 datasets / tables
# how many unique models used alternative sources of fuel in 2008
alt_08 = df_08.query('fuel in ["CNG", "ethanol"]').model.nunique()
alt_08

# how many unique models used alternative sources of fuel in 2018
alt_18 = df_18.query('fuel in ["Ethanol", "Electricity"]').model.nunique()
alt_18

plt.bar(["2008", "2018"], [alt_08, alt_18])
plt.title("Number of Unique Models Using Alternative Fuels")
plt.xlabel("Year")
plt.ylabel("Number of Unique Models");
```

### Communicate (make pretty)

``` python
# Filter results
df_output1 = df[df['column_name1'] == ' >50K']
df_output2 = df[df['column_name2'] == ' <=50K']

# Consistent x-labels
ind = df_output1['another_column'].value_counts().index
df_output1['another_column'].value_counts()[ind].plot(kind='bar');
df_output2['another_column'].value_counts()[ind].plot(kind='bar');

# Pie chart w/ consistent index
ind = df_output1['another_column'].value_counts().index
df_output1['another_column'].value_counts()[ind].plot(kind='pie', figsize=(8, 8));

# Labels
df_mean = df.mean()
df_mean_bar = df_mean.plot(kind='bar');
df_mean_bar.set_xlabel('Stores');
df_mean_bar.set_ylabel('Total Number of Sales');

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
% matplotlib inline # Display inline (Jupyter notebook)

colors = ['red', 'white']

# Means graph
color_means = wine_df.groupby('color')['quality'].mean()
color_means.plot(kind='bar', title='Average Wine Quality by Color', color=colors, alpha=.7)
plt.xlabel('Colors', fontsize=18)
plt.ylabel('Quality', fontsize=18)

# Counts graph
counts = wine_df.groupby(['quality', 'color']).count()
counts.plot(kind='bar', title='Counts by Wine and Color and Quality', color=colors, alpha=.7)
plt.xlabel('Quality and Color', fontsize=18)
plt.ylabel('Count', fontsize=18)

# Proportions graph
totals = wine_df.groupby('color').count()['pH']
proportions = counts / totals
proportions.plot(kind='bar', title='Proportions by Wine Color and Quality', color=colors, alpha=.7)
plt.xlabel('Quality and Color', fontsize=18)
plt.ylabel('Proportion', fontsize=18)

# More matplotlib visual controls
plt.xticks([1, 2, 3], ['a', 'b', 'c']);
plt.bar([1, 2, 3], [224, 620, 425], tick_label=['a', 'b', 'c']);
plt.bar([1, 2, 3], [224, 620, 425], tick_label=['a', 'b', 'c'])
plt.title('Some Title')
plt.xlabel('Some X Label')
plt.ylabel('Some Y Label');

# bar chart with labels
locations = [1, 2]
heights = [mean_quality_low_acid, mean_quality_high_acid]
labels = ['Low Acid', 'High Acid']
plt.bar(locations, heights, tick_label=labels)
plt.title('Average Quality Ratings by Acidity')
plt.xlabel('Acidity')
plt.ylabel('Average Quality Rating');


# Line plot chart
locations = [1, 2]
heights = [mean_quality_low_acid, mean_quality_high_acid]
labels = ['Low Acid', 'High Acid']
plt.plot(locations, heights, 'g--')
plt.title('Average Quality Ratings by Acidity')
plt.xlabel('Acidity')
plt.ylabel('Average Quality Rating');

# Good Example
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
% matplotlib inline
import seaborn as sns
sns.set_style('darkgrid')

wine_df = pd.read_csv('winequality_edited.csv')

# get counts for each rating and color
color_counts = wine_df.groupby(['color', 'quality']).count()['pH']
color_counts

# get total counts for each color
color_totals = wine_df.groupby('color').count()['pH']
color_totals

# get proportions by dividing red rating counts by total # of red samples
red_proportions = color_counts['red'] / color_totals['red']
red_proportions

# get proportions by dividing white rating counts by total # of white samples
white_proportions = color_counts['white'] / color_totals['white']
white_proportions

ind = np.arange(len(red_proportions))  # the x locations for the groups
width = 0.35       # the width of the bars

# plot bars
red_bars = plt.bar(ind, red_proportions, width, color='r', alpha=.7, label='Red Wine')
white_bars = plt.bar(ind + width, white_proportions, width, color='w', alpha=.7, label='White Wine')

# title and labels
plt.ylabel('Proportion')
plt.xlabel('Quality')
plt.title('Proportion by Wine Color and Quality')
locations = ind + width / 2  # xtick locations
labels = ['3', '4', '5', '6', '7', '8', '9']  # xtick labels
plt.xticks(locations, labels)

# legend
plt.legend()
```
![Chart](https://cdn-pro.dprcdn.net/files/acc_332476/WGZSZo)

### Appending Data

``` python
# Add columns
add_column = np.repeat('some-data', df.shape[0])
df['new_column'] = add_column
df.head()

# append dataframes - make sure that the column names lineup. Otherwise, rename them.
red_df = red_df.rename(columns = {'total_sulfur-dioxide':'total_sulfur_dioxide'})
wine_df = red_df.append(white_df)

# save data
wine_df.to_csv('winequality_edited.csv', index=False)
```

## SQL

### Select NULL

``` sql
    SELECT *
        FROM demo.accouts
        WHERE primary_poc IS NULL
    
    SELECT *
        FROM demo.accouts
        WHERE primary_poc IS NOT NULL

    /* RETURN UNMATCH ROWS*/
    WHERE Table_A.column_name IS NULL OR Table_B.column_name IS NULL
```

### JOINS

``` sql
SELECT column_name(s)
FROM Table_A
INNER JOIN Table_B ON Table_A.column_name = Table_B.column_name;

SELECT column_name(s)
FROM Table_A
LEFT JOIN Table_B ON Table_A.column_name = Table_B.column_name;

SELECT column_name(s)
FROM Table_A
RIGHT JOIN Table_B ON Table_A.column_name = Table_B.column_name;

SELECT column_name(s)
FROM Table_A
FULL OUTER JOIN Table_B ON Table_A.column_name = Table_B.column_name; /* Same as FULL JOIN */

```

* UNION vs UNION ALL (UNION drops the same matching records...UNION ALL appends all rows.)
* SQL's two strict rules for appending data:

Both tables must have the same number of columns.
Those columns must have the same data types in the same order as the first table.
A common misconception is that column names have to be the same. Column names, in fact, don't need to be the same to append two tables but you will find that they typically are.

### Difference between WHERE and HAVING

* WHERE subsets the returned data based on a logical condition.
* WHERE appears after the FROM, JOIN, and ON clauses, but before GROUP BY.
* HAVING appears after the GROUP BY clause, but before the ORDER BY clause.
* HAVING is like WHERE, but it works on logical statements involving aggregations.

### GROUPBY Example

``` sql
SELECT s.id, s.name, COUNT(*) num_accounts
    FROM accounts a
    JOIN sales_reps s
    ON s.id = a.sales_rep_id
    GROUP BY s.id, s.name
    HAVING COUNT(*) > 5
    ORDER BY num_accounts;
```

### COUNT Select queries

``` sql
SELECT COUNT(*) num_accounts
    FROM (SELECT a.id, a.name, COUNT(*) num_orders
    FROM accounts a
    JOIN orders o
    ON a.id = o.account_id
    GROUP BY a.id, a.name
    HAVING COUNT(*) > 20
    ORDER BY num_orders;) AS Table2;
```

### Subqueries w/ DATE TRUNCATION

```sql
SELECT channel,
    AVG(event_count) AS avg_event_count
FROM
(SELECT DATE_TRUNC('day', occurred_at) AS day,
    channel,
    COUNT(*) AS event_count
    FROM demo.web_events_full
    GROUP BY 1,2
) sub
    GROUP BY 1
    ORDER BY 2 DESC;

/* DATE_PART() finds month or week or day number */
```

### Data Cleaning / Manipulation

* LEFT / RIGHT
* POSITION, STRPOS, UPPER, LOWER
* CONCAT or ||
* CAST - change columns from one data type to another (shorthand is ::date)
* TRIM - remove char from beginning and end
* COALESCE (passes no non-null value)
* More... https://community.modeanalytics.com/sql/tutorial/sql-string-functions-for-cleaning/
* More more... https://www.w3schools.com/sql/sql_isnull.asp

### SQL - window functions

* OVER / PARTITION BY (always need ORDERED BY)
* RANK()
* DENSE_RANK()
* LAG
* LEAD
* NTILE

### OVER / PARTITION (Window Function)

``` sql
SELECT standard_amt_usd,
    SUM(standard_amt_usd) OVER (ORDER BY occurred_at) AS running_total
    FROM orders

SELECT standard_amt_usd,
    DATE_TRUNC('month', occurred_at) AS month,
    SUM(standard_amt_usd) OVER (PARTITION BY DATE_TRUNC('month', occurred_at) ORDER BY occurred_at) AS running_total
    FROM orders
```

### More Window function

``` sql
SELECT id,
       account_id,
       DATE_TRUNC('year',occurred_at) AS year,
       DENSE_RANK() OVER account_year_window AS dense_rank,
       total_amt_usd,
       SUM(total_amt_usd) OVER account_year_window AS sum_total_amt_usd,
       COUNT(total_amt_usd) OVER account_year_window AS count_total_amt_usd,
       AVG(total_amt_usd) OVER account_year_window AS avg_total_amt_usd,
       MIN(total_amt_usd) OVER account_year_window AS min_total_amt_usd,
       MAX(total_amt_usd) OVER account_year_window AS max_total_amt_usd
FROM orders
WINDOW account_year_window AS (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at))

SELECT occurred_at,
       total_sum,
       LEAD(total_sum) OVER (ORDER BY occurred_at) AS lead,
       LEAD(total_sum) OVER (ORDER BY occurred_at) - total_sum AS lead_difference
FROM (
SELECT occurred_at,
       SUM(total_amt_usd) AS total_sum
  FROM orders
 GROUP BY 1
 ) sub

 SELECT account_id,
		occurred_at,
        standard_qty,
        NTILE(4) OVER (ORDER BY standard_qty) AS standard_quarttile,
        NTILE(2) OVER (ORDER BY standard_qty) AS gloss_half,
        NTILE(100) OVER (ORDER BY standard_qty) AS total_percentile
FROM orders
ORDER BY account_id DESC  
```

### PERFORMANCE

* Use 'EXPLAIN' to see how much a query costs.
* Use sub-queries to pre-aggregate to make the join queries smaller.
