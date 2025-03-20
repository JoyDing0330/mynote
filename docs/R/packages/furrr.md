The combination of map and future (parallel processing)

### future::plan()
specifies how future():s are resolved, e.g. sequentially or in parallel.
```r
plan(sequential) # (1)
plan(multisession, workers = 3) # (2)
```

1.  sequential: Resolves futures sequentially in the current R process
2.  multisession: Resolves futures asynchronously (in parallel) in separate R sessions running in the background on the same machine

### implementation examples
```r
df <- data.frame(
  x = c("apple", "banana", "cherry"),
  pattern = c("p", "n", "h"),
  replacement = c("x", "f", "q"),
  stringsAsFactors = FALSE
)

future_pmap(df, gsub)
future_pmap_chr(df, gsub)
```

### close open connections
```r
if (!inherits(plan(), "sequential")) plan(sequential)
```