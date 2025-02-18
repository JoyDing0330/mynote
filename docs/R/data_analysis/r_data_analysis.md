## Deal with NA
### replace NA to 0
```r
df %>%
    map(
        .,
        ~ mutate(across(everything(), ~ ifelse(is.na(.x), 0, .x)))
    )

df %>%
    map(
        ~ mutate(across(everything(), ~ replace_na(.x, 0)))
    )

df %>%
    mutate_at(
      vars(count),
      ~ replace_na(.x, 0)
    )
```

### replace all blanks with na
```r
apply(df, function(x) gsub("^$", NA, trimws(x)))
```
### replace 'unknnown' with na
```r
starwars %>%
   mutate(across(where(is.character), ~na_if(., "unknown")))
```

### Filter the number of NA on certain columns
```r
data %>% 
  filter(
    str_detect(
      col_name, '^(ha|test_outcome|result_dt_tm)$', negate = T
    ),
    n_missing != 0
  )
```

## Select columns with regular expression
```r
dplyr::select(matches('date|dt_tm'))
```