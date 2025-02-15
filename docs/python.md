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

### Table Reshaping

#### Concatenate data
```py
df = pd.concat(
    [df1, df2],
    axis=0
    )
```

#### Merge data to create a complete dataframe
``` py
df = students.merge(subjects, how='cross')
```

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

## OOP
