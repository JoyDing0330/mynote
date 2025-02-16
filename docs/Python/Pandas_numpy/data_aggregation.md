### Aggregate and count occurance

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

### Aggregate and get max (include ties)

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

### Aggregate and get max (ignore ties)

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

### Aggregate and concatenate

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