## Format numbers

```r
sprintf("%02.1f%%", your_number) # (1)
# Output: "xxxx.x%"

sprintf("Pi to 3 decimal places: %.3f", pi)
# Output: "Pi to 3 decimal places: 3.142"

sprintf("Scientific notation of Pi: %e", pi) # (2)
# Output: "Scientific notation of Pi: 3.141593e+00"

sprintf("%s is %d years old", "Alice", 25) # (3)
# Output: "Alice is 25 years old"
```

1.  * %%: Literal %
    * %f: Double precision value in fixed-point notation
2.  %e, %E: Double precision value in exponential notation
3.  * %d, %i: Integer value
    * %s: Character string

## Regular expression

```r title='Extract anything between certain words'
stringr::str_extract_all(
          string  = .x,
          pattern = "(?<=using last ).+(?= days)" # between the word 'using last ' and ' days'
        )
```

```r title='substitute the last dash with nothing'
sub('-[^-]*$', '', sentence) # (1)
```

1.  matching a dash followed by zero or more characters that are not a dash till the end ($) of the string and replace it with blank ('')

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