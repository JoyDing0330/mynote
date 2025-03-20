## Implementation

### Run multiple functions with the same parameters
```r
x <- c(runif(10), NA)
funs <- c("mean", "median", "sd")

purrr::map_dbl(funs, exec, x, na.rm = TRUE)
```

## Functions
### pluck()
get or set an element deep within a nested data structure
```r
x <- list(
  list("a", list(1, elt = "foo")),
  list("b", list(2, elt = "bar"))
  )

pluck(x, 1, 2) # (1)
```

1.  Same as `x[[1]][[2]]`


### compact()
compact() discards elements where .p evaluates to an empty vector
```r
list(a = "a", b = NULL, c = integer(0), d = NA, e = list()) %>%
  compact()
```

### flatten()
Flattening a list removes a single layer of internal hierarchy, i.e. it inlines elements that are lists leaving non-lists alone.
```r
list(1, list(), 2, list(3)) |> list_flatten()
```