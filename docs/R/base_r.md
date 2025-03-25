## `all()`
Given a set of logical vectors, are all of the values true?
```r
if (all(cols_distinct %in% colnames(df_linelist)))
```

## `.libPaths()`
```r
if (!lib_path %in% .libPaths()) .libPaths(lib_path)
```

### `assign()`
```r
assign(
    ha_name[i],
    read_excel(
        paste(folder, files_to_read[i], sep= "/"),
        sheet=1,
        guess_max = 100000
        )%>%
        clean_names()
        )
```
```r
c("ha", "hsda", "lha") %>%
  walk(
    .,
    ~ eval(parse(text = paste0(.x, "_map$region_name"))) %>%
      unique %>%
      sort %>%
      assign(
        x     = paste0("all_", .x),
        value = .,
        envir = .GlobalEnv
      )
  )
```

## `na.omit()`
removes all incomplete cases of a data object
```r
data <- data.frame(x1 = c(9, 6, NA, 9, 2, 5, NA),     
                   x2 = c(NA, 5, 2, 1, 5, 8, 0),      
                   x3 = c(1, 3, 5, 7, 9, 7, 5))
na.omit(data) 
```

## `basename()` and `dirname()`
```r
basename(file.path("","p1","p2","p3", c("file1", "file2")))
dirname(file.path("","p1","p2","p3","filename"))
```

## `list.files()`
```r title='load everything that follows a name pattern'
here::here('script/funcs') %>% 
  list.files('.R$', full.names = T) %>% 
  walk(source, local = globalenv())

sapply(
  list.files(
    here::here('B_script/funcs'),
    pattern = 'R$',
    full.names = T
  ),
  source
)
```

```r
file <- file_path %>%
    list.files(
        pattern    = file_name_pattern,
        full.names = T
        ) %>% 
        sort(decreasing = T) %>% 
        file.info() %>% 
        mutate(
          .keep = 'none',
          path  = rownames(.),
          name  = basename(path),
          ctime = ctime,
        ) %>% 
        filter(as.Date(ctime) == Sys.Date())
```

## `attach()`
```r title='load data into a deparate environment'
attach(here::here(df_path.RData), name = 'temp_env')
```

## Exit handler: `on.exit()` 
the exit handler is run regardless of whether the function exits normally or with an error.
```r
j06 <- function(x) {
  cat("Hello\n")
  on.exit(cat("Goodbye!\n"), add = TRUE)
  
  if (x) {
    return(10)
  } else {
    stop("Error")
  }
}
```