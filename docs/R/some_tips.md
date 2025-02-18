## Rewriting to prefix form
```r
x + y
`+`(x, y)

names(df) <- c("x", "y", "z")
`names<-`(df, c("x", "y", "z"))

for(i in 1:10) print(i)
`for`(i, 1:10, print(i))

`%+%` <- function(a, b) paste0(a, b)
"new " %+% "string"

```