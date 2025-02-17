
### Concatenate data
```py
df = pd.concat(
    [df1, df2],
    axis=0
    )
```

### Merge data
``` py
df = students.merge(subjects, how='cross') # (1)
df = pd.merge(employee, department, how="left", on='departmentId') # ???
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