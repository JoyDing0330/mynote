## Tidy dots features
```r
my_cases <-   rlang::quos(Species == "setosa" ~ "S", 
                    TRUE ~ "other")

out <-  tiris %>% 
            mutate(new_lable = case_when(!!! my_cases))
```

## Create a new environment

```r
e1 <- rlang::env(a = 1, b = 2, c = 3)
e1$a
```

```r title='put a list into an environment'
objs <- list(b = "foo", c = "bar")
env <- env(a = 1, !!! objs)
env$c
```

```r title='assign values with the definition operator :='
var <- "a"
env <- env(!!var := "A")
env$a
```

## Execute functions
```r
exec("mean", x = 1:10, na.rm = TRUE, trim = 0.1)
```

```r title='Using dots features'
args <- list(x = 1:10, na.rm = TRUE, trim = 0.1)
exec("mean", !!!args)
```

``` r title = 'assign values with the definition operator :='
arg_name <- "na.rm"
arg_val <- TRUE
exec("mean", 1:10, !!arg_name := arg_val)
```