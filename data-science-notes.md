# Data Science (Python & SQL)

## Pandas

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

### Advanced ###
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
```

#### Rename Columns (pandas)

``` python
df.rename(columns = {'old column':'new_column'}, inplace = True)
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