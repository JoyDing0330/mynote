## Data Analysis

### Read, create and write data

#### Create a dataframe from list

``` py
list1 = ['A', 'B', 'C', 'D', 'E']
list2 = [1, 2, 3, 4, 5]

df = pd.DataFrame(list(zip(list1, list2)), columns =['Name', 'value'])
```

#### Create a dataframe from dict

``` py
d = {f'getNthHighestSalary({N})': [111, 222, 333]}
df = pd.DataFrame(data=d)
```

### Data Inspection
#### check the dimensions of dataframe
``` py
df.shape
```

#### Show data heads and tails

``` py
df.head()
df.tail()
```

### Data Selecting

#### Filter by value

```py
students.loc[students['student_id'] == 101]
df.query('col1 <= 1 & 1 <= col1')
```

#### Filter by conditions

```py title='Filter by multiple conditions'
 df = df.loc[(df['area'] >= 3000000)|(df['population'] >= 25000000)]
```

```py title='Filter by value in another list'
df = df[df['A'].isin([3, 6])]
```

```py title='filter by value not in another list'

# Tilde operator
df = df[~df.group.isin(["A","B","D"])]

# False condition
df = df[df.group.isin(["A","B","D"])==False]
```

#### Filter the max value

``` py
df=df.loc[df['count'] == df['count'].max()]
```

#### `loc`

``` py title='return the row as a Series'
df.loc['viper']
```

``` py title='select rows or columns as a DataFrame'
df.loc[['row1', 'row2']]
df.loc[['col1', 'col2']]
```

``` py title='select by row and column'
df.loc['row', 'col']
df.loc[:, 'C':'E']
```

#### `iloc`

```py
df.iloc[N-1]['salary'] # ???
```

### Data Cleaning

#### Change data type

```python title='from char to int'
df['grade'] = df['grade'].astype(int)
```

#### Drop duplicate rows

```py
df.drop_duplicates(subset=['A','B','C'], inplace=True, keep='first')
```

#### Drop missing data

```py
df=df.dropna(
    axis=0,   # (1)
    how='any' # (2)
    subset=['A','B'],
    inplace=True # (3)
    )
```

1.  axis{0 or ‘index’, 1 or ‘columns’}, default 0
2.  how{‘any’, ‘all’}, default ‘any’
    - ‘any’ : If any NA values are present, drop that row or column.
    - ‘all’ : If all values are NA, drop that row or column.
3.  Whether to modify the DataFrame rather than creating a new one.

#### Fill missing data

```py
df[col]=df[col].fillna(value)
```

#### Arrange columns

``` py
df.sort_values(
    by=['a', 'b'],
    ascending=[True, False],
    inplace=True,
    na_position='last' # (1)
    )
```

1.  {‘first’, ‘last’}, default ‘last’
    Puts NaNs at the beginning if first; last puts NaNs at the end.


#### Rename columns

```py
df.rename(
    columns={'old_name1':'new_name1', 'old_name2':'new_name2'},
    inplace=True
    )
```

#### Modify columns

```python title='compute rank'
df['rank'] = df['score'].rank(
    method='dense', # (1)
    ascending=False,
    na_option='keep' # (2)
    ).astype(int)
```

1.  {‘average’, ‘min’, ‘max’, ‘first’, ‘dense’}, default ‘average’
How to rank the group of records that have the same value (i.e. ties):
    - average: average rank of the group
    - min: lowest rank in the group
    - max: highest rank in the group
    - first: ranks assigned in order they appear in the array
    - dense: like ‘min’, but rank always increases by 1 between - groups (assign different rank for ties).
2.  {‘keep’, ‘top’, ‘bottom’}, default ‘keep’
    - keep: assign NaN rank to NaN values
    - top: assign lowest rank to NaN values
    - bottom: assign highest rank to NaN values

```py title='create new column based on condition'
df['level'] = np.where((df['value1'] >= 4) & (df['value2'] >= 10), 'High', 'Low')
```

```py title='create new column based on more than one conditions'
# Method 1
df['cat'] = pd.cut(df['val'], [-1,2,5,10], labels=['low', 'medium', 'high'])

# Method 2
conditions = [
    df['income'] < 20000,
    df['income'].between(20000, 50000, inclusive='both'),
    df['income'] > 50000
]
categories = ["Low Salary", "Average Salary", "High Salary"]
df['category'] = np.select(conditions, categories, default="Unknown")
```
#### Add new columns
```py
employees['bonus'] = employees['salary']*2
```

```py
df['bonus'] = df.apply(
    lambda row: row['salary'] if row['employee_id'] % 2 == 1 and not row['employee_name'].startswith('M')
        else 0,
    axis=1
    )

```

#### Reset index

```py
df['id']=df.reset_index(drop=True).index+1
```

### Aggregate and statistical analysis

#### Aggregate and count occurance

```py
# method 1
df.groupby('Company Name', as_index=False).agg(MySum=('Amount', 'sum'), MyCount=('Amount', 'count'))

# method 2
df.groupby(['name'], as_index=False).agg({'value': 'sum', 'otherstuff': 'first'})
```

#### Aggregate and count distinct

```py
df = daily_sales.groupby(['date_id','make_name'], as_index=False).agg(
    unique_leads=('lead_id', lambda x: x.nunique()),
    unique_partners=('partner_id', lambda x: x.nunique())
    )
```

#### Aggregate and get max (include ties)

```py
# Create dummy date
df = pd.DataFrame({
    'col1': ['A', 'A', 'A', 'A', 'A', 'B', 'B', 'B', 'B', 'B', 'B', 'C', 'C', 'C', 'C', 'C'],
    'col2': ['AX', 'AX', 'AY', 'AY', 'AY', 'BX', 'BX', 'BX', 'BY', 'BY', 'BY', 'CX', 'CX', 'CX', 'CX', 'CX'],
})

# Get Max Value by Group with Ties
df_count = df.groupby('col1', as_index=0)['col2'].value_counts()
max_index = df_count.groupby(['col1'])['count'].transform(max) == df_count['count']
df1 = df_count[max_index]
```

#### Aggregate and get max (ignore ties)

```py

# method 1
df=df.loc[df.groupby(["departmentId", "Department"])["Salary"].idxmax()]

# method 2
df1 = (df
 .groupby('col1')['col2']
 .value_counts()
 .groupby(level=0)
 .head(1))
```

#### Aggregate and concatenate

```py
# method 1: using list and rename at the same time
df = activities.groupby(['sell_date'], as_index=False).agg(
    num_sold=('product', 'count'),
    products=('product', lambda x: ','.join(x))
    )

# method 2: using dict
df = activities.groupby(['sell_date'], as_index=False).agg(
    {'product': ['count', lambda x: ','.join(x)]}
    )
```

### Table Reshaping

#### Concatenate data
```py
df = pd.concat(
    [df1, df2],
    axis=0
    )
```

#### Merge data
``` py
df = students.merge(subjects, how='cross') # (1)
df = pd.merge(employee, department, how="left", on='departmentId') # ???
```

1.  - left: use only keys from left frame, similar to a SQL left outer join; preserve key order.
    - right: use only keys from right frame, similar to a SQL right outer join; preserve key order.
    - outer: use union of keys from both frames, similar to a SQL full outer join; sort keys lexicographically.
    - inner: use intersection of keys from both frames, similar to a SQL inner join; preserve the order of the left keys.
    - cross: creates the cartesian product from both frames, preserves the order of the left keys.


#### Pivot: long to wide
``` py
pivoted = weather.pivot(
    index="month",
    columns="city",
    values="temperature"
    )
```
#### Melt: wide to long
```py
report1 = pd.melt(
    report,
    id_vars=['product'],
    value_vars=['quarter_1', 'quarter_2', 'quarter_3', 'quarter_4'],
    var_name='quarter',
    value_name='sales'
    )
```

### String manipulation

```py title='str.len'
df=tweets[tweets["content"].str.len()>15][["tweet_id"]]
```

```python title='str.contains'
df = patients[patients['conditions'].str.contains(r'(^DIAB1|\sDIAB1)', na=False)] # (1)
```

1.  `\s` matches whitespace (spaces, tabs and new lines)

``` py title='str.match'
pattern = r'^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\.com$'

# Filter valid emails
df = users[users['mail'].str.match(pattern, na=False)]
```

```py title='str.capitalize'
    users['name']=users['name'].str.capitalize()`  # (1)
```

1.  - `Series.str.lower`: Converts all characters to lowercase.
    - `Series.str.upper`: Converts all characters to uppercase.
    - `Series.str.title`: Converts first character of each word to uppercase and remaining to lowercase.
    - `Series.str.capitalize`: Converts first character to uppercase and remaining to lowercase.
    - `Series.str.swapcase`: Converts uppercase to lowercase and lowercase to uppercase.
    - `Series.str.casefold`: Removes all case distinctions in the string.

```py title='start with'
df['employee_name'].startswith('M')
```

## Formatted String Literals


## OOP
