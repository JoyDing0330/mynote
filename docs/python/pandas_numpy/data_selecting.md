### Filter by value

```py
students.loc[students['student_id'] == 101]
df.query('col1 <= 1 & 1 <= col1')
```

### Filter by conditions
```python title='query'
result = df.query('column > value and other_column < other_value')
df = df.query('date.isnull()')
```

```python title='lambda'
df = df.loc[lambda x: (x.cola != "YES") & (x.colb != "YES")]
```

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

### loc

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

### iloc

```py
df.iloc[N-1]['salary'] # ???
```