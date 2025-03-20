## Run multiple functions with the same parameters
```r
x <- c(runif(10), NA)
funs <- c("mean", "median", "sd")

purrr::map_dbl(funs, exec, x, na.rm = TRUE)
```


## pluck()
```r
x <- list(
  list("a", list(1, elt = "foo")),
  list("b", list(2, elt = "bar"))
  )

pluck(x, 1, 2) # (1)
```

1.  Same as `x[[1]][[2]]`