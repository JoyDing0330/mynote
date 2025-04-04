## Deal with NA
### replace NA with 0
```r
df %>%
    purrr::map(
        .,
        ~ dplyr::mutate(across(everything(), ~ ifelse(is.na(.x), 0, .x)))
    )

df %>%
    purrr::map(
        ~ dplyr::mutate(across(everything(), ~ tidyr::replace_na(.x, 0)))
    )

df %>%
  dplyr::mutate_at(
    dplyr::vars(count),
      ~ tidyr::replace_na(.x, 0)
    )

df %>%
  dplyr::mutate_if(
    is.numeric,
    funs(tidyr::replace_na(.,0))
    )
```

### replace all blanks with na
```r
df %>% apply(2, function(x) gsub("^$", NA, trimws(x)))
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

### Fill in missing values with previous or next value
The values are inconsistently missing by position within the group. Use .direction = "downup" to fill missing values in both directions
```r
df %>%
  dplyr::group_by(group) %>%
  tidyr::fill(count, .direction = "downup") %>%
  dplyr::ungroup()
```

### Remove columns with all NAs
```r
df %>% dplyr::select_if(~ !all(is.na(.)))
```

## Deal with date format

```r title='change all columns contain "DATE" as date format'
dplyr::mutate_at(
      vars(contains("DATE")),
      ~ as.Date(.x, format = "%Y-%m-%d")
      )
```

## Select columns
```r
dplyr::select(matches('date|dt_tm'))
```

## Summarize Columns
Calculate the number of records with death date later than the current date, the maximum and minimum for age and death date, and number of records with death age over 100
```r

df <- tibble::tibble(id = c(1L, 2L, 3L, 4L),
             death_date = c('2025-01-03', '2025-01-04', '2025-01-05', '2099-01-01'), 
             age = c(20, 50, 60, 125))

df %>% 
  dplyr::summarise(
    dplyr::across(c(death_date), 
           ~ sum(.x > Sys.Date(), na.rm = T),
           .names = 'count_{col}'),
    dplyr::across(c(death_date, age), 
           ~ as.character(min(.x, na.rm = T)),
           .names = 'min_{col}'),
    dplyr::across(c(death_date, age), 
           ~ as.character(max(.x, na.rm = T)),
           .names = 'max_{col}'),
    dplyr::across(c(age), 
           ~ sum(.x > 100, na.rm = T),
           .names = 'count_{col}'),
  ) %>% 
  tidyr::pivot_longer(
    cols = everything(),
    names_pattern = '(min|max|count)_(.*)',
    names_to      = c('.value', 'date_type')
  )
```

## Reshape data
###  Pivot data from wide to long

```r
tib <- tibble::tibble(type = c(1L, 1L, 1L, 2L, 2L, 2L), 
              id = c(1L, 2L, 3L, 1L, 2L, 3L), 
              age2000 = c(20L, 35L, 24L, 32L, 66L, 14L), 
              age2001 = c(21L, 36L, 25L, 33L, 67L, 15L),
              age2002 = c(22L, 37L, 26L, 34L, 68L, 16L),
              bool2000 = c(1L, 2L, 1L, 2L, 2L, 1L),
              bool2001 = c(1L, 2L, 1L, 2L, 2L, 1L),
              bool2002 = c(1L, 2L, 1L, 2L, 2L, 1L))

tidyr::pivot_longer(tib,
             cols = -c(id, type), # (1)
             names_to = c('.value', 'year'), # (2)
             names_pattern = '([a-z]+)(\\d+)') # (3)
```

1.  Specifies that all columns except id and type should be pivoted (these columns remain unchanged).
2.  This splits the column names into two new variables:
    * '.value': The part of the column name that matches the pattern ([a-z]+) (e.g., "age" or "bool") becomes the new "value" column names (age, bool).
    * 'year': The numeric part matching (\\d+) (e.g., "2000", "2001") is extracted into a new column named year.
3.  Defines the pattern for splitting column names. It looks for:
    * A sequence of lowercase letters ([a-z]+)
    * A sequence of digits (\\d+)

### split date into a list by a column value

```r
df %>%
  # split by indicator and set name
  dplyr::group_split(
    indicator, # (1)
    .keep = T  # (2)
  ) %>%
  # set names
  purrr::set_names(
    purrr::map_chr(
      ., 
      ~ .x$indicator[1] # (3)
      )
  )
```

1.  the grouped variable
2.  `TRUE` if want to keep the grouped variable
3.  Set the name of data within the list

## Rename columns

### Add suffix for multiple column names
```r
dplyr::rename_at(vars(BC:Interior),function(x) paste0(x,"_24"))
dplyr::rename_at(vars(-class), ~ paste0(., "_2014"))
```

### Change all column names to title style except those start with hsda
···r
df %>% dplyr::rename_with(str_to_title, !starts_with("hsda"))
···

### Add prefix to columns that start with "nonexistent"
```r
rename_with(
  iris,
  ~ paste0("prefix_", .x, recycle0 = TRUE),
  starts_with("nonexistent")
)
```

### Rename columns with names ending in "test" to "Test".
```r
dplyr::rename_with(df, ~ 'Test', matches('test$'))
```