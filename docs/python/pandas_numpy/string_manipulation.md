
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
users['name'] = users['name'].str.capitalize()`  # (1)
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