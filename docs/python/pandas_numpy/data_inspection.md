### Check basic information
prints information about a DataFrame including the index dtype and columns, non-null values and memory usage.
``` python
df.info()
```

### Describe the data
Descriptive statistics include those that summarize the central tendency, dispersion and shape of a dataset’s distribution, excluding NaN values.

``` py
df.describe()
```

### check the dimensions of dataframe
``` python
df.shape
```

### CHeck the column name of dataframe

```python
df.column
```

### Show data heads and tails

``` python
df.head()
df.tail()
```
### print formated table
```python
print(
    tabulate(
        df,
        headers="keys",
        tablefmt="psql",
    )
)
```
