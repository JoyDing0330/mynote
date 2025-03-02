
### Concatenate data
```py
df = pd.concat(
    [df1, df2],
    axis=0
    )
```

### Merge data
``` py
df1.merge(
  df2,
  how="right", # (1)
  on=["cola", "colb"],
  suffixes=("_all", "_roommate")
)
```

1.  - left: use only keys from left frame, similar to a SQL left outer join; preserve key order.
    - right: use only keys from right frame, similar to a SQL right outer join; preserve key order.
    - outer: use union of keys from both frames, similar to a SQL full outer join; sort keys lexicographically.
    - inner: use intersection of keys from both frames, similar to a SQL inner join; preserve the order of the left keys.
    - cross: creates the cartesian product from both frames, preserves the order of the left keys.


### Pivot: long to wide
``` py
pivoted = weather.pivot(
    index="month",
    columns="city",
    values="temperature"
    )
```
### Melt: wide to long
```py
report1 = pd.melt(
    report,
    id_vars=['product'],
    value_vars=['quarter_1', 'quarter_2', 'quarter_3', 'quarter_4'],
    var_name='quarter',
    value_name='sales'
    )
```

### Split
```py title='split a dataframe into a list by a column value'
# create a demo data
data = {
    'Category': ['A', 'B', 'A', 'B', 'C', 'A', 'C'],
    'Value': [10, 20, 30, 40, 50, 60, 70]
}
df = pd.DataFrame(data)

# Group by 'Category' and split into a list of DataFrames
grouped_df = df.groupby('Category')
list_of_dfs = [group for _, group in grouped_df]

# Display the list of DataFrames
for i, group_df in enumerate(list_of_dfs):
    print(f"Group {i+1}:\n{group_df}\n")

# check group keys
print(grouped_df.groups.keys())
# get group
grouped_df.get_group('A')
```