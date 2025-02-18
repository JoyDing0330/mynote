## Regular expression

```r title='Extract anything between certain words'
stringr::str_extract_all(
          string  = .x,
          pattern = "(?<=using last ).+(?= days)" # between the word 'using last ' and ' days'
        )
```

```r title='Replace the last specific operator'
sub('-[^-]*$', '', sentence) # substitute the last - with ''(nothing)
#sub by matching a - followed by zero or more characters that are not a - till the end ($) of the string and replace it with blank ('')
```

```r title='Extract date value'
str_extract(
          string = .x,
          pattern = "\\d{2}(-/)\\d{2}(-/)\\d{4}"
        )
```

## Replace 
```r
# Replace multiple strings at a time
rep_str = c('St'='Street','Blvd'='Boulevard','Pkwy'='Parkway')
df$address <- str_replace_all(df$address, rep_str)
```

## String pad
Pad a string to a fixed width.
```r
rbind(
  str_pad(string = "hadley", width = 30, side = "left",pad = "."),
  str_pad("hadley", 30, "right"),
  str_pad("hadley", 30, "both")
)

# All arguments are vectorised except side
str_pad(c("a", "abc", "abcdef"), 10)
str_pad("a", c(5, 10, 20))
str_pad("a", 10, pad = c("-", "_", " "))
```

```r
data %>% pwalk(
        .,
        ~ cat(str_pad(..2, 18, side = 'right', '.'), ..3, '\n') 
        #..2 = col #2 as the string, side = put pad at right
      )
```